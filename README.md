# Software Engineering Knowledge Base

**A field-tested reference for engineers working in Banking, Insurance, and large Enterprise Solutions — written to stay relevant through 2030.**

## A note from the author

I wrote this because most engineering references fall into one of two failure modes: they're either a tutorial that teaches a toy example and abandons you at the first regulatory constraint, or a reference so dry it never tells you *why* the rule exists. Every article here is grounded in a named scenario — a bank, an insurer, an enterprise vendor — because the judgment calls that actually matter only show up once real money, real audits, and real deadlines are in the room.

Nothing here is theoretical. Every article includes a concrete failure, a concrete fix, and a trade-off table that tells you when to ignore the advice. Read it in order if you're starting a project from zero; jump straight to the folder you need if you're already mid-project. If you only have five minutes, go to the [cheat sheet](12-quick-reference/cheat-sheet-one-pager.md).

— Mengty LIM

## How to use this

- **Starting a new project?** Read `00` through `03` in order before writing code.
- **Mid-project and stuck on one thing?** Jump straight to the relevant domain folder — every article is self-contained.
- **Need working code, not just theory?** `10-example-code/` has complete, runnable Spring Boot, Next.js, and React examples.
- **Five minutes only?** [`12-quick-reference/cheat-sheet-one-pager.md`](12-quick-reference/cheat-sheet-one-pager.md).

---

## Table of Contents

### 00 — Project Setup Roadmap
The baseline every project needs before feature work starts.

1. [Branching Strategy Is a Governance Decision, Not a Developer Preference](00-project-setup-roadmap/01-repo-and-branching.md)
2. [Secrets Management: The Config Mistake That Becomes a Breach Notification](00-project-setup-roadmap/02-environment-and-secrets.md)
3. [Package by Feature: Why Enterprise Codebases Rot from the Folder Tree Up](00-project-setup-roadmap/03-project-structure.md)
4. [Automate the Arguments Away: Linting, Formatting and Hooks at Team Scale](00-project-setup-roadmap/04-linting-formatting-hooks.md)
5. [The Ten-Minute Pipeline: CI That People Actually Wait For](00-project-setup-roadmap/05-ci-pipeline.md)
6. [Deploy Is Not Release: Shipping Safely When Downtime Is a Regulatory Event](00-project-setup-roadmap/06-cd-and-deployment.md)
7. [You Cannot Debug What You Did Not Instrument](00-project-setup-roadmap/07-observability.md)
8. [ADRs as Audit Evidence: Documentation That Survives the People Who Wrote It](00-project-setup-roadmap/08-documentation-baseline.md)
9. [Four Eyes, Ten Minutes: Code Review as a Control, Not a Ritual](00-project-setup-roadmap/09-code-review-process.md)
10. ["Done" Is Not a Feeling: A Definition of Done for Regulated Delivery](00-project-setup-roadmap/10-definition-of-done.md)

### 01 — Core Concepts
The judgment layer underneath every other domain.

