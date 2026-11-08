# Bolt Plan — Hospital Bill Check (Phase 1)

A **Bolt** is one build pass over a small piece of the work that ends in something that runs and is merged into `main`. This plan orders the Bolts for Construction: which units and stories each covers, when it counts as done, what shipping it will tell us, and what it demos. Bolts are grouped into seven **milestones**, points where a recognisable piece of the product works.

The units, their dependencies and the stories each unit delivers are fixed in `inception/units-generation/unit-of-work.md`, `unit-of-work-dependency.md` and `unit-of-work-story-map.md`. Every Bolt builds against the contracts in `inception/contract-design/contract-summary.md`. Why the order is what it is, is in `risk-and-sequencing-rationale.md`; who builds each Bolt, in `team-allocation.md`; outside blockers, in `external-dependency-map.md`.

## Sources

- Answers Q1 to Q6 and the follow-up Q6a in `delivery-planning-questions.md` in this folder, confirmed through the consolidated summary.
- Team practices: `inception/practices-discovery/team-practices.md` and `aidlc/spaces/default/memory/team.md` (walking skeleton first and gated; short-lived branches merged within 1 to 2 days; squash-merge; test-after; 80% line coverage; synthetic bills only; nothing deployed until hosting is decided).
- Units and dependencies: `inception/units-generation/unit-of-work.md`, `unit-of-work-dependency.md`, `unit-of-work-story-map.md`.
- Contracts: `inception/contract-design/contract-summary.md`.
- Components: `inception/domain-design/components.md`. Requirements: `inception/requirements-analysis/requirements.md`. Stories: `inception/user-stories/stories.md`. Screens: `inception/refined-mockups/mockups.md`.

## How Construction runs

- **Unit by unit (Q6 A).** Each unit goes through Functional Design, NFR Requirements, NFR Design, Infrastructure Design and Code Generation before the next unit starts. The unit order below follows the dependency map and the risk-then-value choice (Q1 A).
- **Walking skeleton with stand-ins (Q6a A).** The walking skeleton is the thinnest version of the product that runs end to end, built to prove the parts connect. Bolt 1 builds it with the contracts unit: the real interfaces plus thin stand-ins for the six other units on its path. Each later unit replaces its stand-in, and the contracts keep the swap safe.
- **Thin slices (Q2 A).** Each Bolt after Bolt 1 covers 1 to 3 stories and merges within 1 to 2 days. Bolt 1 is split into three merge-sized slices and is gated once, after the last slice.
- **One at a time.** There is no parallel building of independent units in this plan; that follows from building unit by unit.
- **Built locally until hosting is decided (Q3 A).** Bolts run against synthetic bills and local stand-ins for stores, the job runner and the scheduler. Bolts that need a real host are marked **deploy-blocked**: they are built and tested, but not deployed.

### Unit order

| Order | Unit | Milestone | Why here |
|---|---|---|---|
| 1 | U1 contracts (with skeleton stand-ins) | M1 | Every unit depends on it; carries Bolt 1 |
| 2 | U4 bill-analysis | M2 | Riskiest part: reading accuracy and the checks (Q1, Q5) |
| 3 | U9 rules-access | M3 | Checks and the runner need real rule snapshots |
| 4 | U12 accuracy-runner | M3 | Measures the biggest worry as early as possible (Q5) |
| 5 | U6 findings-report | M4 | Turns check results into what families read |
| 6 | U7 feedback-insights | M4 | Depends on bill-analysis and findings-report |
| 7 | U10 test-set-quality | M5 | Required before document-intake by the dependency map; promotion to live needs it |
| 8 | U3 document-intake | M6 | First unit of the family journey |
| 9 | U5 case-processing | M6 | Needs intake, bill-analysis, findings-report, rules-access |
| 10 | U2 session-privacy | M6 | Needs every session-data owner to exist |
| 11 | U8 family-ui | M6 | Needs every family-side unit |
| 12 | U11 internal-ui | M7 | Needs rules-access and test-set-quality; before launch |

## Definition of Done for every Bolt

