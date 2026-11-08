# RAID Log — Hospital Bill Check (Project RS, Phase 1)

This log lists the Risks, Assumptions, Issues and Dependencies found in this feasibility stage. It builds on:

- the approved intent statement (`intent-statement.md`);
- the market research (`competitive-analysis.md`, `market-trends.md`, `build-vs-buy.md`);
- `feasibility-assessment.md` and `constraint-register.md` in this folder.

## Sources

- [Q1]–[Q12]: answers in `feasibility-questions.md` in this folder.
- PRD RK-n / A-n / D-n: `PRDs/app-baseline.md`, v0.14 (PRD-v2). Where an entry restates a PRD risk, its PRD number is given.
- **Scoring:** Likelihood (L) and Impact (I) on a 1–5 scale; Score = L × I, as in PRD §18. Scores here are conservative estimates by this stage.

## Risks

New risks found in this stage come first. The PRD risks that bear most directly on feasibility follow, with their PRD scores.

| ID | Risk | L | I | Score | Response | Owner | Source |
|---|---|---|---|---|---|---|---|
| R1 | **No bill-reading option meets the accuracy targets on real Indian bills.** Without that, the depth and objective-findings differentiators fail | 3 | 5 | 15 | Mitigate. Run the trial on synthetic and then real bills before any decision (F1); keep a second option usable; use the PRD's safety net (user confirmation of low-confidence fields, shadow mode) | You | [Q5] [Q12]; `build-vs-buy.md` |
| R2 | **The synthetic-bill trial overstates real-world accuracy**, leading to a decision that fails on real photos | 4 | 4 | 16 | Mitigate. Treat the synthetic trial as a screen only; the decision is final only after the real-bill repeat [Q12] | You | [Q12] |
| R3 | **One builder carries the whole Phase 1 boundary.** Slow progress, single point of failure, and nobody else reviews the work | 4 | 4 | 16 | Mitigate. Use the workflow's built-in reviews and test gates; arrange outside security testing (F5); keep the weekly founder update | You / founder | [Q1] |
| R4 | **Labelling throughput.** Two people (you and the founder) cannot label 300–500 real bills fast enough, so checks stay hidden longer | 3 | 4 | 12 | Mitigate. Lean on synthetic bills with planted answers first (PRD D28, D29); plan labelling time with the founder; track coverage (PRD LT-12) | You / founder | [Q6] [Q11] |
| R5 | **Labels are not independent of the AI** because the builder also labels | 2 | 4 | 8 | Mitigate. Always label before seeing the AI's output; the founder's labels act as the independent check (PRD D29) | You / founder | [Q1] [Q11] |
| R6 | **The hosting choice conflicts with an India-only residency decision.** The leaning option may not keep stored data and logs only in India | 3 | 4 | 12 | Avoid. Get the residency decision before the hosting decision (D1, D2); confirm with the provider **[VALIDATE]** | Founder / you | [Q3]; `feasibility-assessment.md` §4 |
| R7 | **Running cost grows with no budget or revenue**, especially at 10 times pilot volume | 2 | 3 | 6 | Mitigate. Record cost per audit in the trial; set a cost alarm once hosting is chosen | You / founder | [Q7] [Q8]; PRD D25 |
| R8 | **Incident response and the 6-hour CERT-In reporting rest on one person** | 3 | 4 | 12 | Mitigate. Write an incident plan that one person can run; name the founder as backup contact | You / founder | [Q1]; PRD NFR-11 |
| R9 | **NPPA reference data goes stale.** The knee-implant cap is due for decision in November 2026 | 3 | 3 | 9 | Mitigate. Version and date reference data from launch; run the monthly check (PRD §13.10) | You | `market-trends.md` §2.2 |
| PRD RK7 | AI hallucination: invented line items, wrong amounts, wrong references | 3 | 5 | 15 | As PRD: evidence-linked findings only; arithmetic re-verification; references from the curated library | You | PRD §18 |
| PRD RK2 | False positives damage user credibility | 4 | 4 | 16 | As PRD: checks go live in waves only after passing the Labelled Test Set; shadow mode; neutral language | You | PRD §18 |
| PRD RK27 | Domain-knowledge gap: rules and labels built without billing expertise | 4 | 4 | 16 | As PRD: launch with the most objective checks only; record each rule's public source | You / founder | PRD §18 |
| PRD RK28 | Launching without legal or tax review | 3 | 5 | 15 | Accepted by the founder (PRD D27); conservative wording | Founder | PRD §18 |
| PRD RK5 | DPDP non-compliance: third-party data, minors, deceased patients, consent withdrawal | 3 | 5 | 15 | As PRD: consent flows; de-identification; 3-month retention | You / founder | PRD §18 |
| PRD RK6 | Breach of health data | 2 | 5 | 10 | As PRD: security controls; data minimisation; incident playbook; outside security testing (F5) | You | PRD §18 |
| PRD RK30 | Skewed test set: network bills over-represent a few cities and large hospitals | 3 | 4 | 12 | As PRD: synthetic bills for missing formats; beta testers across States | You / founder | PRD §18 |

