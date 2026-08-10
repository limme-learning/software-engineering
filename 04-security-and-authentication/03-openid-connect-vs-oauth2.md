---
title: "ID Token or Access Token: The Distinction That Breaks SaaS Authentication"
author: Mengty LIM
category: 04-security-and-authentication
last_updated: 2026
---

# ID Token or Access Token: The Distinction That Breaks SaaS Authentication

OAuth 2.0 was never designed to tell you who a user is — it tells a resource server what a client may do. OpenID Connect is the thin, precisely specified layer that adds identity, and the two tokens it leaves you holding are not interchangeable.

## The Real-World Problem

An enterprise SaaS vendor sells a workforce-planning platform to 400 corporate customers, each federating through their own identity provider — Entra ID, Okta, Ping, a couple of self-hosted Keycloak realms. The platform's API accepts a bearer token, decodes it, reads `email`, and looks up the user.

Two defects lived in that one sentence for eighteen months.

First, the API accepted **ID tokens** as API credentials, because a frontend developer found that the ID token conveniently contained `email` and the access token did not. ID tokens are issued for the client, with `aud` set to the client ID, and are frequently handled less carefully than access tokens — logged in browser consoles, attached to analytics payloads, passed through to third-party widgets. Any of those leaks became full API access.

Second, the lookup used `email` as the primary identity key. One customer's IdP allowed users to self-service change their email. A departing employee changed theirs to a colleague's decommissioned address that had been recycled into a new hire's mailbox, and the SaaS platform matched them to a records-retention account with elevated visibility across the customer's whole headcount data.

The final failure was operational rather than conceptual. The platform cached each tenant's JWKS on startup and never refetched it. When one customer rotated their signing key on a scheduled 90-day cycle, every request from that tenant failed with an invalid-signature error, and the on-call engineer's fix was a rolling restart at 02:00. It worked, which meant the underlying bug survived another quarter.

The fix was three rules: never accept an ID token at an API, key identity on `iss` + `sub`, and refetch JWKS on an unknown `kid`.

## The Concept

### Authentication vs. authorization, and which protocol does which

| | OAuth 2.0 | OpenID Connect |
|---|---|---|
| **Question answered** | May this client do this? | Who is this user, and when and how did they authenticate? |
| **Primary artifact** | Access token | ID token |
| **Consumed by** | Resource server (API) | The client that requested the login |
| **Format** | JWT or opaque — the client must not inspect it | Always a signed JWT — the client must validate it |
| **Standardised claims** | No (except `scope`, `client_id` in introspection) | Yes: `iss`, `sub`, `aud`, `exp`, `iat`, `nonce`, `auth_time`, `acr`, `amr`, `at_hash` |

OIDC is OAuth 2.0 plus the `openid` scope, plus an ID token, plus discovery, plus a UserInfo endpoint. It is not an alternative to OAuth — it is a profile of it.

### The two tokens, and the four rules

| Token | Audience (`aud`) | Purpose | Never do this |
|---|---|---|---|
| **ID token** | The client ID | Tell the client a login happened, who it was, and how | Never send it to an API. Never accept it at an API. Never use it as a session token |
| **Access token** | The resource server(s) | Authorize API calls | Never parse it in the frontend to read user attributes. Never assume it contains identity claims |

The rules stated plainly:

1. **An API accepts access tokens only.** Validate `aud` contains your resource server identifier and reject `typ: ID`.
2. **A client validates ID tokens only.** Validate signature, `iss`, `aud == client_id`, `exp`, and `nonce`.
3. **Frontends read identity from the ID token or UserInfo**, never by decoding an access token — access tokens are opaque *by contract* even when they happen to be JWTs, and the issuer is free to change the format.
4. **Identity is `iss` + `sub`, never `email`.** `sub` is stable and issuer-scoped; `email` is mutable, reassignable, and not guaranteed unique across tenants.

### `nonce` and `at_hash`: what they actually defend

**`nonce`** — the client generates a random value, sends it on the authorization request, and requires the returned ID token to contain the same value. This prevents ID token replay: an ID token captured from another session cannot be injected into this one, because the nonce will not match the one this client generated. `state` protects the *request* against CSRF; `nonce` protects the *token* against replay. You need both.