A Bolt is done only when all of these hold. Each Bolt's own "done when" adds to this list.
- Its stories' acceptance criteria pass on synthetic bills, with tests written after the code for each layer (test-after).
- Line coverage on application source stays at or above 80%.
- The automatic checks pass on GitHub Actions: lint, type check, format, Jest, Playwright, secret scan, `npm audit` (high and critical block), package signatures, and the code security scan (high and critical block).
- No real bill, redacted or not, is in the repository, fixtures, logs or screenshots.
- Pages with bill data are rendered per request, never cached or prefetched with bill data.
- The branch is squash-merged into `main` through a pull request named by the Bolt slug.

## M1 Walking skeleton (U1 contracts, with stand-ins)

Bolt 1 is the walking skeleton. It is built solo and gated: the founder approves it before Bolt 2 starts, and then decides whether the remaining Bolts run on their own or are gated one by one (`team.md`). Its three slices merge separately; the founder's approval comes after slice 1c.

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| **1a** skeleton: repository and checks | repository scaffolding (owned by Bolt 1) | none (foundation) | GitHub repository and Actions workflow run every check above on a pull request; `.claude/**` and `aidlc/**` in ESLint ignores; `typecheck` script runs `next typegen && tsc --noEmit`; Node `engines` field set; Prettier with `eslint-config-prettier`; Jest via `next/jest` and Playwright installed; `NEXT_TELEMETRY_DISABLED=1` in CI | That the checks pipeline can block a bad merge from day one | A pull request with a planted lint error and a planted fake secret, both blocked |
| **1b** skeleton: contracts and stand-ins | U1 contracts; stand-ins for U2, U3, U4, U5, U6, U8 | none (foundation) | TypeScript types and interfaces for C6 to C14, including `Result`, `SessionDataOwner`, `ScheduledJob` and `BetaHandoverSink`; one stand-in per skeleton unit that satisfies its interface; a test fails if any `sessionId`-keyed entity's owner is missing from the deletion list | That the contracts are buildable and a stand-in can sit behind each one | Type check passing; the deletion-list test failing when an owner is removed |
| **1c** skeleton: end to end | U1 plus the stand-ins; real deletion in the document-intake stand-in | touches US2.1, US7.5, US5.4 (thin versions only) | A synthetic bill uploads through the REST upload routes, a stand-in job "reads" it, a stub report shows on screen and downloads as a device-built PDF, and the original file is deleted once read, including when reading fails; one Jest unit test and one Playwright end-to-end test cover this path | That the pieces connect end to end and the "delete once read" guarantee holds from the first slice | Upload a synthetic bill on a phone-sized browser, see and download the stub report, show the original is gone |

## M2 Bill reading and checks (U4 bill-analysis)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 2 | U4 | US3.8 | Reader-neutral adapter (C3) with the recorded-output adapter for tests; header, lines and totals from digital PDFs with confidence per value | That one adapter shape can hold a real reader's output | A synthetic PDF read into lines with confidence |
| 3 | U4 | US3.10, US3.11 | Photos and scans read; discharge summary read; first reader-trial run on synthetic bills for each candidate reader (OQ8) | Which reader candidates are worth a real-bill trial | Reader-trial table on synthetic bills |
| 4 | U4 | US3.6, US3.12 | Unread lines listed and excluded; every line mapped to a catalogue entry and one of the 21 categories | That categorisation is accurate enough to check against | A synthetic bill's lines with categories |
| 5 | U4 | US3.1, US3.4 | Case details pre-filled and confirmable; low-confidence fields held until confirmed | That confirmation removes most reading doubt | Case details with unsure fields marked |
| 6 | U4 | US3.5, US3.7 | Line fixes recorded; cross-document mismatches flagged | That fixes feed cleanly into a re-run | A mismatched UHID flagged; a fixed line |
| 7 | U4 | US8.3, US4.1 | Check runner with live, shadow and off from a rule snapshot (fixture until U9); calculation checks | That checks are deterministic: same lines and rules, same results (NFR6) | Two identical runs, identical outcomes |
| 8 | U4 | US4.2, US4.3, US4.4 | Duplicate-charge, length-of-stay and room-rate checks | That planted errors are found on synthetic bills | Planted duplicates found |
| 9 | U4 | US4.5, US4.6 | Drug, consumable and implant price checks against MRP, NPPA ceilings and device caps (fixture data until U9) | That price checks need only public reference data | An above-ceiling drug found |
| 10 | U4 | US4.7, US4.8 | Package check from the hospital's own words only; document checks DOC-01 to DOC-08 | That missing documents become guidance, not errors | A bill with no package letter shows "could not run" |

