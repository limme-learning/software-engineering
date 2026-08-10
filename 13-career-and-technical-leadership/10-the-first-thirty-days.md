---
title: "The First Thirty Days: Starting the T-Shaped Path for Real"
author: Mengty LIM
category: 13-career-and-technical-leadership
last_updated: 2026
---

# The First Thirty Days: Starting the T-Shaped Path for Real

A roadmap and a practice system are only real once someone starts them on a specific Monday. This closing article is that Monday — a four-week plan to move from reading this series to living the first phase of it, plus the positioning statement that the entire series has been building toward.

## The Real-World Problem

An engineer at a national insurer finishes reading a series like this one, agrees with all of it, and opens their calendar to start. The calendar has three release commitments this sprint, a backlog grooming session, two production support rotations, and no visible gap for "become a T-shaped leader." The plan dies exactly where the previous article warned it would — not from disagreement, but from never being converted into a first concrete action with a date attached.

The fix is not more conviction. It is a plan small enough to survive week one: four weeks, one theme each, each with actions specific enough to be verifiably done or not done by Friday.

## The Concept

### Week 1 — Identity and charter

The point of week one is to stop treating this as a private ambition and make it a stated, two-way agreement — the single biggest predictor of whether the plan survives month two.

- [ ] Write your own one-page charter, using the format from [`05-project-management-skills.md`](05-project-management-skills.md): the outcome you're aiming at over 18 months, how you'll know you're on track, and what you are explicitly not taking on yet
- [ ] Have the conversation with your manager — not "I'd like more leadership opportunities" but the charter itself, with a specific ask: which piece of real, funded work becomes the vehicle
- [ ] Identify the vehicle — the actual workstream, team, or initiative the roadmap will be practiced on; it must be real work with real stakes, not a manufactured exercise
- [ ] Block the weekly protected time from [`09-weekly-practice-system-and-metrics.md`](09-weekly-practice-system-and-metrics.md) on your calendar before the week ends, not "soon"

### Week 2 — Architecture and roadmap

Week two grounds the plan in the vertical stroke and the shape of the 18 months, so breadth is built on a foundation that is already visibly solid.

- [ ] Complete one Phase 1 deliverable from [`08-the-eighteen-month-roadmap.md`](08-the-eighteen-month-roadmap.md) against your chosen vehicle — most often a design document or ADR that other people will actually use
- [ ] Map your vehicle against the six-phase roadmap explicitly: which phase are you starting in, and what does Phase 2 look like for this specific piece of work
- [ ] Identify one thing you currently do that you will start delegating in week three, and identify who to
- [ ] Read the [`03-expert-developer-skills.md`](03-expert-developer-skills.md) checklist against yourself honestly and mark your two strongest sub-domains and your weakest — the weakest is not this month's problem, but it is worth knowing

### Week 3 — Backlog and status reporting

Week three is where the plan starts producing artefacts a stakeholder outside engineering will actually read.

- [ ] Write or rewrite one user story for your vehicle using the format from [`06-product-owner-skills.md`](06-product-owner-skills.md) — outcome-oriented, with acceptance criteria, with an explicit out-of-scope list
- [ ] Send your first written status update in the standard format from [`05-project-management-skills.md`](05-project-management-skills.md), even if nobody asked for one yet — this is the artefact that starts building the "trusted without a follow-up meeting" signal from [`07-shared-leadership-skills.md`](07-shared-leadership-skills.md)
- [ ] Make the delegation from week two real — hand the identified task over, with the outcome specified and the how left to them
- [ ] Log one real risk on your vehicle, with an owner and a mitigation, even if the risk log is otherwise informal

### Week 4 — Design review, retrospective, and delegation review

Week four closes the loop and sets up the cadence that carries the plan past month one, which is the point where most such plans quietly die.

- [ ] Run or actively participate in a design review using the review-as-teaching posture from [`04-team-lead-skills.md`](04-team-lead-skills.md) — leave a comment that explains the principle, not just the fix
- [ ] Hold a personal retrospective against the charter from week one: what got done, what slipped, and one specific change for month two
- [ ] Check the week-three delegation: did it land, did you take it back under pressure, what would make the next one land better
- [ ] Schedule the first monthly checkpoint from [`09-weekly-practice-system-and-metrics.md`](09-weekly-practice-system-and-metrics.md) with your manager, using the specific readiness signal for your current roadmap phase, not a general check-in

## How It Works