**`at_hash`** — the left-most half of the SHA-256 of the access token, base64url-encoded, embedded in the ID token. It binds the two tokens together so an attacker cannot pair a legitimate ID token with a substituted access token. It is mandatory when the ID token and access token arrive from the authorization endpoint in the same response, and worth checking whenever it is present.

### Claims: where they live and why the access token stays thin

| Need | Get it from |
|---|---|
| Display name, avatar, locale for the UI | ID token or UserInfo |
| Authorization decisions in the API | Access token `scope` and roles |
| Rarely-needed, large, or PII-heavy attributes | UserInfo endpoint, on demand |
| A stable internal user identifier | `iss` + `sub`, mapped once at provisioning time to your own user ID |

Keep access tokens small. Every claim you add is sent on every request to every service, appears in every log that captures headers, and inflates the token past proxy header limits — an 8 KB access token stuffed with group memberships is a real production failure mode, not a hypothetical one.

### Discovery and JWKS rotation

The discovery document at `/.well-known/openid-configuration` gives you every endpoint and the JWKS URI. Hardcoding endpoints instead of discovering them is how estates break during an IdP upgrade.

JWKS rotation is the operational half nobody tests. The correct client behaviour:

- Cache the key set, honouring `Cache-Control`.
- On a token whose `kid` is not in the cache, refetch **once**, rate-limited (never per request — that is a self-inflicted denial of service against your IdP).
- Support multiple active keys simultaneously, because rotation publishes the new key before it starts signing with it.
- Never pin a single key or a certificate. Never disable signature verification to get past an incident.

Spring Security's `NimbusJwtDecoder.withIssuerLocation(...)` does all of this correctly. The SaaS outage above happened because the team wrote their own decoder.

## How It Works

```mermaid
sequenceDiagram
    autonumber
    actor U as Corporate User
    participant SPA as SaaS Frontend
    participant BFF as SaaS BFF<br/>(confidential client)
    participant IDP as Customer IdP<br/>(Entra / Okta / Keycloak)
    participant API as Planning API<br/>(resource server)

    U->>SPA: Open app
    SPA->>BFF: /login?tenant=acme
    BFF->>IDP: GET /.well-known/openid-configuration
    IDP-->>BFF: endpoints + jwks_uri
    BFF->>BFF: state, nonce, PKCE verifier
    BFF-->>U: 302 /authorize?scope=openid profile<br/>planning:read&nonce=...&code_challenge=...
    U->>IDP: Authenticate (SSO + MFA)
    IDP-->>BFF: 302 /callback?code=...&state=...
    BFF->>IDP: POST /token (code + verifier)
    IDP-->>BFF: id_token + access_token + refresh_token

    Note over BFF: Validate ID TOKEN:<br/>sig, iss, aud==client_id,<br/>exp, nonce, at_hash

    BFF->>BFF: Resolve user by (iss, sub)<br/>→ internal user_id
    BFF-->>SPA: Set-Cookie: session (HttpOnly, SameSite=Lax)

    SPA->>BFF: GET /api/headcount
    BFF->>API: GET /v1/headcount<br/>Authorization: Bearer <access_token>

    Note over API: Validate ACCESS TOKEN:<br/>sig, iss, aud==planning-api,<br/>exp, scope. Reject typ=ID.

    API->>IDP: GET /jwks (cached; refetch on unknown kid)
    API-->>BFF: 200 data
    BFF-->>SPA: 200 data
```

The two validation boxes are deliberately different. The BFF never forwards the ID token, and the API never accepts one.

## Practical Example

**The discovery document, trimmed to what matters:**

```bash
curl -s https://id.acme.example/realms/workforce/.well-known/openid-configuration | jq
```

```json
{
  "issuer": "https://id.acme.example/realms/workforce",
  "authorization_endpoint": "https://id.acme.example/realms/workforce/protocol/openid-connect/auth",
  "token_endpoint": "https://id.acme.example/realms/workforce/protocol/openid-connect/token",
  "userinfo_endpoint": "https://id.acme.example/realms/workforce/protocol/openid-connect/userinfo",
  "jwks_uri": "https://id.acme.example/realms/workforce/protocol/openid-connect/certs",
  "end_session_endpoint": "https://id.acme.example/realms/workforce/protocol/openid-connect/logout",
  "introspection_endpoint": "https://id.acme.example/realms/workforce/protocol/openid-connect/token/introspect",
  "scopes_supported": ["openid", "profile", "email", "planning:read", "planning:write"],
  "id_token_signing_alg_values_supported": ["RS256", "ES256"],
  "code_challenge_methods_supported": ["S256"],
  "claims_supported": ["sub", "iss", "aud", "exp", "iat", "nonce", "at_hash", "acr", "amr",
                       "auth_time", "preferred_username", "email", "email_verified", "name"]
}
```