## M3 Rules and first accuracy numbers (U9 rules-access, U12 accuracy-runner)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 11 | U9 | US8.4 | Rule library store, versions and snapshots; launch reference data (NPPA, device caps, IRDAI Lists II to IV, grievance timelines) loaded with sources | That pinned versions make results reproducible | A snapshot pinned and replayed |
| 12 | U9 | US10.2 | Staff sign-in (provider per NFR Requirements; local stand-in until chosen), roles and access log | That internal access is enforced before any internal screen exists | A signed-out request refused; an access logged |
| 13 | U9 | US8.1, US8.2 | Data-only changes with source, dates, version, owner and review date; approval, with self-approval logged; check set live only with a promotion record | That a wrong value can be fixed without a release, safely | A price fix approved and applied to the next run |
| 14 | U12 | US9.4 | Offline runner measures line accuracy and finding precision; writes the strict result file (C5) | **The first honest read on the biggest worry**: how close reading and checks are to their targets on synthetic bills | Accuracy figures per check |

## M4 Findings, report and feedback (U6 findings-report, U7 feedback-insights)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 15 | U6 | US4.9, US4.10, US4.11 | Findings with four-part explanations; grouping with no rupee counted twice; Bill Breakdown | That findings read as fair and specific | A grouped finding with 12 evidence lines |
| 16 | U6 | US4.12, US4.13, US4.14 | Low-confidence folding; "Who to ask" and the Complex label from labelling settings; checks run and not run | That clinical judgments are routed away from the billing desk | A Complex finding with no clinical opinion |
| 17 | U6 | US3.3, US5.1 | First look (count and topics only); Top 3 | That the first look is useful before confirmation | First look on a synthetic bill |
| 18 | U6 | US5.2, US5.3, US6.1 | Report content (seven parts) and Billing Desk Summary; Will ask / Not pursuing marks | That the report holds together as one document | Report content and one-page summary |
| 19 | U6 | US6.2, US5.5 | Dispute pack; glossary and plain-language checks (reading-ease score at least 60) | That the plain-language bar is measurable | Reading-ease score on generated text |
| 20 | U6 | US5.6, US5.7, US5.8 | Letters; grievance escalation and NPPA pointer; escalation ladder with deadlines | That letters need little editing | Generated letters for a synthetic case |
| 21 | U6 | US5.9, US5.10 | Next-time tips (with the bereaved-family variant); wording guard on every output | Zero banned terms across a generated corpus | Wording-guard report |
| 22 | U7 | US6.4, US6.5, US6.6 | Ratings with fixed reasons; plan answer; anonymous Insurance Check total | That feedback stays unlinked where required | Ratings stored; total counted once |
| 23 | U7 | US7.2, US7.7 | Consented record assembled and de-identified at session end; separate store; 3-month deletion job (local scheduler stand-in) | That de-identification removes all five identifiers, including inside line text | A stored record with identifiers removed |

## M5 Labelled Test Set (U10 test-set-quality)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 24 | U10 | US9.1, US9.2 | Test bills with sources and consents; redaction on receipt; blind labelling and reconciliation | That labelling can be done without either labeller seeing the other's labels or AI output | Two blind labels reconciled on a synthetic bill |
| 25 | U10 | US9.3, US9.5 | Withdrawal (caches cleared, 30-day backup expiry recorded); beta hand-over sink | That withdrawal leaves nothing behind | A withdrawn bill absent from the next run |
| 26 | U10 | US10.1, US10.3 | Import of runner results; promotion gate (10 with the error, 30 without, precision target); dashboard | That a check can go live only with a recorded pass | A promotion refused, then accepted |

