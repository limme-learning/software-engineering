# Khmer Terminology Glossary — Software Engineering Knowledge Base

**Purpose:** one locked-down Khmer rendering per recurring term, so 92 files
translated across many sessions don't each invent a different word for the
same concept. Every translation phase in `TODO-khmer-translation.md` must
use this file, not improvise.

> ⚠️ **This glossary has not had a native-speaker review pass yet.** The
> Khmer here is my best-effort rendering, checked for correctness and
> natural phrasing to the extent I can verify it, but Khmer technical
> vocabulary is not something to fully trust from a single non-native
> drafting pass. Before Step 1 of the translation TODO begins in earnest,
> get 10–15 minutes from a fluent Khmer speaker (ideally one working in
> tech) to read through Tier 1 below and flag anything that sounds stiff,
> wrong, or like it's translating word-for-word instead of meaning-for-
> meaning. That review is cheap now and expensive after 92 files use the
> term you'd have corrected.

## How to use this file

Two tiers, and the difference matters:

- **Tier 1 — translate into Khmer.** These are concepts that have a
  natural, commonly understood Khmer rendering — the kind of word a
  Khmer-speaking engineer would actually use in conversation or writing,
  not an invented calque that only makes sense on paper.
- **Tier 2 — keep in English/Latin script.** Product names, acronyms, and
  jargon that Cambodian engineers say in English even when the surrounding
  sentence is otherwise Khmer. Forcing a Khmer rendering here produces
  text that is *technically* translated and *actually* harder to read —
  exactly what the "easy to read" requirement is trying to avoid.

When in doubt between the two, default to Tier 2. An unnatural forced
translation is worse than an English loanword in an otherwise-Khmer
sentence — that mixing is completely normal in Khmer technical registers
(the same way "computer" and "internet" are just said in English inside
Khmer speech).

**Style note — how a sentence should look:**
```
English:  Run the migration in staging before you touch production.
Khmer:    ត្រូវតែដំណើរការ migration នៅ staging ជាមុន មុននឹងប៉ះពាល់ production។
```
Notice `migration`, `staging`, and `production` stay English (Tier 2 —
environment names and a specific technical mechanism), while the sentence
structure, verbs, and connecting words are natural Khmer.

---

## Tier 1 — Translate These

| English | Khmer | Usage note |
|---|---|---|
| database | មូលដ្ឋានទិន្នន័យ | Standard, textbook-correct, used everywhere in Khmer tech education |
| data | ទិន្នន័យ | |
| deployment / deploy | ការដាក់ឱ្យដំណើរការ / ដាក់ឱ្យដំណើរការ | "Putting into operation" — natural for the general concept |
| release (noun/verb) | ការចេញផ្សាយ / ចេញផ្សាយ | Same word used for publishing a product |
| rollback | ត្រឡប់ក្រោយ | "Revert back" — intuitive, correctly conveys direction |
| rollback plan | ផែនការត្រឡប់ក្រោយ | |
| incident | ឧប្បត្តិហេតុ | Formal but correct and standard in operational contexts |
| root cause | មូលហេតុដើម | "Original cause" |
| audit | សវនកម្ម | Standard finance/compliance Khmer term, correctly reused for tech audits |
| compliance | ការអនុលោមភាព | Standard regulatory Khmer term (used in banking/insurance documents already) |
| stakeholder | ភាគីពាក់ព័ន្ធ | "Related/involved party" — standard business Khmer |
| requirement | តម្រូវការ | Very common, correct |
| acceptance criteria | លក្ខខណ្ឌទទួលយក | "Acceptance conditions" |
| version | កំណែ | Standard, used for app/software versions in everyday Khmer already |
| staging (environment, general concept) | បរិយាកាសសាកល្បង | Use when explaining the *concept*; keep `staging` in English when naming the actual environment (see Tier 2 note) |
| production (environment, general concept) | បរិយាកាសផលិតកម្ម | Same split as staging — concept vs. named environment |
| service | សេវា | Correct for "a service" in an architecture sense |
| testing / test | ការសាកល្បង | |
| session | សម័យ | "Login session" → សម័យចូលប្រើ |
| contract (API contract, in explanatory prose) | កិច្ចសន្យា | Metaphorical use of the legal-contract word; understood in business Khmer |
| breaking change | ការផ្លាស់ប្តូរដែលធ្វើឱ្យខូច | "A change that causes breakage" — descriptive, avoid over-compressing this into a stiffer calque |
| backward compatible | មានភាពឆបគ្នាថយក្រោយ | |
| dependency (general/architectural sense) | ភាពអាស្រ័យ | "Reliance/dependence" |
| coupling | ភាពជាប់ទាក់ទងគ្នា | "Interconnectedness" — use this for the general concept, not as a 1:1 word substitution in every sentence |
| observability | សមត្ថភាពសង្កេតមើល | "Capability to observe" — a little formal but correct and clear |
| log / logging | កំណត់ត្រា | Standard word for "record," correctly extends to "log" |
| trace / tracing | ការតាមដាន | "Tracking/following" |
| metric | សូចនាករ | "Indicator" — genuinely the right natural fit, already used for KPIs |
| alert | ការជូនដំណឹង | "Notification" — very natural, widely used |
| retry | ព្យាយាមម្តងទៀត | "Try again" |
| timeout | ការអស់ពេល | "Running out of time" |
| code review | ការត្រួតពិនិត្យកូដ | `កូដ` (code) is itself an absorbed loanword — expected and fine |
| technical debt | បំណុលបច្ចេកទេស | Direct calque, but this one *is* the standard way it's rendered in Khmer business/tech writing — keep it |
| constraint (database) | លក្ខខណ្ឌកំហិត | "Restrictive condition" |
| index (database) | សន្ទស្សន៍ | Same word as a book index — correct and standard |
| partition (database) | ការចែកជាភាគ | "Division into parts" |
| cache invalidation (the concept, in prose) | ការធ្វើឱ្យលែងមានសុពលភាព | "Making no longer valid" — translate the concept; keep `cache` itself in English (Tier 2) |
| business domain (business sense, not the DDD "domain layer") | វិស័យអាជីវកម្ម | Only for the business-sector sense — see Tier 2 for the architectural sense |