**The ID token payload — for the client, about the login:**

```json
{
  "iss": "https://id.acme.example/realms/workforce",
  "aud": "workforce-planning-bff",
  "sub": "b2f4d9a1-7c33-4e50-9d61-0a8f5c2e7b14",
  "exp": 1786315200,
  "iat": 1786314900,
  "auth_time": 1786314880,
  "nonce": "n-0S6_WzA2Mj",
  "at_hash": "77QmUPtjPfzWtF2AnpK9RQ",
  "acr": "urn:mace:incommon:iap:silver",
  "amr": ["pwd", "mfa", "hwk"],
  "sid": "3c9e1d77-2a5b-4f80-9e3d-71f0b6c4a852",
  "preferred_username": "j.okafor",
  "email": "j.okafor@acme.example",
  "email_verified": true,
  "name": "Jide Okafor"
}
```

**The access token payload — for the API, about permissions. Note there is no `email`:**

```json
{
  "iss": "https://id.acme.example/realms/workforce",
  "aud": ["planning-api"],
  "azp": "workforce-planning-bff",
  "sub": "b2f4d9a1-7c33-4e50-9d61-0a8f5c2e7b14",
  "exp": 1786315200,
  "iat": 1786314900,
  "typ": "Bearer",
  "scope": "openid profile planning:read",
  "acr": "urn:mace:incommon:iap:silver",
  "tenant_id": "acme",
  "resource_access": { "planning-api": { "roles": ["planner"] } }
}
```

**Client side — Spring Security 6 OIDC login with `nonce` and `at_hash` validated, and identity keyed on `iss` + `sub`:**

```java
@Configuration
@EnableWebSecurity
class OidcClientConfig {

    @Bean
    SecurityFilterChain web(HttpSecurity http, OidcUserService delegate) throws Exception {
        http
            .authorizeHttpRequests(a -> a
                .requestMatchers("/", "/health", "/signed-out").permitAll()
                .anyRequest().authenticated())
            .oauth2Login(login -> login
                // Spring generates and validates nonce, state, PKCE, and at_hash
                // as part of OidcAuthorizationCodeAuthenticationProvider. Do not reimplement it.
                .userInfoEndpoint(ui -> ui.oidcUserService(delegate)))
            .logout(l -> l.logoutSuccessHandler(oidcLogoutSuccessHandler()));
        return http.build();
    }
}

@Service
class TenantAwareOidcUserService extends OidcUserService {

    private final UserAccountRepository accounts;

    TenantAwareOidcUserService(UserAccountRepository accounts) { this.accounts = accounts; }

    @Override
    public OidcUser loadUser(OidcUserRequest request) {
        var oidcUser = super.loadUser(request);
        var idToken = oidcUser.getIdToken();

        // Identity key is (issuer, subject). NOT email — email is mutable and reassignable.
        var federatedId = new FederatedIdentity(
            idToken.getIssuer().toString(),
            idToken.getSubject());

        var account = accounts.findByFederatedIdentity(federatedId)
            .orElseGet(() -> accounts.provision(federatedId, idToken.getPreferredUsername()));

        // Email is treated as a mutable profile attribute, synchronised, never used to match.
        account.syncProfile(idToken.getFullName(), idToken.getEmail());

        return new PlatformOidcUser(account, oidcUser);
    }
}
```

**Resource server side — reject ID tokens explicitly. This is the check that would have prevented the incident:**

```java
@Bean
JwtDecoder planningApiDecoder(@Value("${oidc.issuer}") String issuer) {
    var decoder = NimbusJwtDecoder
        .withIssuerLocation(issuer)                 // discovery + JWKS caching + kid refetch
        .jwsAlgorithms(algs -> { algs.add(SignatureAlgorithm.RS256);
                                 algs.add(SignatureAlgorithm.ES256); })
        .build();

    decoder.setJwtValidator(new DelegatingOAuth2TokenValidator<>(
        JwtValidatorFactory.createDefaultWithIssuer(issuer),
        // An ID token has aud == client_id and typ == ID. Both checks reject it.
        new JwtClaimValidator<List<String>>("aud",
            aud -> aud != null && aud.contains("planning-api")),
        new JwtClaimValidator<String>("typ", "Bearer"::equals),
        // Step-up: a token issued without MFA cannot reach write endpoints.
        new JwtClaimValidator<String>("acr", Objects::nonNull)
    ));
    return decoder;
}
```