The four weeks are not independent — each produces the raw material the next one needs, which is why skipping ahead (writing a status report in week one before the vehicle is chosen, say) tends to produce a hollow version of the exercise.

1. **Week 1's charter and vehicle** give weeks 2–4 something real to practice on instead of a hypothetical.
2. **Week 2's design artefact** is what earns the credibility that makes week 3's delegation land — people accept being delegated to by someone whose technical judgment they have just seen demonstrated in writing.
3. **Week 3's status report and risk log** are what week 4's design review and retrospective are measured against — did the risk you logged in week 3 show up, did the status hold.
4. **Week 4's retrospective** feeds directly into the charter, closing the loop back to week 1 and setting the actual content of month two rather than a generic "keep going."

## Practical Example

A senior engineer at a national insurer, running the four weeks against the claims-intake vehicle used throughout this series.

**Week 1.** Charter: "Over 18 months, become the person who can own a delivery commitment on the intake platform end to end — technically, on schedule, and defensibly to the business — without needing a separate PM and PO in the room." Manager conversation lands on the intake-automation workstream as the vehicle, since it is real, funded, and currently has no assigned lead. Thursday-afternoon block goes on the calendar.

**Week 2.** Writes the ADR for the document-classifier redesign that had previously existed only as tribal knowledge. Maps the workstream to the roadmap: currently mid-Phase 1, moving into Phase 2 as soon as the classifier work is handed off. Identifies the classifier implementation itself as the week-three delegation target, to a mid-level engineer who has been quietly duplicating effort — the same specific handoff described in [`04-team-lead-skills.md`](04-team-lead-skills.md).

**Week 3.** Rewrites the vague "improve document intake" backlog item into a proper outcome-oriented story with acceptance criteria and a named riskiest assumption. Sends the first fortnightly status, format borrowed directly from [`05-project-management-skills.md`](05-project-management-skills.md), to a steering group that had previously only heard about the workstream verbally, in passing, in a different meeting. Hands the classifier work over for real, with the domain rules document from [`04-team-lead-skills.md`](04-team-lead-skills.md) as the primary reference instead of herself.

**Week 4.** Reviews the classifier design with the engineer, leaving comments about *why* a confidence threshold matters rather than only what number to use. Retrospective against the charter: the technical artefact and the status report both landed; the delegation is real but she caught herself re-explaining a decision instead of letting him make the next one — the specific adjustment for month two. Books the first monthly checkpoint with her manager against the Phase 1→2 readiness signal: has a pull request in the intake area shipped without her touching the code.

Thirty days in, nothing about her job title has changed. What has changed is that a piece of real work now has a charter, a design record, a properly formed backlog item, a status report a steering group reads, and one delegated task in flight — five artefacts that did not exist a month earlier, each one a direct instance of a checklist item from earlier in this series.

## Enterprise Trade-offs

| Approach to starting | Pros | Cons | When to use it |
|---|---|---|---|
| **Four-week plan against real, funded work (as above)** | Every artefact is real and reviewable; builds the manager relationship needed to sustain 18 months | Requires a vehicle to already exist or be negotiated in week one | The default — always prefer real work over a manufactured exercise |
| **Start immediately, no charter conversation** | Fastest possible start; no dependency on a manager's calendar | No shared agreement to point back to when priorities collide with roadmap time in month two | Only as a stopgap while the charter conversation is being scheduled, never a substitute for it |
| **Wait for a formal opportunity (promotion, reorg, new project)** | Avoids asking for time against unproven intent | Opportunities arrive on someone else's timeline, and the roadmap in [`08`](08-the-eighteen-month-roadmap.md) needs 18 months to run — waiting a year to start costs a year | Reasonable only if genuinely no real vehicle exists yet; even then, week 1's charter can still be written and shopped for a vehicle |
| **Full formal onboarding programme (org-run)** | Structured, resourced, consistent across cohorts | Rare in practice; most organisations have no such programme, which is why this plan is self-directed by design | Use if your organisation actually offers one — otherwise this thirty-day plan is the substitute |

---

**Series index:** [`01`](01-the-t-shaped-technical-leader.md) · [`02`](02-skill-framework-overview.md) · [`03`](03-expert-developer-skills.md) · [`04`](04-team-lead-skills.md) · [`05`](05-project-management-skills.md) · [`06`](06-product-owner-skills.md) · [`07`](07-shared-leadership-skills.md) · [`08`](08-the-eighteen-month-roadmap.md) · [`09`](09-weekly-practice-system-and-metrics.md) · `10` (this article)
