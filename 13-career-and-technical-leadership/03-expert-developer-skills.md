---
title: "The Vertical Stroke: What Expert Actually Means in 2026"
author: Mengty LIM
category: 13-career-and-technical-leadership
last_updated: 2026
---

# The Vertical Stroke: What Expert Actually Means in 2026

Depth is the part of the T that everything else rests on, and it is the part most easily faked by a long list of technologies on a CV. This article turns "expert developer" into six sub-domains of checkable competence, each one wired to the material in this knowledge base that teaches it.

## The Real-World Problem

A multi-tenant enterprise SaaS company selling workforce-management software to logistics and retail customers has a hiring problem it has misdiagnosed. It keeps hiring engineers who interview well on algorithms and ship features quickly, and it keeps having the same three production incidents: a tenant seeing another tenant's data through a filter that was applied in the UI but not in the query; a nightly export that silently drops rows when a customer's timezone crosses a DST boundary; a release that cannot be rolled back because the migration dropped a column.

None of those are exotic failures. Each one is a gap in a specific sub-domain — authorisation enforced at the wrong layer, data-integrity assumptions untested, migrations run without an expand/contract discipline. The company's competency framework said "Strong in Java, React, and AWS," which is a list of tools, and tools are not what failed.

What the company actually needed was a checklist organised by *failure surface* rather than by technology, so that an engineer, a reviewer, and an interviewer could all point at the same gap.

## The Concept

Depth is not one list. It is six sub-domains that fail in different ways and are learned in different orders. Treat each as a checklist you can honestly tick, not as a set of technologies you have touched.

Grade yourself per item on three levels: **can recognise** it in someone else's code, **can implement** it correctly under normal conditions, **can teach and review** it — including catching the subtle wrong version. Working toward the horizontal stroke of the T only makes sense once most of this list is at the third level.

### Programming & Software Design

- [ ] Choose the right decomposition boundary — modules whose reasons to change are genuinely different
- [ ] Apply SOLID as a maintainability test, not a ceremony — see [`01-core-concepts/01-solid-and-separation-of-concerns.md`](../01-core-concepts/01-solid-and-separation-of-concerns.md)
- [ ] Resist premature abstraction; know when duplication is cheaper than the wrong shared type — see [`01-core-concepts/02-yagni-kiss-dry.md`](../01-core-concepts/02-yagni-kiss-dry.md)
- [ ] Reach for a design pattern only when the problem is genuinely the one the pattern solves — see [`02-design-patterns/04-when-not-to-use-a-pattern.md`](../02-design-patterns/04-when-not-to-use-a-pattern.md)
- [ ] Model errors deliberately: what is retryable, what is fatal, what must be idempotent — see [`01-core-concepts/03-failure-modes-and-resilience.md`](../01-core-concepts/03-failure-modes-and-resilience.md)
- [ ] Write a design document a reviewer can disagree with — options considered, decision, consequences
- [ ] Name and track technical debt as an explicit, costed item — see [`01-core-concepts/08-technical-debt-tracking.md`](../01-core-concepts/08-technical-debt-tracking.md)
- [ ] Read unfamiliar code faster than you rewrite it

**Architecture-level, once the above is solid:** layered, hexagonal, and clean architectures and the trade-off between them ([`03-architecture/01-layered-architecture.md`](../03-architecture/01-layered-architecture.md), [`02-hexagonal-architecture.md`](../03-architecture/02-hexagonal-architecture.md), [`03-clean-architecture.md`](../03-architecture/03-clean-architecture.md)); the monolith-versus-microservices decision on its actual criteria rather than fashion ([`04-microservices-vs-monolith.md`](../03-architecture/04-microservices-vs-monolith.md)); event-driven designs and the consistency cost they carry ([`05-event-driven-architecture.md`](../03-architecture/05-event-driven-architecture.md)).

### Frontend Engineering

