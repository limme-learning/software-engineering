---
title: "The One-Pager: Every Domain in a Line"
author: Mengty LIM
category: 12-quick-reference
last_updated: 2026
---

# The One-Pager: Every Domain in a Line

No new content here — one line per domain, pointing back to the articles that carry the actual argument. Use this to find the right folder fast; use the linked article for the reasoning, the diagram, and the trade-offs.

| Domain | One line | Start here |
|---|---|---|
| `00` Project Setup & Roadmap | Repo, secrets, structure, hooks, CI/CD, observability, docs, review, and Definition of Done — the baseline before any feature work starts | [`01-repo-and-branching.md`](../00-project-setup-roadmap/01-repo-and-branching.md) |
| `01` Core Concepts | SOLID, YAGNI/KISS/DRY, resilience, data integrity, security by default, performance budgets, observability mindset, technical debt — the judgment layer under everything else | [`01-solid-and-separation-of-concerns.md`](../01-core-concepts/01-solid-and-separation-of-concerns.md) |
| `02` Design Patterns | Creational, structural, and behavioral patterns — and the discipline to know when *not* to reach for one | [`04-when-not-to-use-a-pattern.md`](../02-design-patterns/04-when-not-to-use-a-pattern.md) |
| `03` Architecture | Layered, hexagonal, clean, microservices-vs-monolith, event-driven — and a decision table instead of a preference | [`06-choosing-the-right-architecture.md`](../03-architecture/06-choosing-the-right-architecture.md) |
| `04` Security & Authentication | OAuth 2.0/2.1, OIDC, Keycloak realms/clients/roles, banking SSO, PCI-DSS/GDPR/SOC 2, internal vs. external API security | [`01-oauth2-fundamentals.md`](../04-security-and-authentication/01-oauth2-fundamentals.md) |
| `05` APIs & Integration | REST, GraphQL, gRPC, typed clients from OpenAPI via Orval, and how to actually choose between the three | [`05-choosing-rest-vs-graphql-vs-grpc.md`](../05-apis-and-integration/05-choosing-rest-vs-graphql-vs-grpc.md) |
| `06` Database Strategies | Indexing, partitioning, normalization trade-offs, DDL/DML ownership, Liquibase YAML vs. XML, zero-downtime migrations | [`06-schema-migrations-in-a-regulated-environment.md`](../06-database-strategies/06-schema-migrations-in-a-regulated-environment.md) |
| `07` Frontend Best Practices | React composition, Next.js rendering strategies, Vue/Nuxt, Tailwind, motion, TanStack Query, nuqs, React Hook Form + Zod | [`01-react-component-architecture.md`](../07-frontend-best-practices/01-react-component-architecture.md) |
| `08` Testing Strategies | The pyramid re-shaped for 2026, unit vs. integration, Playwright/Cypress without flake, contract testing, regulated-system evidence | [`01-the-testing-pyramid-revisited.md`](../08-testing-strategies/01-the-testing-pyramid-revisited.md) |
| `09` UX/UI Guidelines | Design tokens, WCAG as a procurement gate, responsive-with-an-opinion, loading/error/empty states, engineer-designer handoff | [`01-ui-consistency-and-design-tokens.md`](../09-ux-ui-guidelines/01-ui-consistency-and-design-tokens.md) |
| `10` Example Code | Working, non-placeholder code: Spring Boot hexagonal/CRUD/Keycloak/Liquibase/testing; Next.js App Router/Orval/forms; React TanStack Query/nuqs | [`10-example-code/`](../10-example-code/) |
| `11` Stakeholder Communication | Talking to PMs, POs, and Operations; the technical-to-business translation table; meeting scripts; golden rules | [`06-golden-rules-confident-communication.md`](../11-stakeholder-communication/06-golden-rules-confident-communication.md) |
| `13` Career & Technical Leadership | The T-shaped path from Expert Developer to Team Lead, Project Management, and Product Owner — an 18-month roadmap with a weekly cadence | [`01-the-t-shaped-technical-leader.md`](../13-career-and-technical-leadership/01-the-t-shaped-technical-leader.md) |

## The one thing to remember per domain, if you remember nothing else

- **Setup:** automate the argument away — linting, CI, and DoD exist so humans stop re-litigating the same decision.
- **Core Concepts:** every principle here answers one question — how much has to change when the requirement changes?
- **Design Patterns:** a pattern is a solution to a named problem; applying one without the problem is the anti-pattern.
- **Architecture:** start with the modular monolith; earn the next layer of complexity with evidence, not fashion.
- **Security:** deny by default, enforce authorization server-side, and never in the UI layer.
- **APIs:** choose the protocol per boundary, not once for the whole estate.
- **Database:** the schema is the last line of defence — migrate expand/contract, always reversible.
- **Frontend:** server state is not component state; stop reinventing the cache.
- **Testing:** fewer end-to-end tests, more integration and contract tests, all of them fast enough to trust.
- **UX/UI:** design tokens are a contract you can lint, not a suggestion.
- **Example Code:** copy the shape, not the literal values — every example assumes real secrets and real config live elsewhere.
- **Stakeholder Communication:** lead with the decision, not the derivation.
- **Career & Leadership:** depth earns you the room; breadth is what makes the room listen.
