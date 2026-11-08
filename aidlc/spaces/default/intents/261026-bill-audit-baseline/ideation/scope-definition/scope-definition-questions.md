# Scope Definition — Questions

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- The approved intent statement (`ideation/intent-capture/intent-statement.md`): Phase 1 Hospital Bill Check MVP, including its internal modules and the P0 tooling it needs; Phase 1.5 onwards out of scope; no fixed timeline.
- The feasibility assessment and constraint register (`ideation/feasibility/`): feasible with conditions; a solo builder; a bill-reading trial before the build-vs-buy decision; no real bills yet.
- The PRD's Phase 1 scope (§10.2), out-of-scope list (§10.3, W1–W22), check rollout waves (§13.8) and MoSCoW list (§15) in `PRDs/app-baseline.md`.

Already settled, so not asked again:

- The product boundary is the Phase 1 Hospital Bill Check plus its internal modules and P0 tooling (intent Q12).
- There is no fixed timeline or hard deadline (intent Q11, Q17).
- The PRD's out-of-scope list (W1–W22) applies, including all insurance checks, languages other than English, accounts, notifications and payments.
- Where a PRD gap has a single suggested resolution, it is used as the decision (intent Q10). The questions below cover only the gaps whose suggestions offer a choice and that first matter at this stage (intent Q15).

---

## Q1. Which priority tiers of the PRD's feature list belong in this build?

The PRD sorts Phase 1 features into Must Have (M1–M22), Should Have (S1–S9 plus implant detail capture and test-set coverage reporting) and Could Have (a public knowledge hub; implant identity, operation-theatre duration and anaesthesia checks) (PRD §15). Your intent-capture answer set the boundary at Phase 1, but did not say which tiers.

A. Must Have and Should Have; Could Have items are deferred to a later build
B. Must Have, Should Have and Could Have: all of Phase 1
C. Must Have only; Should Have and Could Have are deferred
X. Other (please specify)

[Answer]: A

## Q2. What is the minimum first release?

The PRD's launch shape is all Must Have features, with the 18 most objective checks live and the 8 document checks as guidance. Wave 2 and Wave 3 checks follow after launch as each passes the Labelled Test Set (PRD §10.2, §13.8). Feasibility found that bill reading is unproven and that one person is building it (`feasibility-assessment.md`).

A. The PRD's launch shape: all Must Have features with the 18-check launch set and document guidance
B. A smaller first release for the closed beta testers only (bill reading, the launch checks and the report), then the remaining Must Have features before the public launch
C. A smaller public launch with fewer than 18 checks, adding the rest as each passes (please say which under X)
X. Other (please specify)

[Answer]: A

## Q3. Where should "typical quantity" data come from? (PRD gap G-03)

Several Wave 3 checks look for implausible quantities, for example too many gloves or tests for the length of stay. They need typical ranges, which the PRD sources from an expert panel and case data. Phase 1 has neither experts nor long-term case data. The PRD suggests three ways out.

A. Derive the ranges from the Labelled Test Set, which is not limited by the 3-month retention rule (PRD D30)
B. Search for a public source of typical quantities first, and use the test set only if none is found
C. Move these checks to Phase 1.5, outside this build
D. Keep them in this build in shadow mode (hidden from users) until a source exists
X. Other (please specify)

[Answer]: B

## Q4. How should two Must Have items that depend on Wave 3 checks be read? (PRD gap G-04)

The Must Have list includes flagging unusual charges and excessive consumables (AA-03, AA-04). The checks that deliver them come only in Wave 3, after launch. The PRD suggests two readings.

A. "Must" means the capability is built for launch; the checks appear to users only when they pass the Labelled Test Set
B. Move unusual-charge and excessive-consumable flagging to Should Have
X. Other (please specify)

[Answer]: A

## Q5. In what order should the work be sequenced?

Feasibility made a measured bill-reading trial a condition before any build work that depends on bill reading (`feasibility-assessment.md`, condition F1).