1. [SOLID Without the Dogma: What Actually Buys You Maintainability](01-core-concepts/01-solid-and-separation-of-concerns.md)
2. [The Abstraction You Didn't Need: YAGNI, KISS and the Two Kinds of DRY](01-core-concepts/02-yagni-kiss-dry.md)
3. [Everything Remote Fails: Designing for the Call That Doesn't Come Back](01-core-concepts/03-failure-modes-and-resilience.md)
4. [The Database Is the Last Line of Defence](01-core-concepts/04-data-integrity-and-migrations.md)
5. [Deny by Default: Why Authorization Is the Bug That Reaches Production](01-core-concepts/05-security-by-default.md)
6. [A Performance Budget Turns Opinion Into a Failing Build](01-core-concepts/06-performance-budget.md)
7. [Instrument for the Question You'll Be Asked at 3am](01-core-concepts/07-observability-mindset.md)
8. [Technical Debt Is a Number, Not a Feeling](01-core-concepts/08-technical-debt-tracking.md)

### 02 — Design Patterns
Named solutions to named problems — and when not to reach for one.

1. [Constructing the Right Object: Creational Patterns When One Payment Has Five Formats](02-design-patterns/01-creational-patterns.md)
2. [Three Legacy Raters, One Interface: Structural Patterns as Legacy Containment](02-design-patterns/02-structural-patterns.md)
3. [The 4,000-Line If: Behavioral Patterns and the Approval Workflow That Nobody Could Change](02-design-patterns/03-behavioral-patterns.md)
4. [Seventeen Files to Save a Claim: How to Recognise a Pattern Applied for Its Own Sake](02-design-patterns/04-when-not-to-use-a-pattern.md)

### 03 — Architecture
Structural choices, with a decision table instead of a preference.

1. [Layered Architecture: The Default That Quietly Becomes a Pass-Through](03-architecture/01-layered-architecture.md)
2. [Ports and Adapters: How a Bank Replaced Its Core Without Rewriting Its Rules](03-architecture/02-hexagonal-architecture.md)
3. [Clean Architecture: The Dependency Rule, and Where It Stops Paying](03-architecture/03-clean-architecture.md)
4. [Start With a Modular Monolith: Earn Your Microservices](03-architecture/04-microservices-vs-monolith.md)
5. [Events, Exactly Once, and Other Lies: Event-Driven Architecture in Payments](03-architecture/05-event-driven-architecture.md)
6. [Choosing the Right Architecture: A Decision Table, Not a Preference](03-architecture/06-choosing-the-right-architecture.md)

### 04 — Security & Authentication
OAuth, OIDC, Keycloak, and compliance as engineering constraints.

1. [OAuth 2.0 Without the Folklore: The Four Flows That Still Matter](04-security-and-authentication/01-oauth2-fundamentals.md)
2. [OAuth 2.1: The Version That Deletes Things](04-security-and-authentication/02-oauth21-whats-new-and-why-it-matters.md)
3. [ID Token or Access Token: The Distinction That Breaks SaaS Authentication](04-security-and-authentication/03-openid-connect-vs-oauth2.md)
4. [Keycloak by Design: Realms, Clients and the Role Model You Won't Regret](04-security-and-authentication/04-keycloak-realms-clients-and-roles.md)
5. [One Login, Nine Applications: Keycloak as a Bank's Tier-0 Dependency](04-security-and-authentication/05-keycloak-in-a-banking-sso-architecture.md)
6. [PCI-DSS, GDPR and SOC 2 as Backlog Items: What Each One Forces You to Build](04-security-and-authentication/06-compliance-standards-pci-dss-gdpr-soc2.md)
7. [The Network Is Not an Authentication Mechanism](04-security-and-authentication/07-securing-internal-vs-external-apis.md)

### 05 — APIs & Integration
REST, GraphQL, gRPC, and how to actually choose between them.

1. [REST at Enterprise Scale: The Parts That Break When You Can't Force an Upgrade](05-apis-and-integration/01-rest-api-design-principles.md)
2. [GraphQL Without the Regret: Schemas, N+1, and the Queries That Take You Down](05-apis-and-integration/02-graphql-core-concepts-and-tradeoffs.md)
3. [gRPC Inside the Estate: Protobuf Contracts That Survive Ten Years of Changes](05-apis-and-integration/03-grpc-and-protobuf-for-internal-services.md)
4. [Stop Hand-Writing API Clients: Orval, OpenAPI, and a CI Gate That Catches Drift](05-apis-and-integration/04-orval-generating-typed-clients-from-openapi.md)
5. [REST, GraphQL, or gRPC: Stop Choosing One and Start Drawing Boundaries](05-apis-and-integration/05-choosing-rest-vs-graphql-vs-grpc.md)

### 06 — Database Strategies
Indexing, partitioning, ownership, and migrations that survive an audit.

1. [Indexes Are Not Free: What to Add, What to Delete, and How to Prove It](06-database-strategies/01-indexing-strategies-that-actually-work.md)
2. [Partitioning: Making a Billion-Row Table Behave Like a Small One](06-database-strategies/02-partitioning-for-scale.md)
3. [Normalise Until It Hurts, Denormalise Until It Works — But Never in the Same Place](06-database-strategies/03-normalization-vs-denormalization.md)
4. [Nobody Types UPDATE Into Production: Owning DDL and DML Separately](06-database-strategies/04-ddl-vs-dml-roles-and-ownership.md)
5. [YAML or XML: Choosing a Liquibase Dialect You Can Still Audit in 2030](06-database-strategies/05-liquibase-yaml-vs-xml-changelogs.md)
6. [Three Deploys, One Column: Zero-Downtime Migration When an Auditor Is Watching](06-database-strategies/06-schema-migrations-in-a-regulated-environment.md)

### 07 — Frontend Best Practices
React, Next.js, Vue/Nuxt, and the modern data/state/form stack.

1. [Composition Is the Architecture: Structuring React 19 for Systems That Outlive Their Authors](07-frontend-best-practices/01-react-component-architecture.md)
2. [Dynamic Is Not a Failure: Choosing Rendering Strategies in the Next.js App Router](07-frontend-best-practices/02-nextjs-app-router-and-rendering-strategies.md)
3. [Vue's Composition API and Nuxt, Judged Fairly Against Next.js](07-frontend-best-practices/03-vue-and-nuxt-composition-api.md)
4. [Utility-First Without the Soup: Tailwind CSS v4 as an Enforced Design System](07-frontend-best-practices/04-styling-with-tailwind-css.md)
5. [Motion That Says Something: Animation as Information in Data-Dense Enterprise UIs](07-frontend-best-practices/05-motion-for-purposeful-animation.md)
6. [Server State Is Not Your State: TanStack Query v5 as the Cache You Were Going to Write Badly](07-frontend-best-practices/06-data-fetching-and-caching-with-tanstack-query.md)
7. [The URL Is a Feature: Type-Safe Search-Param State with nuqs](07-frontend-best-practices/07-url-state-management-with-nuqs.md)
8. [One Schema, Both Sides: React Hook Form and Zod for Forms That Cannot Be Wrong](07-frontend-best-practices/08-forms-with-react-hook-form-and-zod.md)

### 08 — Testing Strategies
The pyramid re-shaped for 2026, contract testing, and regulated-system evidence.

1. [The Pyramid Was Never About Ratios: Re-Shaping the Test Suite for 2026](08-testing-strategies/01-the-testing-pyramid-revisited.md)
2. [Test Against the Real Database: Where Unit Ends and Integration Begins](08-testing-strategies/02-unit-and-integration-testing.md)
3. [Forty Tests You Trust: Killing Flake in End-to-End Suites](08-testing-strategies/03-end-to-end-testing-playwright-and-cypress.md)
4. [Am I Safe to Deploy? Contract Testing When Two Teams Ship Independently](08-testing-strategies/04-contract-testing-for-apis.md)
5. [Tests as Evidence: Traceability, Synthetic Data, and Proving the Controls Work](08-testing-strategies/05-testing-in-regulated-enterprise-systems.md)

### 09 — UX/UI Guidelines
Engineer-facing rules, not designer-facing suggestions.

1. [Five Shades of Delete: Design Tokens as the Contract You Can Lint](09-ux-ui-guidelines/01-ui-consistency-and-design-tokens.md)
2. [The VPAT Question: Accessibility as a Procurement Gate, Not a Nice-to-Have](09-ux-ui-guidelines/02-accessibility-basics-wcag.md)
3. [A Claims Grid Is Not a Phone Experience: Responsive Design With an Actual Opinion](09-ux-ui-guidelines/03-responsive-and-mobile-first.md)
4. [The Five States: Why One Failing Widget Should Never Blank a Page](09-ux-ui-guidelines/04-loading-error-empty-states.md)
5. [Handoff Is a Contract: What to Demand, What You Owe, and Who Decides](09-ux-ui-guidelines/05-engineer-designer-handoff.md)

### 10 — Example Code
Complete, runnable code — not placeholders.

**Spring Boot**
1. [A Hexagonal Payment Slice You Could Actually Ship](10-example-code/spring-boot/hexagonal-architecture-example.md)
2. [A CRUD Resource That Survives an Insurance Audit](10-example-code/spring-boot/rest-api-crud-example.md)
3. [Locking Down a Banking API with Keycloak and Spring Security 6](10-example-code/spring-boot/keycloak-oauth2-resource-server-example.md)
4. [Three Releases to Rename a Column Without Downtime](10-example-code/spring-boot/liquibase-changelog-example.md)
5. [The Test Pyramid, Built Once and Argued About Never Again](10-example-code/spring-boot/testing-example.md)

**Next.js**
1. [The App Router Layout That Survives Its Third Team](10-example-code/nextjs/app-router-project-structure.md)
2. [Route Handlers or Server Actions? Build Both, Then Choose Correctly](10-example-code/nextjs/api-route-example.md)
3. [Stop Hand-Writing the Client: Orval, Zod, MSW and a CI Job That Breaks on Contract Drift](10-example-code/nextjs/orval-generated-client-example.md)
4. [One Schema, Four Steps, Zero Trust: A KYC Onboarding Form That Validates the Same Way Twice](10-example-code/nextjs/form-with-react-hook-form-zod-example.md)

**React**
1. [One Table, Twenty-Three Props: Building a Claims Grid That Survives Its Third Stakeholder](10-example-code/react/component-structure-example.md)
2. [The Dashboard That Stopped Lying: TanStack Query v5 With a Key Factory, Optimistic Rollback, and No Blanket Invalidation](10-example-code/react/tanstack-query-example.md)
3. [The URL Is the View: Filterable Tables with nuqs](10-example-code/react/nuqs-url-state-example.md)

### 11 — Stakeholder Communication
Talking to PMs, POs, and Operations without losing the room.

1. [Talking to Product Managers: Estimates That Survive Contact With a Roadmap](11-stakeholder-communication/01-talking-to-pm.md)
2. [Talking to Product Owners: Acceptance Criteria Are a Contract, Not Paperwork](11-stakeholder-communication/02-talking-to-po.md)
3. [Talking to Operations: The People Who Get Paged for Your Code](11-stakeholder-communication/03-talking-to-operations.md)
4. [Say It in Their Currency: A Translation Table for Engineering Language](11-stakeholder-communication/04-technical-to-business-translation-table.md)
5. [Eight Scripts for the Conversations You'd Rather Not Have](11-stakeholder-communication/05-meeting-scripts.md)
6. [Say the Conclusion First: The Rules That Make You Sound Like You Know](11-stakeholder-communication/06-golden-rules-confident-communication.md)

### 12 — Quick Reference
If you only have five minutes.

1. [The One-Pager: Every Domain in a Line](12-quick-reference/cheat-sheet-one-pager.md)

### 13 — Career & Technical Leadership
*(Bonus domain, beyond the original brief — kept because it's a natural extension of everything above: how the judgment in this knowledge base compounds into a career.)*

1. [The T-Shaped Technical Leader: One Career, Not Four](13-career-and-technical-leadership/01-the-t-shaped-technical-leader.md)
2. [Five Skill Clusters: The Map Before the Climb](13-career-and-technical-leadership/02-skill-framework-overview.md)
3. [The Vertical Stroke: What Expert Actually Means in 2026](13-career-and-technical-leadership/03-expert-developer-skills.md)
4. [From Solver to Multiplier: The Team Lead Shift](13-career-and-technical-leadership/04-team-lead-skills.md)
5. [Delivery You Can Defend: Project Management for Engineers](13-career-and-technical-leadership/05-project-management-skills.md)
6. [Deciding What to Build: Product Ownership for Technical Leaders](13-career-and-technical-leadership/06-product-owner-skills.md)
7. [The Connective Tissue: Skills That Make Technical Depth Usable](13-career-and-technical-leadership/07-shared-leadership-skills.md)
8. [The Eighteen-Month Roadmap to a T-Shaped Technical Leader](13-career-and-technical-leadership/08-the-eighteen-month-roadmap.md)
9. [The Weekly Practice System: Making the Roadmap Actually Happen](13-career-and-technical-leadership/09-weekly-practice-system-and-metrics.md)
10. [The First Thirty Days: Starting the T-Shaped Path for Real](13-career-and-technical-leadership/10-the-first-thirty-days.md)

---

**93 articles, 13 domains.** Every article is self-contained — read the ones you need, in whatever order your project demands.