## M6 Family journey (U3, U5, U2, U8)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 27 | U3 | US2.1, US2.11, US2.3 | Real upload with limits (20 files, 25,000,000 bytes, 60 pages); "When are you checking?" | That limits are named clearly and kept files stay | The 21st file refused with the limit named |
| 28 | U3 | US2.4, US2.10 | Document types; resumable 4 MiB chunked upload | That uploads survive a dropped connection on throttled 4G | Drop and resume mid-upload |
| 29 | U3 | US2.8, US2.5, US2.6 | Password PDFs; page order, duplicates and missing pages; re-upload of unreadable pages | That page prompts catch common bill problems | A missing page named |
| 30 | U3 | US2.7, US2.9, US7.5 | Summary-bill help; document checklist; originals deleted once read (real store) | That no original survives reading, in any path | Temp folders and logs checked after a failed read |
| 31 | U5 | US3.9, US6.7 | Background job with progress, arrival order, re-run from the changed step, cancel; worker with scheduled jobs; read budgets and 6-page concurrency (D-CD15) | Whether first-look and full-report times fit NFR1 and NFR2 locally | Progress screen through a full run. **Deploy-blocked** for the real worker host |
| 32 | U2 | US7.9, US1.1, US1.3 | Session cookie; no-account start; privacy notice | That nothing about the user is collected | Data inventory showing no profile fields |
| 33 | U2 | US1.4, US1.5 | Processing consent or decline; relationship | That gates hold on the server against direct requests | A direct upload request refused |
| 34 | U2 | US1.6, US1.7 | Guardian declaration; adult consent or authority, including a deceased patient | That every declaration path is enforced | Each declaration path |
| 35 | U2 | US7.4, US7.3 | Idle warning at 25 minutes, end at 30 by the sweep job; Discard everything through `endSession` | That an abandoned session's data is gone within 31 minutes | Session ended by the sweep; owners all confirm deletion |
| 36 | U8 | US1.2, US3.2, US2.2 | Explainer screen; "where am I" progress; camera capture guidance | That the first two screens get a family to upload | Start to upload on a phone-sized browser |
| 37 | U8 | US7.6, US7.8 | No-cache and no-prefetch rules on every bill-data route; WCAG 2.2 AA checks | That bill data never leaks between sessions and every screen passes accessibility checks | Two sessions side by side; accessibility report |
| 38 | U8 | US5.4, US5.11 | Device-built PDF on desktop; phone share sheet with nothing sent to the server | That reports leave the device only through the user's own share | Save or share on a phone |
| 39 | U8 | US7.1, US6.3 | Before-you-go reminder and "I'm done"; nothing sent on the user's behalf | That the journey ends cleanly | Full journey from start to finish |

## M7 Internal tools and launch readiness (U11 internal-ui)

| Bolt | Units | Stories | Done when | It will show us | Demo |
|---|---|---|---|---|---|
| 40 | U11 | US8.5 | Rule library and check-state screens; promotion screen | That a check can be paused without a release | A check switched to shadow |
| 41 | U11 | US10.4 | Dashboard and support material; no case lookup anywhere | That support cannot reach anyone's bill | Support material screen |
| 42 | all | none (launch readiness) | Hosting and residency decided; staging deployment; timing and load test at 10 times pilot volume; beta hand-over switched off with a test that no original is kept; counsel review of privacy wording and the accepted re-identification risk | That the product meets NFR1, NFR2 and NFR14 on its real host | Staging run at 10 times pilot volume. **Deploy-blocked** until hosting is decided |

## Story coverage

All 85 in-scope stories are assigned to exactly one Bolt between Bolt 2 and Bolt 41, matching their units in `unit-of-work-story-map.md`. Bolt 1 builds only thin versions of US2.1, US7.5 and US5.4, which their later Bolts (27, 30 and 38) complete. The 8 Won't Have stories (US11.1 to US11.8) are in no Bolt.

## Assumptions & Open Questions

- The Bolt order follows the unit order above. The workflow walks units in dependency order, so the plan follows that order and adds no deviation from it.
- Bolt sizes are estimates for one builder working with the AI assistant. If a Bolt cannot merge within 2 days, it is split rather than extended.
- Milestone M3's accuracy figures come from synthetic bills only. Real-bill accuracy needs the test-set workflow (M5), consented bills, and a reading service that has passed the data-processor check (`external-dependency-map.md`).
- How the remaining Bolts run after Bolt 1, on their own or gated one by one, is decided after Bolt 1 (`team.md`).
