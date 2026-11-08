# Units of Work — Hospital Bill Check (Phase 1)

This document groups the 15 building blocks of `inception/domain-design/components.md` into 12 units of work. A unit is designed, built and tested as one. In Construction, each unit goes through Functional Design, NFR Requirements, NFR Design, Infrastructure Design and Code Generation. Its kind decides which design documents apply to it.

This document says what each unit is and owns. The dependencies between units are in `unit-of-work-dependency.md`, and the stories each unit delivers are in `unit-of-work-story-map.md`. It does not choose a build order; Delivery Planning does.

## Sources

- Answers Q1 to Q8 in `units-generation-questions.md` in this folder, confirmed through the consolidated summary, and the approved decomposition plan.
- Component catalogue: `inception/domain-design/components.md` (15 blocks, 46 entities) and the decision log `inception/domain-design/decisions.md` (ADR-001 to ADR-014).
- Requirements: `inception/requirements-analysis/requirements.md` (FR1 to FR16, NFR1 to NFR21).
- User stories: `inception/user-stories/stories.md` (US1.1 to US11.8).
- Team practices: `aidlc/spaces/default/memory/team.md` (walking skeleton first, one builder, test-after with an 80% line-coverage floor, real bills never within the AI assistant's reach).

## Decisions taken in this stage

| ID | Decision | Source |
|---|---|---|
| D-UG1 | 12 units. They follow the "about 10" grouping in Q2, plus a shared contracts unit (Q5) and an offline accuracy runner (Q7) | Q2, Q5, Q7, plan approval |
| D-UG2 | Bill reading, the bill model and the check engine form one bill-analysis unit. These are the three blocks that call no other block, and they are exactly what the offline runner needs. The first reason given for this grouping (that the Q2 grouping would create loops) was wrong once accuracy runs moved to the runner; that was corrected, and the grouping was kept for the runner's sake | Plan approval and follow-up "Keep approved plan" |
| D-UG3 | One Next.js app serves all screens and routes. A background worker built from the same codebase runs processing jobs and the scheduled 3-month deletion. Nothing depends on a particular host | Q3 |
| D-UG4 | The internal tools live in the same app, in their own route area with their own sign-in and role checks | Q4 |
| D-UG5 | Units call each other in-process through module interfaces. There are no internal network APIs. The worker runs the same code, and each unit's data is reached only through that unit | Q6 |
| D-UG6 | Accuracy runs use an offline command-line runner on a machine outside the AI coding assistant's reach. Only bill IDs and scores leave that machine, imported into the test-set unit | Q7 |
| D-UG7 | Units with no dependency path between them may be designed and built in parallel | Q8 |
| D-UG8 | The Domain Design review points R-01 to R-05 get the reviewer's suggested answers as unit notes now. Contract Design formalises them | Q1 |

### Answers to the carried review points (D-UG8)

- **R-01, how pinned rule data reaches the bill model, checks and findings:** case-processing (U5) pins the rule-library version for a run and passes the rule snapshot into bill-analysis (U4) and findings-report (U6). Neither of those units reads rules-access (U9) itself, except LettersGuidance, which reads grievance timelines at the pinned version.
- **R-02, a consented record that fails to store at session close:** session deletion always goes ahead. The record that failed to store is dropped and an alert is raised. Health data never outlives the session because a store failed.
- **R-03, a failed beta hand-over:** the original is deleted anyway, so FR2.3 wins over FR2.11, and an alert is raised.
- **R-04, alerting:** an alerting service is an external dependency of session-privacy (U2), case-processing (U5), feedback-insights (U7) and test-set-quality (U10). The service is chosen with hosting (OQ2).
- **R-05, the hand-kept deletion list:** contracts (U1) defines a required "delete session data" interface. A test fails if any entity keyed by `sessionId` belongs to a unit not on session-privacy's deletion list.

## Unit Summary

| Unit ID | Directory | Unit | Kind | Building blocks | Runs in | Complexity |
|---|---|---|---|---|---|---|
| U1 | u1-contracts | contracts | spec | Cross-boundary types and interfaces (no block of its own) | Everywhere, at build time | S |
| U2 | u2-session-privacy | session-privacy | service | SessionConsent | App | M |
| U3 | u3-document-intake | document-intake | service | DocumentIntake | App (upload); worker (release of originals) | L |
| U4 | u4-bill-analysis | bill-analysis | library | BillReading, BillModel, CheckEngine | Worker, app (reads), accuracy runner | XL |
| U5 | u5-case-processing | case-processing | service | CaseProcessing, plus the background worker entry point | Worker (jobs); app (start, progress, changes) | M |
| U6 | u6-findings-report | findings-report | library | FindingsReport, LettersGuidance | Worker (build); app (show, letters) | L |
| U7 | u7-feedback-insights | feedback-insights | service | Feedback, ConsentedInsights | App; worker (3-month deletion) | M |
| U8 | u8-family-ui | family-ui | ui | FamilyJourneyUI | App (family routes) | XL |
| U9 | u9-rules-access | rules-access | service | RuleLibrary, InternalAccess | App (internal area); snapshot read by worker and runner | L |
| U10 | u10-test-set-quality | test-set-quality | service | TestSetQuality (all except running accuracy) | App (internal area) | L |
| U11 | u11-internal-ui | internal-ui | ui | InternalToolsUI | App (internal area) | M |
| U12 | u12-accuracy-runner | accuracy-runner | packaging | TestSetQuality's accuracy run | Offline machine outside the AI assistant's reach | M |

Complexity is relative: S is a few days; M about a week; L one to two weeks; XL more than two weeks for one builder.

Every building block of `components.md` is in exactly one unit. TestSetQuality is the only block split across two units: running accuracy (U12) and everything else (U10).

## Unit Definitions

### U1 contracts (`u1-contracts`) — kind: spec

- **Description:** the shared types and interfaces that cross unit boundaries. Every other unit depends on it, and nothing else is shared between units (Q5).
- **Boundaries:** types and interface signatures only. No behaviour, no data, no runtime state.
- **Responsibilities:**
  - cross-unit shapes: session ID, case and bill lines as confirmed, check outcomes, findings, the rule snapshot, test-set results (IDs and scores);
  - the required "delete session data" interface every session-data owner implements (R-05);
  - the bill-reader adapter interface (ADR-005).
- **Deployment model:** embedded in every unit at build time.
- **Implementation notes:** Contract Design (next stage) fills this unit's content. Changing a shape here is a change to every consumer, so it goes through Contract Design's change rules.

### U2 session-privacy (`u2-session-privacy`) — kind: service

- **Description:** the anonymous session, consent and declarations, the idle timeout, and confirmed deletion of everything a session holds.
- **Boundaries:** owns Session, ConsentRecord and PatientDeclaration. Holds no bill content.
- **Responsibilities:**
  - the HttpOnly, Secure, same-site session cookie;
  - the consent and declaration gates (FR1.2 to FR1.7, FR1.11);
  - the 25-minute warning and 30-minute end;
  - "Discard everything";
  - deletion run through every session-data owner, with confirmations (ADR-002);
  - the hand-over to consented insights at session close when storage consent is yes;
  - the separate closed-beta consent.
- **Deployment model:** shared, inside the Next.js app.
- **Implementation notes:**
  - It calls the owners in U3, U4, U5, U6 and U7 to delete.
  - A failed consented-record store does not stop deletion (R-02).
  - External dependencies: the session data store and the alerting service.
  - The foundation story US7.9 is here.

### U3 document-intake (`u3-document-intake`) — kind: service

- **Description:** receiving files, upload limits, resumable upload, document types, page prompts, the document checklist, and deleting originals once read.
- **Boundaries:** owns UploadedFile, Page and DocumentChecklist. It is the only unit that ever holds an original file.
- **Responsibilities:**
  - FR4.1 to FR4.11;
  - the "When are you checking?" answer;
  - deleting each original after extraction, including when extraction fails, and at session end (FR2.3);
  - during the closed beta only, handing beta originals to test-set-quality (ADR-014). A failed hand-over still deletes the original (R-03).
- **Deployment model:** shared, inside the Next.js app for uploads. The worker calls it, through case-processing, to release originals.
- **Implementation notes:**
  - The password for a protected PDF is used once and never stored or logged.
  - The beta hand-over is switched off at launch, with a test that no original is kept after launch.
  - External dependencies: the temporary file store.

### U4 bill-analysis (`u4-bill-analysis`) — kind: library

- **Description:** reading bill pages behind the replaceable adapter, building the confirmed bill model, and running the checks.
- **Boundaries:** owns ExtractionResult, Case, BillLine, BillTotals, ClinicalSummary, PackageTerms, LineCorrection, ConsistencyIssue, CheckRun and CheckOutcome. Calls no other unit. It receives the pinned rule snapshot from its caller (R-01).
- **Responsibilities:**
  - FR5 (reading with confidence, unread lines, Indian bill features);
  - FR6 (catalogue mapping, the 21 categories, IRDAI Lists II to IV);
  - FR7.1 and FR7.2 (consistency, arithmetic, misread versus billing error);
  - FR8.1 to FR8.11 (live and shadow checks, document checks, "could not run" reasons);
  - package terms only from the hospital's own words (ADR-011).
- **Deployment model:** embedded. The worker runs it for real cases, the app reads its stored bill model for display, and the offline runner runs it for accuracy.
- **Implementation notes:**
  - Checks are deterministic code; their states and numbers come from the rule snapshot (ADR-006).
  - The bill-reading service is reached only through the adapter, and only if it has passed the data-processor check. It is chosen by the reader trial (OQ8).
  - The code test suites swap in the recorded-output adapter.
  - This is the largest unit; Functional Design may split its design into reading, bill model and checks sections.

### U5 case-processing (`u5-case-processing`) — kind: service

- **Description:** the one coordinator, as a background job with progress, and the background worker entry point.
- **Boundaries:** owns ProcessingJob. Holds no bill content of its own.
- **Responsibilities:**
  - the fixed step order: read, build bill model, check, build findings, build report;
  - arrival-order queueing;
  - pinning rule-library versions per run;
  - re-running from the changed step;
  - telling document-intake to release originals;
  - partial failure and retry, and cancellation;
  - queue length and age reporting (NFR18).
- **Deployment model:** standalone worker process built from the same codebase (Q3). The app enqueues jobs and reads progress through this unit's interface.
- **Implementation notes:**
  - The worker entry point also runs feedback-insights' scheduled 3-month deletion.
  - The job runner and scheduler are chosen with hosting (OQ2).
  - External dependencies: the job runner and the alerting service.

### U6 findings-report (`u6-findings-report`) — kind: library

- **Description:** turning live check outcomes into findings and report content, plus letters, guidance, the glossary and the wording guard.
- **Boundaries:** owns Finding, FindingMark, ReportContent, LetterTemplate, GuidanceText, GlossaryTerm and WordingRule.
- **Responsibilities:**
  - FR8.12 to FR8.20 (grouping, reconciled amounts, classes, routes, the Complex label, folding);
  - FR9 (explanations, Top 3, letters, escalation ladder, tips);
  - FR10 content (Audit Report and Billing Desk Summary);
  - FR11.1 and FR11.2 (marks, dispute pack);
  - FR13.2 and FR13.3 (the wording guard).
- **Deployment model:** embedded. The worker builds findings and report content, and the app shows them and generates letters on request.
- **Implementation notes:**
  - It reads grievance timelines from rules-access at the pinned version.
  - The clinically sensitive list comes from the rule snapshot passed by case-processing (ADR-010).
  - Generated letters are returned, not stored (ADR-007).

### U7 feedback-insights (`u7-feedback-insights`) — kind: service

- **Description:** ratings, the plan answer and the anonymous Insurance Check count, plus the separate consented store with its 3-month deletion.
- **Boundaries:** owns FindingRating, PlanAnswer, InsuranceInterestTotal, InterestClickFlag, StoredCaseRecord and DeletionRun.
- **Responsibilities:**
  - FR12 (ratings with fixed reasons, the plan answer, the Insurance Check total);
  - FR2.4 to FR2.6 and FR2.9 (assembling, de-identifying and storing the consented record, unlinked from the session; deleting it after 3 months);
  - FR12.3 (grouping consented records for quality work).
- **Deployment model:** shared, inside the app for ratings and the close-time store. The worker runs the scheduled deletion.
- **Implementation notes:**
  - It reads bill-analysis and findings-report data at session close.
  - A failed store drops the record with an alert and never delays deletion (R-02).
  - The re-identification risk on stored records is accepted for counsel review before launch (ADR-008).
  - External dependencies: the consented data store, the scheduler and the alerting service.

### U8 family-ui (`u8-family-ui`) — kind: ui

- **Description:** every family-facing screen, from start to download, and the server routes behind them.
- **Boundaries:** holds no bill logic and no stored data.
- **Responsibilities:**
  - screens S1 to S11 and their states;
  - server-side gate enforcement on every route (ADR-004);
  - per-request rendering with `Cache-Control: no-store` for every route with bill data, never `use cache`, prerendering or prefetching with bill data (FR2.10);
  - the on-device PDF and the phone share path (ADR-007);
  - the mockup fixes R-15 and R-17 (ADR-013);
  - WCAG 2.2 AA (NFR12);
  - at most 300 KB of compressed script per route, with the PDF tool within 200 KB (NFR11).
- **Deployment model:** shared, inside the Next.js app.
- **Implementation notes:**
  - It uses Tailwind and shadcn/ui components in `components/ui/`, with the native-element fallbacks in `design-system-mapping.md` (project rule).
  - Read the Next.js 16 guides in `node_modules/next/dist/docs/` before writing routes.

### U9 rules-access (`u9-rules-access`) — kind: service

- **Description:** the rule library (check states and thresholds, reference data, the standard catalogue, labelling settings) and internal sign-in, roles and the access log.
- **Boundaries:** owns CheckDefinition, ReferenceItem, CatalogueEntry, LabellingSettings, RuleChange, LibraryVersion, StaffAccount and AccessLogEntry.
- **Responsibilities:**
  - FR14 (data-only changes without a release; source, dates, version, owner and review date on every item; approval, with self-approval logged);
  - publishing library versions and rule snapshots for case-processing and the offline runner;
  - a check is set live only with a promotion record from test-set-quality (ADR-006);
  - FR15.2 (strong sign-in, the ops, domain admin and support roles, the access log).
- **Deployment model:** shared, inside the app's internal area. Its snapshot reader is embedded in the worker and in the offline runner.
- **Implementation notes:**
  - Launch reference data (US8.4) is loaded here with its sources.
  - The sign-in method is set in NFR Requirements (D-US4).
  - External dependencies: the rule library store and the identity provider.

### U10 test-set-quality (`u10-test-set-quality`) — kind: service

- **Description:** the Labelled Test Set records, blind labelling and reconciliation, withdrawal, importing accuracy results, promotion to live, and the quality dashboard.
- **Boundaries:** owns TestBill, ContributorConsent, Label, Reconciliation, AnswerKey, AccuracyRun (imported scores), PromotionRecord and WithdrawalRecord. It does not run accuracy and does not call bill-analysis (D-UG6).
- **Responsibilities:**
  - FR16 except running the measurement itself: sources and consents, redaction on receipt, two blind labels per real bill with no AI pre-fill, withdrawal with caches cleared and 30-day backup expiry (ADR-009), the beta hand-over intake;
  - FR15.1 and FR16.12 (promotion only with at least 10 bills with the error and 30 without, and the precision target);
  - FR15.4 (the dashboard, with ratings from consented records only).
- **Deployment model:** shared, inside the app's internal area.
- **Implementation notes:**
  - Redacted real bills are held in the test-set store, not in git or any folder the AI coding assistant can open (FR16.13).
  - Every access is logged through rules-access.
  - Imported results hold bill IDs and scores only.

### U11 internal-ui (`u11-internal-ui`) — kind: ui

- **Description:** the internal screens: the rule library, test-set labelling and withdrawal, promotion, the dashboard and support material.
- **Boundaries:** holds no rule or test-set logic and no stored data.
- **Responsibilities:**
  - role check before every route;
  - no screen that finds or opens a family's case (FR15.3);
  - the labelling screen never shows AI output or the other person's labels (FR16.7);
  - shadow and off switching; promotion only through the promotion screen.
- **Deployment model:** shared, inside the app's own internal route area (Q4).
- **Implementation notes:** internal routes are rendered per request and never cached, like the family routes.

### U12 accuracy-runner (`u12-accuracy-runner`) — kind: packaging

- **Description:** the offline command-line runner that measures reading accuracy and finding precision on the Labelled Test Set.
- **Boundaries:** no stored data of its own beyond its output file of bill IDs and scores.
- **Responsibilities:**
  - reading test bills through bill-analysis's adapter, on approved services only;
  - running the checks on labelled bill data with a rule snapshot from rules-access;
  - writing per-bill scores, line accuracy for PDF and photo bills, precision by check, missed-issue rate and coverage, for import into test-set-quality (NFR4, NFR16, FR16.10).
- **Deployment model:** standalone command-line tool, run by the builder or founder on a machine kept outside the AI coding assistant's reach (D-UG6).
- **Implementation notes:**
  - Its cost is the bill-reading service's per-page charge for each run.
  - It runs before a check goes live and on every change to prompts, models or rules, not on every merge (`team.md`).
  - Its tests use synthetic bills only.

## Assumptions & Open Questions

- Hosting and data residency (OQ1, OQ2) decide the worker host, the job runner, the scheduler, every store and the alerting service. No unit depends on a particular host.
- The worker and the app share one codebase, and every unit's data stays behind that unit's interface. NFR Design confirms how the worker reaches each store securely.
- Functional Design for bill-analysis (U4) is the largest piece of design work; it may need to be reviewed in sections.
- The offline machine for U12 must hold the redacted real bills outside any folder the AI coding assistant can open. The founder or builder chooses and sets it up before the first accuracy run.