## Assumptions

| ID | Assumption | How it will be checked | Status | Source |
|---|---|---|---|---|
| A1 | The speed targets (minutes, not seconds) are reachable for a single bill **[hypothesis]** | Measure in the bill-reading trial | To validate | `feasibility-assessment.md` §2.3 |
| A2 | Running cost is not what decides feasibility at under 1,000 audits a month **[hypothesis]** | Cost per audit recorded in the trial | To validate | [Q7] [Q8] |
| A3 | The existing web app starter is a viable base for the product | Later design stages | Likely | [Q2] |
| A4 | Users can obtain itemised bills when guided | As PRD A4 / H1: at least 60% of cases | To validate | PRD §16 |
| A5 | Accurate rules and a reliable Labelled Test Set can be built from public sources without domain experts | As PRD H2 / A18: precision targets met | To validate | PRD §16 |
| A6 | Consented real bills can be gathered from personal networks | Count of consented bills gathered | To validate | [Q6]; PRD D28 |

## Issues

| ID | Issue | Impact | Action | Owner | Source |
|---|---|---|---|---|---|
| I1 | There are no real bills for the Labelled Test Set yet | No accuracy figure on real bills is possible yet | Start with synthetic bills; begin gathering consented bills from personal networks (F4) | You / founder | [Q6] |
| I2 | It is not known whether a cloud account exists for the product | Hosting cannot be set up yet | Check, and set one up once hosting is decided (D2) | You | [Q4] |
| I3 | Plausibility checks that need "typical quantity" data have no Phase 1 source | Those checks cannot go live in Phase 1 as specified | Carry PRD G-03 into Scope Definition: name a public source, derive ranges from the test set, or move them to Phase 1.5 | Founder | PRD G-03 |

## Dependencies

| ID | Dependency | Blocks | Owner | Source |
|---|---|---|---|---|
| D1 | **Founder decision on data residency** (India only or India first) | The hosting decision (D2) and the choice of any bill-reading supplier (D3) | Founder | PRD G-14; market research Q7 |
| D2 | **Hosting decision.** Vercel is the leaning, decided later [Q3] | Anything that stores data; setting up the cloud account (I2) | You / founder | [Q3] |
| D3 | **Bill-reading trial:** synthetic bills first, then real bills, measuring accuracy, speed and cost against `build-vs-buy.md` | The build-vs-buy decision, then any Construction work that depends on bill reading | You | [Q5] [Q12] |
| D4 | **Real bills gathered and labelled by you and the founder** | The real-bill repeat of the trial (D3); any check going live (PRD NFR-05) | You / founder | [Q6] [Q11] |
| D5 | **Outside security testing and an incident plan** arranged | Launch | You / founder | PRD NFR-08, NFR-11 |

## Assumptions & Open Questions

- The likelihood and impact scores for R1–R9 are this stage's conservative estimates, not measurements.
- D1 (residency) has no date yet; it should go into the next weekly update to the founder (`intent-statement.md`).