```java
@Test
void idToken_isRejectedAtTheApi() throws Exception {
    var idToken = tokens.idTokenFor("j.okafor", "workforce-planning-bff");   // aud = client id

    mockMvc.perform(get("/v1/headcount").header("Authorization", "Bearer " + idToken))
        .andExpect(status().isUnauthorized())
        .andExpect(header().string("WWW-Authenticate", containsString("invalid_token")));
}

@Test
void unknownKid_triggersSingleJwksRefetch_notOnePerRequest() {
    idp.rotateSigningKey();
    var token = tokens.accessTokenSignedWithNewKey();

    IntStream.range(0, 50).forEach(i -> assertThat(decoder.decode(token)).isNotNull());

    assertThat(idp.jwksRequestCount()).isEqualTo(2);   // initial + one rotation refetch
}
```

**UserInfo, for attributes that do not belong in a token:**

```bash
curl -s https://id.acme.example/realms/workforce/protocol/openid-connect/userinfo \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" | jq
```

```json
{
  "sub": "b2f4d9a1-7c33-4e50-9d61-0a8f5c2e7b14",
  "preferred_username": "j.okafor",
  "name": "Jide Okafor",
  "email": "j.okafor@acme.example",
  "cost_centre": "CC-4471",
  "line_manager_sub": "9a1e7f22-4b6c-4d13-8e05-c7b3a2f61d90"
}
```

## Enterprise Trade-offs

| Approach | Pros | Cons | When to use (Banking / Insurance / Enterprise) |
|---|---|---|---|
| **OIDC for login + OAuth access tokens for APIs** | Correct separation; standard claims; federation across customer IdPs is configuration | Two tokens to reason about; teams must be taught the distinction | Default for every user-facing enterprise system |
| **Claims embedded in the access token** | Zero extra calls per request; works offline of the IdP | Stale until expiry; token bloat; PII spread across every service log | Small, stable, non-sensitive claims: tenant, roles, `acr` |
| **UserInfo call per session** | Fresh data; keeps PII out of tokens and logs | Extra round trip; IdP availability becomes a login dependency | Large or sensitive profiles: broker hierarchies, HR attributes, medical flags |
| **Opaque access tokens + introspection** | Immediate revocation; token reveals nothing if leaked | Network call per request unless cached; IdP is on the hot path | Public and partner-facing tokens; high-value operations such as payment initiation |
| **Email as the user key** | Human-readable; easy joins | Mutable, reassignable, not unique across tenants — the scenario above | Never. Key on `iss` + `sub`, keep email as a synchronised attribute |
| **Accepting ID tokens at an API** | "It already has the email in it" | ID tokens are handled loosely by clients; audience confusion is full account takeover | Never |

## Why This Still Matters Through 2030

OIDC is the settled standard for enterprise federation and shows no sign of being replaced — the active work is layered on top of it rather than beside it. Verifiable-credential and digital-identity-wallet schemes, including the European regimes now coming into force, use OIDC-derived protocols for issuance and presentation. Fine-grained authorization systems consume OIDC identity as their subject input. Agentic and delegated-access patterns need exactly what OIDC provides: a stable subject, an assertion of *how* and *when* authentication happened via `acr`, `amr`, and `auth_time`, and a discoverable trust anchor. Multi-tenant SaaS makes the discipline non-negotiable, because every additional federated customer multiplies the cost of keying identity on a mutable attribute or of caching a key set forever. The distinction at the heart of this article is not a subtlety to be papered over by a library: one token tells your application who logged in, the other tells someone else's application what you may do, and the day a team forgets which is which they ship an authentication bypass.

→ Next: [04-keycloak-realms-clients-and-roles.md](04-keycloak-realms-clients-and-roles.md) · Related: [02-oauth21-whats-new-and-why-it-matters.md](02-oauth21-whats-new-and-why-it-matters.md) · [../01-core-concepts/05-security-by-default.md](../01-core-concepts/05-security-by-default.md)
