# Risk and Sequencing Rationale — Hospital Bill Check (Phase 1)

This document explains why the Bolts in `bolt-plan.md` come in the order they do. A **Bolt** is one build pass over a small piece of the work that ends in something that runs and is merged.

## Sources

- Answers in `delivery-planning-questions.md` in this folder: Q1 (risk first, then value), Q2 (thin slices), Q5 (biggest worry: accuracy), Q6 and Q6a (unit by unit, skeleton with stand-ins).
- Team practices: `inception/practices-discovery/team-practices.md` (walking skeleton first).
- Dependency map: `inception/units-generation/unit-of-work-dependency.md`. Unit sizes: `unit-of-work.md`.
- Accuracy targets and speed targets: `inception/requirements-analysis/requirements.md` (NFR1, NFR2, NFR4).
- Contracts: `inception/contract-design/contract-summary.md` (stand-ins sit behind its interfaces).

## The approach

Three heuristics are used, in this order:

1. **Walking skeleton first** (Cockburn, *Crystal Clear*). A walking skeleton is the thinnest version of the product that runs end to end. Bolt 1 builds it before any feature, to prove the parts connect. Team practice requires it (`team-practices.md`).
2. **Risk first** (Boehm, spiral model). The most uncertain parts are built next, so later work is built on known ground. The biggest uncertainty here is whether bill reading and the checks reach their accuracy targets (NFR4); you named it as your biggest worry (Q5). So bill-analysis (M2), real rule data (M3) and the accuracy runner (M3) come straight after the skeleton.
3. **Value first** for the rest. Once the core is proven, the work that families see comes next (M4 report and letters, M6 the journey), with the internal tools last but before launch (M7).

A formal scoring model was not used. WSJF (Weighted Shortest Job First: value plus urgency plus risk reduction, divided by size) was offered (Q1 C) and not chosen.

## How the order respects the dependency map

The workflow builds units in an order the dependency map allows. The order in `bolt-plan.md` is one such order:

contracts → bill-analysis → rules-access → accuracy-runner → findings-report → feedback-insights → test-set-quality → document-intake → case-processing → session-privacy → family-ui → internal-ui

- Every unit comes after everything it depends on, so there is **no deviation from the dependency order**.
- One consequence: test-set-quality (M5) must come before document-intake. The reason is the closed-beta hand-over link in the dependency map (Domain Design ADR-014). So part of the internal test-set workflow is built before the family journey, even though "value first" would put the journey first. This also helps the biggest worry, because the test set is how real-bill accuracy gets measured.
- If the closed beta does not happen, that link could be removed from the dependency map in a later change, and the family journey could move ahead of the test set.

## Why the skeleton uses stand-ins

Building unit by unit (Q6 A) means each unit is fully designed and built before the next. The skeleton runs through seven units. Built fully first, those seven would push the first end-to-end run late. With stand-ins (Q6a A):
- Bolt 1 builds the real contracts plus a thin stand-in for each of the six other units on the path.
- The skeleton runs end to end in Bolt 1.
- Each later unit replaces its stand-in. The contracts and their typed results catch any mismatch at the type check.

The one stand-in that is not thin is document-intake's: it deletes originals for real from Bolt 1, because that guarantee must hold from the first slice (`team.md`, Walking Skeleton).

## Bolt size

Most units are large: bill-analysis and family-ui are over two weeks each for one builder. Team practice merges each branch within 1 to 2 days, so Bolts are thin slices of 1 to 3 stories (Q2 A), grouped into seven milestones. Bolt 1 is split into three merge-sized slices with one founder approval at the end.

## Risk register

| Risk | Likelihood | Impact | How the plan handles it |
|---|---|---|---|
| Bill reading or checks miss their accuracy targets (NFR4) | High | Critical: checks cannot go live | Reader trial in Bolt 3; first accuracy figures in Bolt 14 (M3), before most other code; checks stay in shadow until they pass (FR16.12) |
| Hosting and residency decided late (OQ1, OQ2) | Medium | High: no deployment, no real-bill measurement, no timing test | Build locally with stand-ins (Q3 A); host-dependent Bolts marked deploy-blocked; decision needed before Bolt 42 |
| One builder runs out of time | Medium | High | Thin slices make progress visible; the riskiest work is first, so a cut would come from late, lower-value Bolts |
| Page reading too slow for 5 and 10 minute first look at volume (NFR1) | Medium | High | Read budgets and 6-page concurrency in the contracts (D-CD15); measured locally in Bolt 31, at volume in Bolt 42 |
| Stand-ins hide integration problems until late | Medium | Medium | Contracts are typed and every unit returns typed results; the end-to-end test from Bolt 1c re-runs on every merge, so a real unit that breaks the path fails at once |
| Privacy guarantee broken by a bug (deletion, caching, de-identification) | Low | Critical | Deletion is real from Bolt 1c; the deletion-list test from Bolt 1b; no-cache checks in Bolt 37; de-identification tests in Bolt 23 |
| Counsel review of privacy wording or the accepted re-identification risk asks for changes | Medium | Medium | Planned into Bolt 42; the consented-record assembly is isolated in feedback-insights, so a change stays there |

## Assumptions & Open Questions

- The order assumes the closed beta stays in scope. If it is dropped, the beta link in the dependency map can be removed and the family journey can move earlier.
- The first accuracy figures (Bolt 14) use synthetic bills. They show whether the approach works, not whether real bills meet the targets; that needs M5 and consented real bills.
