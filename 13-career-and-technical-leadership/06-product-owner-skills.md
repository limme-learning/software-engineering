---
title: "Deciding What to Build: Product Ownership for Technical Leaders"
author: Mengty LIM
category: 13-career-and-technical-leadership
last_updated: 2026
---

# Deciding What to Build: Product Ownership for Technical Leaders

Engineering discipline stops you building the thing badly. Product discipline stops you building the wrong thing beautifully — which is the more expensive mistake, and the one no amount of test coverage catches.

## The Real-World Problem

A retail SaaS vendor sells point-of-sale software to convenience-store chains. Its largest customer, a chain of 340 stores across a region with patchy rural connectivity, escalates the same complaint every quarter: when the network drops, the tills stop. Staff fall back to paper, and reconciling paper at the end of the day takes a manager forty minutes per store.

The request arrives at the engineering team as a one-line ticket: *"Make the POS work offline."* A capable team can start on that immediately, and that is exactly the danger. "Works offline" spans everything from caching the product catalogue for read-only lookups to full bidirectional sync of transactions, inventory decrements, loyalty accruals, and promotional pricing — with conflict resolution for a till that has been offline for six hours while head office repriced half the catalogue.

The narrow version is roughly six weeks. The full version is well over a year and introduces distributed-consistency problems into a system that currently has none. Both are legitimate readings of the same one-line ticket. Choosing between them is not an engineering decision, and the team that treats it as one will pick the interesting version rather than the valuable one.

## The Concept

Product ownership decomposes into three groups of skills. Note the boundary from the previous article: the Product Owner decides *what and why*, the Project Manager delivers *when and how much*. See [`02-skill-framework-overview.md`](02-skill-framework-overview.md) for that distinction in full.

### Discovery — *what is actually true about the customer?*

- [ ] Run customer interviews that ask what people did, not what they would like — past behaviour is evidence, stated preference is not
- [ ] Build personas from real segments with real differences in behaviour, not demographic decoration
- [ ] Map the customer journey end to end, including the parts that happen outside your software
- [ ] Identify the job the customer is hiring the product to do, which is frequently not the feature they asked for
- [ ] Instrument the product so you can see what is used, and check your beliefs against it — see [`00-project-setup-roadmap/07-observability.md`](../00-project-setup-roadmap/07-observability.md)
- [ ] Distinguish the loudest customer from the most representative one
- [ ] Watch someone use the product without helping them

### Strategy — *where is this going, and what will we refuse?*

- [ ] Write a product vision short enough that the team can repeat it without reading it
- [ ] Maintain a roadmap of outcomes and themes, not a dated feature list you will be held to and forced to defend
- [ ] Define an MVP by the riskiest assumption it tests, not by the smallest thing that compiles
- [ ] Say no in a way that keeps the relationship — with the reasoning and the alternative attached
- [ ] Understand the commercial frame: pricing, contract commitments, renewal risk, cost to serve
- [ ] Know which competitors matter and on which specific dimension
- [ ] Distinguish a feature that wins one deal from a feature that changes the product

### Execution — *what does the team build next, and how do we know it is done?*

- [ ] Keep the backlog small enough to be genuinely prioritised; an item nobody will pull in six months is noise
- [ ] Write user stories with a real user, a real outcome, and no implementation instruction
- [ ] Write acceptance criteria that are testable and complete enough to be argued with before coding, not after
- [ ] Prioritise with an explicit method — cost of delay, WSJF, RICE, opportunity cost — and state which one you used
- [ ] Slice vertically: every increment delivers a usable outcome, never just a layer
- [ ] Groom with the team, so that engineering reality reshapes the request before it is committed
- [ ] Accept or reject work against the criteria, and hold that line
- [ ] Measure adoption after release and be willing to remove what nobody uses

### Bad user story vs. good user story

This is the highest-leverage single habit in the cluster, so it is worth being concrete. Take the escalation above.

**Bad — a technical instruction wearing a story's clothes:**

> As a developer, I want to implement an IndexedDB write queue with a background sync worker so that transactions are stored locally when the network is unavailable.

Everything wrong with this in one place. The user is a developer, so it encodes no customer value. The solution is pre-decided, so the team cannot propose a cheaper one. There is no way to tell whether it succeeded — a write queue can be flawlessly implemented while the store manager's forty minutes of reconciliation remain untouched. And it is unsliceable: it is either done or not, with nothing usable in between.

**Good — outcome-oriented and customer-framed:**

