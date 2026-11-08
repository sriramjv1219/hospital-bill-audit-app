# Units Generation — Questions

This stage groups the 15 building blocks from Domain Design (`inception/domain-design/components.md`) into units of work. A unit is a piece that is designed, built and tested as one. In Construction, every unit goes through Functional Design, NFR Requirements, NFR Design, Infrastructure Design and Code Generation, so the number of units sets how many design passes you will review. This stage maps what can depend on what. It does not decide what to build first; that is Delivery Planning (next but one).

## Already settled, not asked again

- **One codebase on the existing Next.js 16 / React 19 / TypeScript app** (constraint CR-T9; `codekb/.../architecture.md`).
- **Hosting is not decided; Vercel is a working assumption only** (OQ2). Units must not depend on a particular host.
- **The first build slice is a thin end-to-end path**: upload, stub report, deletion of the original, test tools and the automatic checks (`team.md`, Walking Skeleton). Delivery Planning decides which units it touches.
- **One builder plus the founder** (CR-O1). There is no team to split units between.
- **Block-to-block calls are synchronous, with one coordinator** (Domain Design ADR-003).

## Carried forward from Domain Design

The architecture reviewer raised five points that you approved as accepted risks. Per the project rule, they are raised here first:

- R-01: how pinned rule-library data reaches the check engine, bill model and findings blocks (intended: the coordinator passes it in);
- R-02: what happens when storing the consented record fails at session close;
- R-03: what happens when handing a beta bill to the test set fails;
- R-04: no alerting service is listed, though several blocks raise alerts;
- R-05: the session-deletion list of 8 blocks is kept by hand.

---

## Q1. How to handle the five carried-forward review points (R-01 to R-05)

A. Carry all five into Contract Design as explicit items, since each one is about what crosses a boundary (data passed in, failure behaviour, a required "delete session data" interface, an alerting dependency)
B. Record the reviewer's suggested answers now as unit notes (coordinator passes rule data in; deletion always proceeds and a failed consented record is dropped with an alert; a failed beta hand-over still deletes the original; alerting is an external dependency; deletion list checked by a test), and let Contract Design formalise them
C. Leave them as accepted risks with no further action
X. Other (please specify)

[Answer]: B

## Q2. How many units

Each unit gets its own pass through the five Construction design and build stages, at the Comprehensive depth this project runs at.

A. About 10 units, grouping blocks that change and are tested together:
   - session and privacy (SessionConsent);
   - document intake and reading (DocumentIntake, BillReading);
   - bill model (BillModel);
   - check pipeline (CaseProcessing, CheckEngine);
   - findings, report and letters (FindingsReport, LettersGuidance);
   - feedback and consented insights (Feedback, ConsentedInsights);
   - family screens (FamilyJourneyUI);
   - rule library and internal access (RuleLibrary, InternalAccess);
   - test set and quality (TestSetQuality);
   - internal screens (InternalToolsUI)
B. About 5 coarser units:
   - family journey (FamilyJourneyUI, SessionConsent, DocumentIntake);
   - bill processing (BillReading, BillModel, CaseProcessing, CheckEngine);
   - reporting (FindingsReport, LettersGuidance, Feedback, ConsentedInsights);
   - rules and access (RuleLibrary, InternalAccess);
   - test set and internal tools (TestSetQuality, InternalToolsUI)
C. One unit per building block (15 units)
X. Other (please specify)

[Answer]: A

## Q3. How the product is deployed

Processing can take up to 15 minutes per check (NFR2), and 3-month deletion runs on a schedule (FR2.9). Many hosts limit how long a web request may run.

A. One Next.js app for all screens and routes, plus a separate background worker built from the same codebase that runs processing jobs and the scheduled deletion. This works on any host
B. Everything inside the one Next.js deployment, using the host's own background-function and scheduled-job features
C. Record every unit as part of one deployable for now, and decide the worker question in Infrastructure Design once hosting is chosen
X. Other (please specify)

[Answer]: A

## Q4. Where the internal tools run

The internal tools reach redacted real bills (test set) and change live rules. Family screens are anonymous and public.

A. Same Next.js app, in a separate route area with its own sign-in and role checks (ADR-012)
B. A separate internal app, deployed on its own and reachable only by the builder and founder
C. Same codebase, built and deployed as a separate internal deployment from the same code
X. Other (please specify)

[Answer]: A

## Q5. How units share their types and interfaces

Several units use the same shapes: the bill model is read by the check pipeline, findings and consented insights; findings are read by feedback and insights.

A. A small shared contracts unit (a "spec" unit) holding the types and interfaces that cross unit boundaries; every unit depends on it and nothing else is shared
B. Each unit publishes its own types, and consumers import them from that unit directly
C. Leave it to Contract Design (next stage) to decide
X. Other (please specify)

[Answer]: A

## Q6. How units talk to each other at runtime

A. In-process calls through each unit's module interface (one codebase), with no internal network APIs. The worker, if any, runs the same code and reaches each unit's data only through that unit
B. Internal HTTP APIs between units, even inside one deployment
C. A message queue between the coordinator and the steps, with direct calls elsewhere
X. Other (please specify)

[Answer]: A

## Q7. Where the accuracy runs happen

Real bills in the test set must stay outside git and out of the AI coding assistant's reach, and accuracy runs may use only approved services (FR16.13, team practice).

A. The accuracy runner is a separate command-line tool (a "packaging" unit) run on a machine kept outside the assistant's reach; labelling, withdrawal, promotion and the dashboard stay in the internal tools
B. Accuracy runs are started from the internal tools and run on the deployed service like everything else
C. Both: the internal tools can start a run, and a command-line runner exists for offline runs
X. Other (please specify)

[Answer]: A

## Q8. Building independent units side by side

Some units do not depend on each other (for example, rule library and feedback). Later, Construction can design or build such units side by side, for example with helpers working in parallel.

A. Allow it: units with no dependency between them may be designed and built in parallel
B. One unit at a time, always in an order the dependencies allow
X. Other (please specify)

[Answer]: A

Q7 note: you first answered "I dont know....can you recommend the cheapest and accurate approach?"; after the recommendation (same accuracy either way; cheapest because only the bill reader's per-page cost applies and nothing extra is hosted; works before hosting and residency are decided; real bills stay on one machine outside the assistant's reach, with only IDs and scores leaving it) you chose A.

## Consolidated Summary Confirmation

- Q1: record the reviewer's suggested answers to R-01 to R-05 now as unit notes, and let Contract Design formalise them: the coordinator passes the pinned rule data in; session deletion always proceeds and a consented record that fails to store is dropped with an alert; a failed beta hand-over still deletes the original; alerting is an external dependency; the deletion list is checked by a test against every session-keyed entity (B).
- Q2: about 10 units grouping blocks that change and are tested together: session and privacy; document intake and reading; bill model; check pipeline; findings, report and letters; feedback and consented insights; family screens; rule library and internal access; test set and quality; internal screens (A).
- Q3: one Next.js app for all screens and routes, plus a background worker built from the same codebase that runs processing jobs and the scheduled deletion (A).
- Q4: internal tools in the same app, in their own route area with their own sign-in and role checks (A).
- Q5: a small shared contracts unit holds the types and interfaces that cross unit boundaries; every unit depends on it (A).
- Q6: units call each other in-process through module interfaces, with no internal network APIs; the worker runs the same code (A).
- Q7: accuracy runs use an offline command-line runner on a machine outside the AI assistant's reach; only bill IDs and scores go into the internal tools (A).
- Q8: units with no dependency between them may be designed and built in parallel (A).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