---

## Tier 2 — Keep in English/Latin Script

**Products, platforms, and named tools** (no ambiguity, never translate):
API, OAuth, OpenID Connect (OIDC), JWT, JWKS, PKCE, SSO, REST, GraphQL,
gRPC, SQL, PostgreSQL, MySQL, JSON, YAML, XML, HTTP, HTTPS, TCP/IP, Docker,
Kubernetes, React, Next.js, Vue, Nuxt, Spring Boot, Keycloak, TanStack
Query, Zod, nuqs, Orval, Motion, Tailwind CSS, Liquibase, Testcontainers,
WireMock, MSW, Playwright, Cypress, Pact, CI/CD, ADR.

**Compliance/regulatory acronyms** (these are the terms of art; a Khmer
paraphrase would be less precise, not more readable):
PCI-DSS, GDPR, SOC 2, KYC, VPAT, WCAG, SLA, SLO.

**Architecture and pattern names** (specific technical terms with no
natural Khmer equivalent in actual use — translating them produces
academic-sounding Khmer nobody in the industry would recognise):
monolith, microservice, domain (in the DDD/hexagonal "domain layer" sense —
distinct from the Tier 1 "business domain" entry above), port, adapter,
circuit breaker, idempotency / idempotent, feature flag, endpoint, token,
cache (as a noun — "cache invalidation" as a concept is Tier 1 above, but
the word `cache` itself stays English), migration (the database-schema
sense — "run the migration" stays as `migration`), pull request / PR,
mock / mocking, fixture, regression (test), N+1 (the query problem), RBAC,
ABAC, CRUD, DTO, ORM, IDOR, runbook.

**Rule of thumb for anything not listed here:** if a Cambodian engineer
would say the English word out loud in an otherwise-Khmer sentence, it's
Tier 2. If they'd naturally reach for a Khmer phrase, it's Tier 1. When
translating a new file surfaces a term not in this list, add it here
first, then use it — don't decide ad hoc inside a single article.

---

## Growing this list

This is a living document. As translation proceeds domain by domain
(`TODO-khmer-translation.md`), any term that comes up repeatedly and isn't
here yet gets added — to this file, not invented locally inside one
article. If the same English term ends up with two different Khmer
renderings in two different domains, that is the specific failure mode
this glossary exists to prevent; catching it means fixing the glossary and
then revisiting every file that used the wrong one.
