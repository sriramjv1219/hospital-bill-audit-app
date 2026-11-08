# Approval & Handoff — Questions

These questions close the ideation work before detailed planning (Inception) starts. The answers shape the initiative brief and the go/no-go recommendation.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- Earlier ideation work, all under `ideation/`:
  - `intent-capture/intent-statement.md` and `intent-capture/stakeholder-map.md`;
  - `market-research/competitive-analysis.md`;
  - `feasibility/feasibility-assessment.md`, `feasibility/constraint-register.md` and `feasibility/raid-log.md`;
  - `scope-definition/scope-document.md` and `scope-definition/intent-backlog.md`;
  - `rough-mockups/wireframes.md`, `rough-mockups/user-flow.md` and the design check on them (`rough-mockups/reviews/review-01.md`).

Already settled, so not asked again:

- **The decision-maker.** The founder decides scope and priority; status goes to the founder weekly (`stakeholder-map.md`).
- **The product boundary and scope.** The Phase 1 Hospital Bill Check MVP: every Must Have and Should Have except urgent-situation guidance, plus the internal tools in their leanest form. Could Have items are deferred (`scope-document.md`).
- **No fixed timeline** (`intent-statement.md`).
- **Order of work.** Value first, with the bill-reading trial running first (`scope-document.md`, section 5).
- **Feasibility verdict.** Feasible with conditions F1–F5 (`feasibility-assessment.md`, section 7).
- **Team formation was skipped.** You build alone with this AI-assisted workflow. You and the founder label real bills (`feasibility-questions.md`, Q1 and Q11).

---

## Q1. What should happen to the eight rough-mockup changes we agreed?

We went through the eight points from the design check (R-01 to R-08) and agreed a change for each. You then approved the rough mockups as they were, so none of the changes is in the sketches yet. Two of them matter before launch: the missing way to add a document or fix a line after the report appears (R-01, a Must Have), and the privacy wording that needs a check (R-02).

A. Carry all eight agreed changes forward as inputs to Requirements Analysis and Refined Mockups, and list them in the initiative brief
B. Carry forward only the two that matter before launch (R-01 and R-02); drop the six minor ones
C. Drop them; the sketches stand as approved
X. Other (please specify)

[Answer]: A

## Q2. Has the founder agreed to the intent and scope?

The founder is the only decision-maker (`stakeholder-map.md`). The brief will say whether the founder has signed off, and whether detailed planning waits for that sign-off.

A. Yes, the founder has already agreed to the intent and scope
B. Not yet: send the initiative brief to the founder in the next weekly update, and start detailed planning in the meantime
C. Not yet: wait for the founder's sign-off before detailed planning starts
X. Other (please specify)

[Answer]: A

## Q3. Do you accept the main risks with their planned responses?

The highest-scoring risks (`raid-log.md`, `scope-document.md`) are:

- bill reading may not reach the accuracy targets on real Indian bills, and a trial on synthetic bills may overstate accuracy (R1, R2);
- one builder carries the whole build (R3);
- false findings or a domain-knowledge gap could damage trust (PRD RK2, RK27);
- launching without legal review (PRD RK28, accepted by the founder);
- with pay-under-protest guidance moved to Phase 1.5, the patient-welfare risk stays partly open; the only cover is the rule against encouraging families to withhold payment (PRD RK15, C10).

A. Yes, accept them with the responses already planned
B. Yes, but raise one or more risks for the founder to decide on (name which under X)
C. No, one or more responses need changing first (say which under X)
X. Other (please specify)

[Answer]: A

## Q4. Should a running-cost budget be set?

No budget or cost ceiling is set (`constraint-register.md`, CR-O6). Phase 1 is free, so every audit costs money and earns nothing. The bill-reading trial will measure cost per audit.

A. No budget for now: record cost per audit in the trial, then propose a ceiling to the founder once hosting is chosen
B. Ask the founder to set a monthly ceiling before any work that runs bill reading at volume
C. Set a ceiling now (give the amount under X)
X. Other (please specify)

[Answer]: A

## Q5. Is the market research enough to proceed?

The research found a thin field of direct competitors: one live free checker and one concept. It found five clear differentiators. It also found a gap: the free checker shows the effect of the room-rent cap, which Phase 1 does not (`competitive-analysis.md`). Several facts are marked as needing a check against a primary source.

A. Yes, it is enough to proceed; the claims still to check stay as open items
B. Proceed, but run a deeper search for Indian free bill checkers before Requirements Analysis
C. No, more market research is needed before going further (say what under X)
X. Other (please specify)

[Answer]: A

## Q6. Who is on the team for the build?

Team formation was skipped. The plan as recorded is:

- you build alone with this AI-assisted workflow;
- the founder makes the decisions and is the second labeller of real bills;
- there are no domain experts, and no legal review before launch.

A. That is right
B. Someone else is joining (say who and their role under X)
C. The founder's role is different (describe it under X)
X. Other (please specify)

[Answer]: A

## Q7. Which founder decisions must be made before detailed planning ends?

Three decisions are still open with the founder:

- data residency: India only, or India first (PRD G-14);
- hosting, which depends on residency; Vercel is the working assumption;
- how to measure whether billing desks actually correct bills (PRD G-02).

The feasibility work says residency must come before hosting, and both must come before building anything that stores data.

A. Residency and hosting before the build starts (end of Inception); the bill-correction measure can wait until launch
B. All three before the build starts
C. Residency before Requirements Analysis ends; hosting before the build starts; the bill-correction measure before launch
X. Other (please specify)

[Answer]: B

## Q8. What is your go/no-go decision?

The recommendation is **Go, with conditions**. Proceed to detailed planning, and track feasibility conditions F1–F5 (the bill-reading trial, the residency decision, the hosting confirmation, gathering real bills, and the incident plan with outside security testing) as gates on the build and the launch.

A. Go: proceed to detailed planning, with conditions F1–F5 tracked
B. Go, but run the bill-reading trial (F1) before any further planning
C. Hold: do not proceed until the open items are resolved (name them under X)
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Rough-mockup changes (Q1): carry all eight agreed changes (R-01 to R-08) forward as inputs to Requirements Analysis and Refined Mockups, and list them in the initiative brief.
- Founder (Q2): the founder has already agreed to the intent and scope.
- Risks (Q3): accepted with the responses already planned in the risk log.
- Running cost (Q4): no budget for now; the trial records cost per audit, and a ceiling is proposed to the founder once hosting is chosen.
- Market research (Q5): enough to proceed; the claims still to check stay as open items.
- Team (Q6): you build alone with this AI-assisted workflow; the founder decides and is the second labeller; no domain experts and no legal review before launch.
- Founder decisions (Q7): data residency, hosting and the bill-correction measure are all decided before the build starts.
- Go/no-go (Q8): Go. Proceed to detailed planning, with conditions F1–F5 tracked as gates on the build and launch.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