A. Risk first: run the bill-reading trial on synthetic bills and start the Labelled Test Set; then a thin end-to-end slice; then the remaining features
B. A thin end-to-end slice first (upload to report with one check), then the bill-reading trial, then the remaining features
C. Value first: the features users see first, then the internal tools
D. Foundations first: internal tools (rule library, labelling, ops/admin) before user features
X. Other (please specify)

[Answer]: C

## Q6. How full should the internal tools be in this build?

The internal tools must meet their requirements:

- rule library with versioning, sources, on/off and shadow switches (M19);
- ops views, role-based access, audit logs and a quality dashboard (M20);
- Labelled Test Set tooling (M15, LT-01–LT-11).

You are building alone (feasibility Q1), so how much effort goes into internal screens matters.

A. The leanest form that meets each requirement; internal screens stay minimal and are improved later
B. Full internal tools, built to the same standard as the user-facing product
C. Decide in the later design stages
X. Other (please specify)

[Answer]: A

## Q7. How should "pay under protest" and urgent-situation guidance be prioritised? (PRD gap G-12)

The PRD relies on guidance to pay under protest and pursue the dispute later, so families do not delay discharge or treatment (constraint C10, risk RK15 with impact 5). No requirement covers it yet, and urgent-situation guidance (TS-03, for when discharge or treatment is denied) is only a Should Have. The PRD suggests adding a requirement and "considering" raising TS-03 to Must Have.

A. Add a pay-under-protest guidance requirement as Must Have, and raise urgent-situation guidance (TS-03) to Must Have
B. Add the pay-under-protest requirement as Must Have, and keep urgent-situation guidance as Should Have
C. Keep both as Should Have
X. Other (please specify)

[Answer]: X. Other: "Move this scenario to phase 1.5"

## Q8. Follow-up to Q5: value first vs the bill-reading trial

In Q5 you chose value first: the features users see come first, then the internal tools. Feasibility made a measured bill-reading trial a condition before any build work that depends on bill reading (`feasibility-assessment.md`, condition F1), and you approved that. Almost every feature users see (upload, the checks, the report) depends on bill reading, so the two answers pull in different directions.

A. Value first, but run the bill-reading trial as the very first piece of work; user features start once it has screened the options
B. Value first, and build the user features in parallel with the trial; accept that some work may be redone if the trial changes the choice
C. Change the sequence to risk first after all (trial first, then a thin slice, then the rest)
D. Start the trial first; at the same time build the user features that do not depend on which bill-reading option wins (consent and upload screens, the checks working on already-extracted line items, the report and templates), and connect the chosen bill reader after the trial
X. Other (please specify)

[Answer]: D

## Consolidated Summary Confirmation

- Priority tiers (Q1): Must Have and Should Have are in this build; Could Have items are deferred to a later build.
- First release (Q2): the PRD's launch shape, with all Must Have features, the 18-check launch set and the 8 document checks as guidance; Wave 2 and Wave 3 checks follow as each passes the Labelled Test Set.
- Typical-quantity data (Q3, gap G-03): search for a public source first; derive ranges from the Labelled Test Set only if none is found.
- Wave 3 dependencies (Q4, gap G-04): unusual-charge and excessive-consumable flagging (AA-03, AA-04) are built for launch; their checks appear to users only when they pass the test set.
- Sequencing (Q5, Q8): value first. The bill-reading trial starts first; at the same time, the features that do not depend on the bill reader are built (screens, checks on extracted line items, report, templates); the chosen bill reader is connected after the trial.
- Internal tools (Q6): the leanest form that meets each requirement; internal screens stay minimal and are improved later.
- Pay-under-protest and urgent-situation guidance (Q7, gap G-12): "Move this scenario to phase 1.5". Both are out of this build. The rule against encouraging families to withhold payment in a way that blocks discharge (constraint C10) still applies to all report wording.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
