# Team Allocation — Hospital Bill Check (Phase 1)

This document says who builds and who approves each Bolt. A **Bolt** is one build pass over a small piece of the work that ends in something that runs and is merged. The Bolts themselves are listed in `bolt-plan.md`.

## Sources

- Team practices: `inception/practices-discovery/team-practices.md` (one builder plus the founder; the workflow's approval step and the automatic checks act as the review before merge).
- Answers in `delivery-planning-questions.md` in this folder, including the confirmed team plan.
- Units and stories: `inception/units-generation/unit-of-work.md` and `unit-of-work-story-map.md`.
- Team Formation did not run. The team is one builder plus the founder, so the plan is confirmed rather than asked (project rule).

## The team

| Who | Role in Construction |
|---|---|
| Builder | Builds every Bolt, working with the AI coding assistant as the developer, and answers its design questions |
| AI coding assistant (developer) | Writes the code and tests for each unit, under the builder's direction and the workflow's approval steps |
| Founder | Decides scope and priority; approves Bolt 1 (the walking skeleton) before Bolt 2 starts; approves every production release; owns the hosting, data-residency and counsel decisions |

There is no second human reviewer. The review before every merge is the workflow's approval step plus the automatic checks, including the security checks (`team-practices.md`).

A **mob** usually means a small group building together at one keyboard. Here the "mob" is the builder and the AI assistant.

## Bolt allocation

| Bolts | Milestone | Units | Built by | Approved by |
|---|---|---|---|---|
| 1a, 1b, 1c | M1 Walking skeleton | U1 contracts, stand-ins | Builder with the AI assistant | **Founder (gate before Bolt 2)** |
| 2 to 10 | M2 Bill reading and checks | U4 bill-analysis | Builder with the AI assistant | Builder at each workflow approval step |
| 11 to 14 | M3 Rules and first accuracy numbers | U9 rules-access, U12 accuracy-runner | Builder with the AI assistant; accuracy runs are started by the builder or the founder on the offline machine | Builder; founder reviews the first accuracy figures (Bolt 14) |
| 15 to 23 | M4 Findings, report and feedback | U6 findings-report, U7 feedback-insights | Builder with the AI assistant | Builder; founder reviews report wording (Bolts 18 to 21) |
| 24 to 26 | M5 Labelled Test Set | U10 test-set-quality | Builder with the AI assistant; labelling is done by both the builder and the founder | Builder |
| 27 to 39 | M6 Family journey | U3, U5, U2, U8 | Builder with the AI assistant | Builder; founder walks the full journey at Bolt 39 |
| 40 to 42 | M7 Internal tools and launch readiness | U11 internal-ui, all | Builder with the AI assistant | **Founder (production release)** |

## Construction staffing

- **One session, one unit at a time.** Every unit is built here, unit by unit, with approvals as the work goes. There is no team ownership of units, because there is one builder.
- **Gating after Bolt 1.** Whether the remaining Bolts run on their own or are gated one by one is decided after Bolt 1 is approved (`team.md`).

## Assumptions & Open Questions

- Labelling real test-set bills needs both the builder and the founder (FR16.5). Their time for labelling is the main human cost outside the builder's own work.
- If a second builder joins, units could move to team ownership. That would need a new plan, because team ownership changes how approvals work.