- [ ] Compose components along data boundaries, not visual ones — see [`07-frontend-best-practices/01-react-component-architecture.md`](../07-frontend-best-practices/01-react-component-architecture.md)
- [ ] Choose a rendering strategy per route with a stated reason (SSR / SSG / ISR / server components) — see [`07-frontend-best-practices/02-nextjs-app-router-and-rendering-strategies.md`](../07-frontend-best-practices/02-nextjs-app-router-and-rendering-strategies.md)
- [ ] Work in at least one second framework well enough to judge trade-offs — see [`07-frontend-best-practices/03-vue-and-nuxt-composition-api.md`](../07-frontend-best-practices/03-vue-and-nuxt-composition-api.md)
- [ ] Build on design tokens rather than ad-hoc values — see [`09-ux-ui-guidelines/01-ui-consistency-and-design-tokens.md`](../09-ux-ui-guidelines/01-ui-consistency-and-design-tokens.md) and [`07-frontend-best-practices/04-styling-with-tailwind-css.md`](../07-frontend-best-practices/04-styling-with-tailwind-css.md)
- [ ] Treat accessibility as a correctness requirement, not a polish phase — see [`09-ux-ui-guidelines/02-accessibility-basics-wcag.md`](../09-ux-ui-guidelines/02-accessibility-basics-wcag.md)
- [ ] Design loading, empty, and error states before the happy path — see [`09-ux-ui-guidelines/04-loading-error-empty-states.md`](../09-ux-ui-guidelines/04-loading-error-empty-states.md)
- [ ] Animate only where it communicates state change — see [`07-frontend-best-practices/05-motion-for-purposeful-animation.md`](../07-frontend-best-practices/05-motion-for-purposeful-animation.md)
- [ ] Hold a performance budget and know what breaks it — see [`01-core-concepts/06-performance-budget.md`](../01-core-concepts/06-performance-budget.md)
- [ ] Never treat a UI-level check as an authorisation control

### Backend & Database Engineering

- [ ] Design REST resources, versioning, idempotency, and pagination you will not regret in two years — see [`05-apis-and-integration/01-rest-api-design-principles.md`](../05-apis-and-integration/01-rest-api-design-principles.md)
- [ ] Know when GraphQL earns its cost and where the N+1 lands — see [`05-apis-and-integration/02-graphql-core-concepts-and-tradeoffs.md`](../05-apis-and-integration/02-graphql-core-concepts-and-tradeoffs.md)
- [ ] Use gRPC/protobuf for internal service-to-service paths where it fits — see [`05-apis-and-integration/03-grpc-and-protobuf-for-internal-services.md`](../05-apis-and-integration/03-grpc-and-protobuf-for-internal-services.md)
- [ ] Generate typed clients from the contract instead of hand-writing fetch code — see [`05-apis-and-integration/04-orval-generating-typed-clients-from-openapi.md`](../05-apis-and-integration/04-orval-generating-typed-clients-from-openapi.md)
- [ ] Read an execution plan and design indexes from it — see [`06-database-strategies/01-indexing-strategies-that-actually-work.md`](../06-database-strategies/01-indexing-strategies-that-actually-work.md)
- [ ] Partition large tables on a key that matches the access pattern — see [`06-database-strategies/02-partitioning-for-scale.md`](../06-database-strategies/02-partitioning-for-scale.md)
- [ ] Denormalise deliberately, with a named consistency owner — see [`06-database-strategies/03-normalization-vs-denormalization.md`](../06-database-strategies/03-normalization-vs-denormalization.md)
- [ ] Keep DDL and DML ownership and audit trail separate — see [`06-database-strategies/04-ddl-vs-dml-roles-and-ownership.md`](../06-database-strategies/04-ddl-vs-dml-roles-and-ownership.md)
- [ ] Run schema change as expand/contract, always reversible — see [`01-core-concepts/04-data-integrity-and-migrations.md`](../01-core-concepts/04-data-integrity-and-migrations.md)
- [ ] Choose transaction boundaries and isolation levels on purpose

### Cloud & DevOps

