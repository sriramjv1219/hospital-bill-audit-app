# Unit Story Map — Hospital Bill Check (Phase 1)

Every user story in `inception/user-stories/stories.md` is mapped to the one unit that implements it (from `unit-of-work.md`). Stories that need work in other units too are listed with those units. Story order inside each unit follows the story dependencies in `stories.md`; it is not an order across units. Delivery Planning chooses that.

## Sources

- Units: `unit-of-work.md` and `unit-of-work-dependency.md` in this folder.
- Story-to-block mapping: `inception/domain-design/traceability.json` and `components.md` (each block sits in exactly one unit; US9.4 moves to accuracy-runner, D-UG6).
- Stories and their dependencies: `inception/user-stories/stories.md`; requirements behind them: `inception/requirements-analysis/requirements.md`.

## Story Map

| Story | Title | Unit ID | Directory | Also needs work in |
|---|---|---|---|---|
| US1.1 | Start a check without an account | U2 | u2-session-privacy | — |
| US1.2 | Understand what the check does and does not do | U8 | u8-family-ui | — |
| US1.3 | Read the privacy notice before uploading | U2 | u2-session-privacy | — |
| US1.4 | Give consent to process my documents, or decline | U2 | u2-session-privacy | — |
| US1.5 | Say whose bill this is | U2 | u2-session-privacy | — |
| US1.6 | Declare that I am the guardian of a child patient | U2 | u2-session-privacy | — |
| US1.7 | Declare consent or authority for an adult patient, including one who has died | U2 | u2-session-privacy | — |
| US2.1 | Upload bills and documents from files | U3 | u3-document-intake | — |
| US2.2 | Photograph a bill with guidance | U8 | u8-family-ui | — |
| US2.3 | Stay within the upload limits | U3 | u3-document-intake | — |
| US2.4 | Have each file recognised and corrected if needed | U3 | u3-document-intake | — |
| US2.5 | Be told about missing, duplicate or shuffled pages | U3 | u3-document-intake | — |
| US2.6 | Re-upload an unreadable page | U3 | u3-document-intake | — |
| US2.7 | Get help when I only have a summary bill | U3 | u3-document-intake | — |
| US2.8 | Open a password-protected PDF | U3 | u3-document-intake | — |
| US2.9 | See which documents I have and which are missing | U3 | u3-document-intake | — |
| US2.10 | Keep my upload going on a patchy connection | U3 | u3-document-intake | U8 (upload screen) |
| US2.11 | Say where I am in the hospital stay | U3 | u3-document-intake | — |
| US3.1 | Confirm the case details read from my documents | U4 | u4-bill-analysis | — |
| US3.2 | See where I am and what to do next | U8 | u8-family-ui | — |
| US3.3 | See a first look while I confirm a few details | U6 | u6-findings-report | — |
| US3.4 | Confirm the details the system is unsure about | U4 | u4-bill-analysis | — |
| US3.5 | See and fix the lines read from my bill | U4 | u4-bill-analysis | U5 (re-run after a fix), U8 (line editor) |
| US3.6 | Be told which lines could not be read | U4 | u4-bill-analysis | — |
| US3.7 | Be warned when my documents don't match each other | U4 | u4-bill-analysis | — |
| US3.8 | Read header, lines and totals from a digital PDF | U4 | u4-bill-analysis | — |
| US3.9 | Have my documents processed in the background while I see progress | U5 | u5-case-processing | U8 (progress screen) |
| US3.10 | Read photographed and scanned bills | U4 | u4-bill-analysis | — |
| US3.11 | Read the discharge summary | U4 | u4-bill-analysis | — |
| US3.12 | Sort each line into its category and add its details | U4 | u4-bill-analysis | — |
| US4.1 | Calculation checks | U4 | u4-bill-analysis | — |
| US4.2 | Duplicate-charge checks | U4 | u4-bill-analysis | — |
| US4.3 | Length-of-stay checks | U4 | u4-bill-analysis | — |
| US4.4 | Room-rate check | U4 | u4-bill-analysis | — |
| US4.5 | Drug-price checks | U4 | u4-bill-analysis | — |
| US4.6 | Consumable and implant price checks | U4 | u4-bill-analysis | — |
| US4.7 | Package check | U4 | u4-bill-analysis | — |
| US4.8 | Get guidance when documents or details are missing | U4 | u4-bill-analysis | — |
| US4.9 | Understand each finding | U6 | u6-findings-report | — |
| US4.10 | See similar findings grouped, with no rupee counted twice | U6 | u6-findings-report | — |
| US4.11 | See the Bill Breakdown | U6 | u6-findings-report | — |
| US4.12 | Have low-confidence findings folded away | U6 | u6-findings-report | — |
| US4.13 | Know whom to ask, and when to get professional advice | U6 | u6-findings-report | U9 (labelling settings) |
| US4.14 | Know which checks ran and which could not | U6 | u6-findings-report | — |
| US5.1 | See the top three things to do now | U6 | u6-findings-report | — |
| US5.2 | Get the full Audit Report | U6 | u6-findings-report | — |
| US5.3 | Get a one-page Billing Desk Summary | U6 | u6-findings-report | — |
| US5.4 | Download the report on desktop | U8 | u8-family-ui | U6 (report content) |
| US5.5 | Read the report in plain language | U6 | u6-findings-report | — |
| US5.6 | Get letters I can edit and send | U6 | u6-findings-report | — |
| US5.7 | Get an escalation letter and an NPPA complaint pointer | U6 | u6-findings-report | — |
| US5.8 | See the escalation ladder and its deadlines | U6 | u6-findings-report | — |
| US5.9 | Get tips for next time | U6 | u6-findings-report | — |
| US5.10 | Read respectful wording everywhere | U6 | u6-findings-report | U8, U11 (every output passes the wording guard) |
| US5.11 | Save or share the report from my phone | U8 | u8-family-ui | U6 (report content) |
| US6.1 | Choose which findings I will raise | U6 | u6-findings-report | — |
| US6.2 | Add a dispute pack to my report | U6 | u6-findings-report | — |
| US6.3 | Know that nothing is sent on my behalf | U8 | u8-family-ui | U6, U8 |
| US6.4 | Rate each finding | U7 | u7-feedback-insights | — |
| US6.5 | Say what I plan to do next | U7 | u7-feedback-insights | — |
| US6.6 | Say I want the Insurance Check | U7 | u7-feedback-insights | — |
| US6.7 | Add a document after the report | U5 | u5-case-processing | U3 (new document), U8 ("Add a document") |
| US7.1 | Be reminded to save before I go | U8 | u8-family-ui | — |
| US7.2 | Choose whether to share de-identified data after the report | U7 | u7-feedback-insights | U2 (storage consent, close-time hand-over) |
| US7.3 | Discard everything at any point | U2 | u2-session-privacy | U3, U4, U5, U6, U7 (each deletes its own data) |
| US7.4 | Have an idle session end safely | U2 | u2-session-privacy | U8 (25-minute warning) |
| US7.5 | Have my original files deleted once read | U3 | u3-document-intake | U5 (triggers release), U2 (session end) |
| US7.6 | Keep my bill visible only to me | U8 | u8-family-ui | U11 |
| US7.7 | Have stored data deleted after 3 months | U7 | u7-feedback-insights | — |
| US7.8 | Use the check with large text, a keyboard or a screen reader | U8 | u8-family-ui | U11 |
| US7.9 | Keep my check tied to this browser only | U2 | u2-session-privacy | U8 (every route resolves the session) |
| US8.1 | Maintain rules and reference data without a release | U9 | u9-rules-access | — |
| US8.2 | Approve a rule change before it takes effect | U9 | u9-rules-access | — |
| US8.3 | Run each check as live, shadow or off | U4 | u4-bill-analysis | U9 (check states) |
| US8.4 | Load the launch reference data with its sources | U9 | u9-rules-access | — |
| US8.5 | Switch a check's state from the internal tools | U11 | u11-internal-ui | — |
| US9.1 | Add consented bills to the test set | U10 | u10-test-set-quality | — |
| US9.2 | Label bills independently and reconcile | U10 | u10-test-set-quality | — |
| US9.3 | Withdraw a contributor's bill | U10 | u10-test-set-quality | — |
| US9.4 | Measure accuracy and coverage | U12 | u12-accuracy-runner | U10 (imports results) |
| US9.5 | Keep closed-beta bills with consent | U10 | u10-test-set-quality | U2 (beta consent), U3 (hand-over) |
| US10.1 | Promote a check to live only with a recorded pass | U10 | u10-test-set-quality | U9 (sets live) |
| US10.2 | Sign in to the internal tools with limited access | U9 | u9-rules-access | — |
| US10.3 | See product quality on a dashboard | U10 | u10-test-set-quality | — |
| US10.4 | Explain data practices in support material, with no case lookup | U11 | u11-internal-ui | — |
| US11.1 | Won't Have (this time): abuse controls | Deferred | Phase 1.5 | not in this build |
| US11.2 | Won't Have (this time): share-in from WhatsApp | Deferred | a later build | not in this build |
| US11.3 | Won't Have (this time): urgent-situation guidance and queue priority | Deferred | Phase 1.5 | not in this build |
| US11.4 | Won't Have (this time): bill-correction measure | Deferred | not planned | not in this build |
| US11.5 | Won't Have (this time): regulatory-change tracking | Deferred | Phase 2 | not in this build |
| US11.6 | Won't Have (this time): Expert route and Expert Review | Deferred | Phase 1.5 | not in this build |
| US11.7 | Won't Have (this time): Could Have items | Deferred | a later build | not in this build |
| US11.8 | Won't Have (this time): PRD Won't Have list W1-W22 | Deferred | later phases or never | not in this build |