> **As a** till operator in a store with unreliable connectivity,
> **I want to** keep completing sales while the connection is down,
> **so that** customers are served without falling back to paper and the store manager has nothing to re-key at end of day.
>
> **Acceptance criteria**
> - Given the network is unavailable, when the operator scans items and takes a cash payment, then the sale completes and prints a receipt with a normal transaction reference
> - Given the network is unavailable, when the operator attempts a card payment, then the till states clearly that card is unavailable offline and cash is offered — the operator is never left guessing
> - Given the till has completed sales offline, when connectivity returns, then those sales appear in head-office reporting within 5 minutes with no manual action
> - Given a product's price changed centrally while the till was offline, when the sale is synced, then the sale retains the price charged at the till and the discrepancy is listed on the daily exception report
> - Given the till has been offline, when the operator opens the shift summary, then offline sales are included in the totals
>
> **Out of scope for this story:** offline inventory decrementing, offline loyalty point accrual, offline card payments.
>
> **Riskiest assumption being tested:** that cash-only offline selling removes the majority of the manager's reconciliation work. Measured by: reconciliation time per store, before and after.

The second version is longer, and that length is the work. The team can now propose a cheaper implementation than IndexedDB-plus-sync-worker if one exists. The fourth criterion — price changed while offline — is a distributed-consistency decision surfaced *before* coding rather than discovered in production. The out-of-scope list is what turns a year of work into six weeks. And the riskiest-assumption line means that if reconciliation time does not drop, everyone finds out in one release rather than after the full sync engine is built.

## How It Works

The sequence from escalation to committed increment:

1. **Restate the request as a problem.** "Make the POS work offline" becomes "store managers spend forty minutes per day re-keying paper transactions caused by connectivity loss." A problem statement can be prioritised against other problems; a feature request can only be scheduled.
2. **Quantify it.** 340 stores, roughly 12 affected per day, 40 minutes each — around eight hours of labour daily, plus the error rate on manual re-keying, plus a renewal risk on the largest contract.
3. **Name the riskiest assumption.** Here: that cash-only offline selling captures most of the value. If false, the cheap version is worthless and the expensive one is required — so this is what the first increment must test.
4. **Cut the thinnest vertical slice that tests it.** Offline cash sales with deferred sync, everything else explicitly out.
5. **Write the story and criteria with the team.** Engineering reshapes it here; the price-conflict criterion came from a developer who knew the pricing service pushes changes asynchronously.
6. **Ship, measure, decide.** Reconciliation time per store is the number. It decides whether increment two is inventory sync or something else entirely.

## Practical Example

The vendor runs the sequence and the first increment ships in seven weeks. Reconciliation time per store drops from 40 minutes to 6 — the residual being card transactions, still unavailable offline.

That result reshapes the roadmap in a way no amount of upfront design would have. The team had assumed inventory sync was the natural second increment; the measurement says the remaining pain is card payments, which is not an engineering problem at all — it is a question for the payment processor and the acquirer about offline authorisation limits and who carries the chargeback risk. The technical lead's next move is a commercial conversation, not a sprint.

Meanwhile the exception report from criterion four produces an unplanned finding: 3% of offline sales hit a price discrepancy, concentrated in two stores that lose connectivity for hours rather than minutes. That is a targeted network problem affecting six stores, solvable with a connectivity change costing a fraction of the sync engine.

Compare with the counterfactual. The team implements the IndexedDB write queue as specified in the bad story. It works. Fourteen months later there is a full bidirectional sync engine with conflict resolution, the largest customer is broadly satisfied, nobody ever measured reconciliation time, and the two rural stores with genuinely bad connectivity still have problems — because their problem was the network, and the software absorbed the cost of never asking.

## Enterprise Trade-offs

| Approach to deciding what to build | Pros | Cons | When to use it in Retail / Insurance / Enterprise SaaS |
|---|---|---|---|
| **Customer-request-driven (build what is escalated)** | Responsive; visibly protects large accounts; easy to justify internally | Produces a product shaped by whoever escalates loudest; features nobody else adopts; unbounded scope | Genuine contractual commitments and regulatory obligations — where the requirement is not actually optional |
| **Assumption-driven increments (as above)** | Cheapest route to knowing whether the value is real; reshapes the roadmap with evidence | Requires instrumentation and the discipline to actually measure; slower to a complete feature | Default for product work where the value is uncertain — most net-new capability |
| **Vision-driven (build the coherent product)** | Produces a product with a spine rather than a feature pile; strong differentiation | Can drift far from what customers will pay for; expensive when the vision is wrong | Platform and category-defining bets, with a leadership team that will fund them for years |
| **Engineering-driven (build what is technically interesting)** | High morale; occasionally produces genuine platform leverage | Value is accidental; hardest to defend when budgets tighten | Time-boxed enablement and platform work with an explicit efficiency case, never as the main roadmap |

---

**Next:** [`07-shared-leadership-skills.md`](07-shared-leadership-skills.md) — making any of this land with people who do not write code.