- [ ] Structure repositories and branching for a team, not for yourself — see [`00-project-setup-roadmap/01-repo-and-branching.md`](../00-project-setup-roadmap/01-repo-and-branching.md)
- [ ] Handle configuration and secrets per environment with no secret in the repo — see [`00-project-setup-roadmap/02-environment-and-secrets.md`](../00-project-setup-roadmap/02-environment-and-secrets.md)
- [ ] Build a CI pipeline that fails fast and says why — see [`00-project-setup-roadmap/05-ci-pipeline.md`](../00-project-setup-roadmap/05-ci-pipeline.md)
- [ ] Deploy with a promotion path and a rehearsed rollback — see [`00-project-setup-roadmap/06-cd-and-deployment.md`](../00-project-setup-roadmap/06-cd-and-deployment.md)
- [ ] Instrument logs, metrics, and traces so an incident is diagnosable at 3 a.m. — see [`00-project-setup-roadmap/07-observability.md`](../00-project-setup-roadmap/07-observability.md) and [`01-core-concepts/07-observability-mindset.md`](../01-core-concepts/07-observability-mindset.md)
- [ ] Containerise and size workloads against real resource profiles
- [ ] Express infrastructure as code and review it like application code
- [ ] Understand cost attribution — per tenant, per environment, per service

### Security

- [ ] Make the secure path the default path — see [`01-core-concepts/05-security-by-default.md`](../01-core-concepts/05-security-by-default.md)
- [ ] Explain OAuth 2.x flows precisely and pick the right one — see [`04-security-and-authentication/01-oauth2-fundamentals.md`](../04-security-and-authentication/01-oauth2-fundamentals.md)
- [ ] Know what OAuth 2.1 removed and why (mandatory PKCE, retired implicit and password grants) — see [`04-security-and-authentication/02-oauth21-whats-new-and-why-it-matters.md`](../04-security-and-authentication/02-oauth21-whats-new-and-why-it-matters.md)
- [ ] Distinguish an ID token from an access token and never swap their jobs — see [`04-security-and-authentication/03-openid-connect-vs-oauth2.md`](../04-security-and-authentication/03-openid-connect-vs-oauth2.md)
- [ ] Model realms, clients, roles, and groups in an identity provider — see [`04-security-and-authentication/04-keycloak-realms-clients-and-roles.md`](../04-security-and-authentication/04-keycloak-realms-clients-and-roles.md)
- [ ] Design an SSO topology across multiple resource servers — see [`04-security-and-authentication/05-keycloak-in-a-banking-sso-architecture.md`](../04-security-and-authentication/05-keycloak-in-a-banking-sso-architecture.md)
- [ ] Read PCI DSS, GDPR, and SOC 2 as engineering constraints — see [`04-security-and-authentication/06-compliance-standards-pci-dss-gdpr-soc2.md`](../04-security-and-authentication/06-compliance-standards-pci-dss-gdpr-soc2.md)
- [ ] Enforce authorisation server-side, per tenant, in the query — never in the component
- [ ] Threat-model a feature before building it; validate input and encode output at every boundary

### Quality & Reliability

- [ ] Shape the test suite for 2026 economics — fewer end-to-end tests, more integration and contract tests — see [`08-testing-strategies/01-the-testing-pyramid-revisited.md`](../08-testing-strategies/01-the-testing-pyramid-revisited.md)
- [ ] Draw the unit/integration boundary where it earns confidence — see [`08-testing-strategies/02-unit-and-integration-testing.md`](../08-testing-strategies/02-unit-and-integration-testing.md)
- [ ] Keep end-to-end tests on critical paths only, and keep them non-flaky — see [`08-testing-strategies/03-end-to-end-testing-playwright-and-cypress.md`](../08-testing-strategies/03-end-to-end-testing-playwright-and-cypress.md)
- [ ] Design for failure: timeouts, retries with backoff, circuit breakers, graceful degradation — see [`01-core-concepts/03-failure-modes-and-resilience.md`](../01-core-concepts/03-failure-modes-and-resilience.md)
- [ ] Hold a real Definition of Done and refuse to move the line under pressure — see [`00-project-setup-roadmap/10-definition-of-done.md`](../00-project-setup-roadmap/10-definition-of-done.md)
- [ ] Run code review as a quality and teaching mechanism — see [`00-project-setup-roadmap/09-code-review-process.md`](../00-project-setup-roadmap/09-code-review-process.md)
- [ ] Write documentation that survives the author leaving — see [`00-project-setup-roadmap/08-documentation-baseline.md`](../00-project-setup-roadmap/08-documentation-baseline.md)
- [ ] Run a blameless post-incident review that produces a change, not a document