The titles of the Won't Have stories US11.1 to US11.8 are in the `stories.md` table under Group 11. None of them is built.

## Story Order Within Each Unit

- **U1 contracts (`u1-contracts`):** no story of its own. It carries the types and interfaces every story's units share (see Cross-Cutting Stories).
- **U2 session-privacy (`u2-session-privacy`):** US7.9 → US1.1 → US1.3 → US1.4 → US1.5 → US1.6 → US1.7 → US7.4 → US7.3.
- **U3 document-intake (`u3-document-intake`):** US2.1 → US2.11 → US2.3 → US2.4 → US2.10 → US2.8 → US2.5 → US2.6 → US2.7 → US2.9 → US7.5.
- **U4 bill-analysis (`u4-bill-analysis`):** US3.8 → US3.10 → US3.11 → US3.6 → US3.12 → US3.1 → US3.4 → US3.5 → US3.7 → US8.3 → US4.1 → US4.2 → US4.3 → US4.4 → US4.5 → US4.6 → US4.7 → US4.8.
- **U5 case-processing (`u5-case-processing`):** US3.9 → US6.7.
- **U6 findings-report (`u6-findings-report`):** US4.9 → US4.10 → US4.11 → US4.12 → US4.13 → US4.14 → US3.3 → US5.1 → US5.2 → US6.1 → US5.3 → US6.2 → US5.5 → US5.6 → US5.7 → US5.8 → US5.9 → US5.10.
- **U7 feedback-insights (`u7-feedback-insights`):** US6.4 → US6.5 → US6.6 → US7.2 → US7.7.
- **U8 family-ui (`u8-family-ui`):** US1.2 → US3.2 → US2.2 → US7.6 → US7.8 → US5.4 → US5.11 → US7.1 → US6.3.
- **U9 rules-access (`u9-rules-access`):** US8.4 → US10.2 → US8.1 → US8.2.
- **U10 test-set-quality (`u10-test-set-quality`):** US9.1 → US9.2 → US9.3 → US9.5 → US10.1 → US10.3.
- **U11 internal-ui (`u11-internal-ui`):** US8.5 → US10.4.
- **U12 accuracy-runner (`u12-accuracy-runner`):** US9.4.

## Cross-Cutting Stories

| Story | Why it spans units |
|---|---|
| US7.3 | "Discard everything" is run by session-privacy, but each data owner deletes its own data (ADR-002). |
| US7.5 | document-intake deletes originals; case-processing triggers it after each extraction; session-privacy covers session end. |
| US7.6 | The no-cache, per-request rule applies to every route with bill data, family and internal. |
| US7.8 | Accessibility applies to every screen, family and internal. |
| US5.10 | The wording guard in findings-report applies to every output, including screen text. |
| US9.5 | Beta consent is in session-privacy, the hand-over in document-intake, the beta test bill in test-set-quality. |
| US4.13 | The Complex label is applied in findings-report from labelling settings held in rules-access. |
| US8.3 | Checks run in bill-analysis, reading check states held in rules-access. |
| US10.1 | Promotion records live in test-set-quality; rules-access sets the check live. |
| US9.4 | The run happens in accuracy-runner; test-set-quality imports its results. |
| US7.2 | Storage consent and the close-time hand-over are in session-privacy; the record is built and kept in feedback-insights. |
| US3.9, US6.7 | case-processing runs and re-runs the job; family-ui shows progress and takes the added document; document-intake receives it. |
| All stories | contracts (U1) carries the shared types every unit uses. |

## Coverage Check

- 93 stories in `stories.md`: 85 assigned to exactly one unit, 8 Won't Have (US11.1 to US11.8) assigned to none.
- Stories per unit: U2 9; U3 11; U4 18; U5 2; U6 18; U7 5; U8 9; U9 4; U10 6; U11 2; U12 1. U1 contracts has no story of its own by design.
- Every unit except U1 has at least one story, and no story is assigned twice.

## Assumptions & Open Questions

- bill-analysis (U4) and findings-report (U6) carry 18 stories each, the most of any unit; Delivery Planning may split their work across more than one Bolt.
- The order within each unit is the order the story dependencies allow; Functional Design may refine it.