## How It Works

The six sub-domains are not equally weighted at every career stage, and they are not learned in isolation. Programming and design come first because everything else is expressed through them; security and reliability come last to mature because they are learned largely from things going wrong.

A practical sequencing that holds up in enterprise environments:

1. **Programming & Software Design** to a teach-and-review level. Without this the rest is cargo cult.
2. **Backend & Database** *or* **Frontend**, whichever your product's centre of gravity is — to teach-and-review; the other to implement level.
3. **Quality & Reliability** — this is what turns a fast developer into a trusted one, and it is the sub-domain most visible to a team lead.
4. **Cloud & DevOps** to implement level; enough to own a pipeline and diagnose an incident, not enough to replace a platform team.
5. **Security** continuously and permanently, at recognise level from day one and teach level by the time you are leading — because in banking, insurance, and multi-tenant SaaS this is the sub-domain where a single gap is unrecoverable.

## Practical Example

Return to the SaaS company's three incidents and map each to the checklist, which is what makes the framework worth having.

**Incident 1 — cross-tenant data exposure.** A list view filtered by tenant in the React component; the API returned all rows. Failure surface: *Security* ("enforce authorisation server-side, per tenant, in the query") and *Frontend* ("never treat a UI-level check as an authorisation control"). The remediation was not a training course; it was a review rule — every list endpoint must show its tenant predicate in the SQL, and reviewers reject the diff otherwise. See [`01-core-concepts/05-security-by-default.md`](../01-core-concepts/05-security-by-default.md).

**Incident 2 — silent row loss in the nightly export.** The export bounded its window with local timestamps; on the DST transition an hour was either duplicated or skipped, and no test covered it because the test data was all mid-month. Failure surface: *Quality & Reliability* ("draw the unit/integration boundary where it earns confidence") and *Backend & Database* ("choose transaction boundaries on purpose"). Remediation: store instants in UTC, make the export window explicit and half-open, and add a boundary test at the DST edge.

**Incident 3 — unrollbackable release.** The migration dropped a column in the same release that stopped writing to it, so rolling back the application left it writing to a column that no longer existed. Failure surface: *Backend & Database* ("expand/contract, always reversible"). Remediation: a migration checklist gate in CI — any `DROP` or `NOT NULL` addition requires a linked prior release that stopped depending on it. See [`01-core-concepts/04-data-integrity-and-migrations.md`](../01-core-concepts/04-data-integrity-and-migrations.md).

Three incidents, three named checklist lines, three specific control changes. That is what a checklist organised by failure surface buys you that a technology list does not.

## Enterprise Trade-offs

| Way of building depth | Pros | Cons | When to use it in an enterprise setting |
|---|---|---|---|
| **Breadth-first across all six sub-domains** | Well-rounded; good for small teams where you own the whole stack; fastest route to being useful anywhere | No sub-domain reaches teach-and-review level; you become the second-best person in every conversation | Startups, small platform teams, early career |
| **Deep in one sub-domain, recognise-level elsewhere** | Genuine authority in one area; the classic specialist path | Fragile to reorganisation and stack change; limited influence on decisions outside the area | Core banking engines, cryptography, actuarial calculation, performance-critical platform work |
| **Two deep, four solid (the realistic T)** | Enough authority to lead design review across a product; still credible when the stack shifts | Takes roughly 3–5 years of deliberate effort; requires saying no to work outside the plan | The default target for a technical lead in insurance, banking, or enterprise SaaS |
| **Tool-list breadth (framework collecting)** | Interviews well; feels productive | Fails exactly where this article's incidents failed — the failure surfaces are conceptual, not tool-specific | Avoid; if a new tool is needed, learn it inside a sub-domain you already understand |

---

**Next:** [`04-team-lead-skills.md`](04-team-lead-skills.md) — the first move outward along the horizontal stroke.
