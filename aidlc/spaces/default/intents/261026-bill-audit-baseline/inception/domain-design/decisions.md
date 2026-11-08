# Domain Design — Architecture Decision Records

This is the decision log for the component catalogue in `components.md`. Each record gives the context, the decision, its consequences (including security and compliance), and the alternatives rejected. The records are numbered in the order they build on each other, not by importance. Every record is Accepted: each one follows an answer you confirmed in this stage or a decision already settled in an earlier stage.

## Sources

- Answers Q1 to Q9 and the follow-up Q7a in `domain-design-questions.md` in this folder.
- Requirements: `inception/requirements-analysis/requirements.md`, including the decisions recorded there (D-RA1 to D-RA3) and its constraints (CR-T8, CR-T9, CR-T11).
- User stories: `inception/user-stories/stories.md`, decisions D-US1 to D-US23 and review findings R-04 and R-07.
- Refined mockups: `inception/refined-mockups/` review findings R-15, R-16 and R-17.
- Existing code: `aidlc/spaces/default/codekb/hospital-bill-audit-app/architecture.md` and `component-inventory.md` (an unmodified Next.js 16.4.3 scaffold with Cache Components on).
- Team practices: `inception/practices-discovery/team-practices.md`.

## Index

| ADR | Title | Status | Source |
|---|---|---|---|
| ADR-001 | Split by capability: 13 logic blocks and 2 screen blocks | Accepted | Q4, Q8, Q9 |
| ADR-002 | Each block owns its session data; SessionConsent runs and confirms deletion | Accepted | Q5 |
| ADR-003 | One coordinator, fixed step order, versions pinned per run | Accepted | Q8; US3.9; D-US1, D-US23 |
| ADR-004 | Screens hold no bill logic; gates are enforced at the screen routes | Accepted | Q9; US1.4 to US1.7, US7.6, US7.9 |
| ADR-005 | The bill reader sits behind a replaceable adapter | Accepted | Requirements Q11 (D-RA3), CR-T8 |
| ADR-006 | Checks are code; their states and numbers are data; only the test set can make a check live | Accepted | User Stories Q23 (D-US17); FR15.1 |
| ADR-007 | The report PDF is built on the device; letter edits stay on the device | Accepted | User Stories Q15 (D-US9) |
| ADR-008 | Consented data in a separate, unlinked store; re-identification risk accepted for counsel review | Accepted | Q1; US7.2, US7.7 |
| ADR-009 | Withdrawn test-set bills: IDs and scores only, caches cleared, 30-day backups | Accepted | Q2 |
| ADR-010 | The clinically sensitive list for the Complex label, held as data | Accepted | Q6; requirements OQ3 |
| ADR-011 | Package inclusions come only from the hospital's own words | Accepted | Q7, Q7a |
| ADR-012 | Internal tools behind their own sign-in; data owners log access | Accepted | US10.2; CR-T11 |
| ADR-013 | Mockup fixes R-15, R-16 and R-17 | Accepted | Q3 |
| ADR-014 | Closed-beta originals are handed from intake to the test set | Accepted | D-US8; FR2.11 |

---

## ADR-001: Split by capability: 13 logic blocks and 2 screen blocks

**Status:** Accepted

**Context.** The product handles data with very different lifetimes: originals that must be gone once read, session data that must be gone at session end, consented records kept for 3 months, and redacted test-set bills kept for the life of the set (`requirements.md` FR2, FR16). One builder writes it. Units Generation will group blocks into what is deployed together, so a finer split here costs no extra deployments. The existing code has no module boundaries to keep (`architecture.md`, `component-inventory.md`).

**Decision.** We split by capability into 13 logic blocks: SessionConsent, DocumentIntake, BillReading, BillModel, CaseProcessing, CheckEngine, FindingsReport, LettersGuidance, Feedback, ConsentedInsights, RuleLibrary, TestSetQuality and InternalAccess. We add 2 screen blocks: FamilyJourneyUI and InternalToolsUI. This follows Q4 option A. CaseProcessing is the 13th logic block: the list in Q4 did not name a coordinator, and Q8 chose one, so it needs a home.

**Consequences.**
- Positive: each retention rule has one owner, so the deletion guarantees are block boundaries rather than conventions inside a large block. Five blocks (CheckEngine, BillModel, BillReading, Feedback, InternalAccess) call no other block and can be tested on plain inputs.
- Negative: 15 blocks mean more interfaces to define in Contract Design, and more places for Units Generation to group.
- Security and compliance: health data never sits in a block that does not need it. The test-set store and the consented store are separate blocks from session data, as FR16.8 and US7.2 require.

**Alternatives Rejected.**
- About 6 coarser blocks (Q4 B): fewer interfaces, but it mixes data with different retention inside one block.
- One block per story group (Q4 C): it follows the journey, not data ownership, and splits the bill model across groups.

---

## ADR-002: Each block owns its session data; SessionConsent runs and confirms deletion

**Status:** Accepted

**Context.** "Discard everything", the 30-minute timeout and normal session end must delete every piece of a session's data at once, with no copy left anywhere (FR1.8, FR2.3, FR2.4, FR2.8). The session's data is spread across uploaded-file records, extraction results, the bill model, check outcomes, findings, marks and ratings.

**Decision.** Each block owns its own session data, keyed by `sessionId`. SessionConsent runs deletion. It cancels processing first, hands the session to ConsentedInsights only when storage consent is yes and the user did not discard, and then asks each owner in turn to delete that session's data: DocumentIntake, BillReading, BillModel, CaseProcessing, CheckEngine, FindingsReport and Feedback. It records each confirmation. A missing confirmation is retried and raises an alert, and the session stays marked as ending until every owner has confirmed (Q5 A).

**Consequences.**
- Positive: no shared mutable store crosses block boundaries. Each owner can test its own deletion. The confirmation list makes "everything is gone" checkable.
- Negative: SessionConsent depends on every owner of session data. Adding a new session-data owner means adding it to the deletion list. Contract Design should make "delete session data" a required interface on every session-data owner, so nobody forgets.
- Security and compliance: supports the DPDP requirement to delete on withdrawal of consent (CR-R1, FR1.8). Deletion failures become alerts rather than silent leftovers.

**Alternatives Rejected.**
- One session record holding everything (Q5 B): deletion would be a single delete, but every block would read and write one shared record. That is the shared-state coupling the boundaries are meant to prevent, and it would make the unlinked consented record harder to guarantee.
- Deletion by expiry only (each store sets its own time to live): it is simple, but it cannot confirm that "Discard everything" deleted everything straight away.

---

## ADR-003: One coordinator, fixed step order, versions pinned per run

**Status:** Accepted

**Context.** Processing can take up to 15 minutes, so it runs as a background job with progress, in arrival order (US3.9, D-US1). Fixing a line, confirming details or adding a document must redo the findings and the report in the same session (FR4.11, FR5.8, FR10.5). The same confirmed lines and rule versions must give the same findings (NFR6, D-US23).

**Decision.** CaseProcessing is the only coordinator. It runs read, build bill model, check, build findings and build report in that fixed order. At the start of each run it pins one rule-library version and its reference-data versions, and passes them to every step. Every change goes through it. Confirmed details and line fixes re-run from the check step. An added document re-runs from reading that document onward. After each extraction it tells DocumentIntake to delete the original, whether the extraction succeeded or failed (Q8 A).

**Consequences.**
- Positive: one place decides order and re-runs, so reproducibility is testable. A failed step keeps the steps before it, and a retry starts from the failed step (FR7.3). Cancelling one job stops all later writes (AC3.9.2).
- Negative: CaseProcessing is a central block that every change passes through, so its interface must stay small. A full re-run from the check step costs more than patching a single finding, but it stays well inside NFR2 because checks are deterministic code.
- Security and compliance: the coordinator is where "the original is deleted once read" is triggered, so FR2.3 has one tested trigger point.

**Alternatives Rejected.**
- Blocks reacting to change events with no coordinator (Q8 B): there is no central block, but the result could depend on the order of events, which works against NFR6, and cancelling a run would need every listener to handle it.
- A coordinator plus in-place patches for small fixes (Q8 C): fixes are faster, but a patched result could differ from a full re-run on the same inputs.

---

## ADR-004: Screens hold no bill logic; gates are enforced at the screen routes

**Status:** Accepted

**Context.** The consent and declaration gates (US1.4 to US1.7) and the session checks (US7.9) must be enforced on the server and tested with direct requests that skip the screens (`stories.md`, Common test conditions). Pages with bill data must be rendered per request and never cached, prerendered or prefetched with bill data, because Cache Components and partial prefetching are on in this Next.js version (FR2.10, US7.6, `architecture.md`). Each user route may load at most 300 KB of compressed script (NFR11).

**Decision.** There are two screen blocks. FamilyJourneyUI holds the family screens and the server routes behind them. InternalToolsUI holds the internal tools. Neither holds bill or rule logic (Q9 A). Every FamilyJourneyUI route first asks SessionConsent to resolve the session and its gates, and refuses the request when a gate is not passed. It then passes the validated session ID to the logic blocks. FamilyJourneyUI also owns the rule that bill-data routes are rendered per request with `Cache-Control: no-store` and never use `use cache`.

**Consequences.**
- Positive: one block holds the gate check and the no-cache rule, so a code-scan rule and one set of direct-request tests cover them. Logic blocks have no calls back to SessionConsent, which keeps the graph free of cycles.
- Negative: logic blocks trust the session ID they are given. They must never be reachable except through the screen routes or the coordinator. Contract Design must keep their interfaces internal, with no public route of their own.
- Security and compliance: the gates are enforced on the server, never only in the browser. Keeping family screens apart from internal screens means redacted real bills are never one route away from a family session.

**Alternatives Rejected.**
- Each logic block owns its own screens (Q9 B): the slices are vertical, but the gate check and the no-cache rule would be repeated in many blocks.
- One screens block for family and internal tools together (Q9 C): there is one place for all UI, but internal screens that reach redacted real bills would share a block and an access model with anonymous family screens.

---

## ADR-005: The bill reader sits behind a replaceable adapter

**Status:** Accepted

**Context.** The requirements record the decision to keep the bill-reading service replaceable without rewriting checks (D-RA3), and CR-T8 recommends avoiding a single vendor. The reader is chosen by a trial (OQ8). Bill reading by AI need not return the same result twice (D-US23). In the code test suites, the reader is replaced by recorded outputs of synthetic bills (`team-practices.md`).

**Decision.** BillReading is the only block that talks to a bill-reading service, through one adapter interface. Every result records the reader name and version. A service is called only if it has passed the data-processor check and is contracted not to train on user data (NFR19).

**Consequences.**
- Positive: the trial winner, or a later replacement, plugs in without changing CheckEngine or BillModel. The test suites swap in the recorded-output adapter.
- Negative: the adapter must use a reader-neutral result shape, which may hide some features specific to one reader.
- Security and compliance: one block holds the only outbound path for bill content, so the data-processor check and the no-training term have one place to enforce and audit.

**Alternatives Rejected.**
- Calling a vendor SDK directly from the reading and checking code: it is quicker to start, but it ties checks to one vendor, against CR-T8.
- Running two readers on every bill and reconciling: it gives better accuracy in principle, but doubles cost and data exposure. The adapter allows this later if the trial shows a need.

---

## ADR-006: Checks are code; their states and numbers are data; only the test set can make a check live

**Status:** Accepted

**Context.** "Without a release" covers data and settings only. A change to how a check works goes through a release (D-US17). Each check must be switchable between live, shadow and off (FR14.4). A check may go live only from the ops view and only with a recorded pass (FR15.1, FR16.12).

**Decision.** Check logic lives in CheckEngine as code. CheckEngine calls no other block and makes no AI call. RuleLibrary holds data only: check states and thresholds, reference data, the standard catalogue and labelling settings, each change approved and published as a new library version. RuleLibrary sets a check live only when it is given a PromotionRecord reference, and only TestSetQuality supplies one. Shadow and off can be set from the internal tools.

**Consequences.**
- Positive: checks are deterministic and testable on structured fixtures. Promotion has one path, and RuleLibrary does not need to call TestSetQuality, which avoids a cycle.
- Negative: a new kind of check always needs a release, even a simple one.
- Security and compliance: every data change records who, what, why and source and is approved before it takes effect (NFR15). Self-approval is allowed and logged (D-US18).

**Alternatives Rejected.**
- Rules written as data in a small rule language, so new checks need no release: more flexible, but harder to test and to measure, and it goes against D-US17.
- RuleLibrary verifies the pass itself by calling TestSetQuality: it is one less handover, but it creates a cycle with TestSetQuality's need to read rule versions.

---

## ADR-007: The report PDF is built on the device; letter edits stay on the device

**Status:** Accepted

**Context.** The report PDF is built on the device on both phone and desktop, and on phones "Save or share" opens the phone's own share menu with nothing sent through our servers (FR10.3, D-US9). The PDF tool loads only after a tap, within 200 KB. Letters can be edited before download (FR9.4).

**Decision.** FindingsReport and LettersGuidance return report content and generated letter text. FamilyJourneyUI builds the PDF on the device. Letter edits stay on the device and go only into the downloaded file; no typed text is sent back to our servers.

**Consequences.**
- Positive: no server ever holds a report file or the user's typed letter text, which also keeps text out of server logs.
- Negative: letter edits are lost if the page reloads before download. The on-device PDF tool has to meet its script limit and time limit on the target phone.
- Security and compliance: it supports FR2.3 and FR10.3. Anything the user types into a letter, such as a patient name, never reaches a store.

**Alternatives Rejected.**
- Building the PDF on the server: rendering would be more consistent, but the file would pass through our servers, which goes against FR10.3.
- Saving letter edits on the server: edits would survive a reload, but user-typed personal text would land in a session store.

---

## ADR-008: Consented data in a separate, unlinked store; re-identification risk accepted for counsel review

**Status:** Accepted

**Context.** With storage consent, a record keeps line items dated by day of stay, hospital, city, findings, ratings, age band, gender, admission month, length of stay, procedure, diagnosis and the plan answer, and is deleted after 3 months (US7.2, US7.7, D-US2, D-US21). The User Stories review (R-04) noted that a rare procedure at a small hospital could point to one patient even with names removed.

**Decision.** ConsentedInsights keeps these records in a store apart from session data. Each record gets a new random ID and has no link back to its session or case. It is assembled at session close when storage consent is yes; "Discard everything" withdraws consent and nothing is kept. A scheduled run deletes every record older than 3 months and raises an alert when a run fails. The fields are stored as planned. The re-identification risk for rare hospital-and-procedure combinations is accepted, and counsel reviews it before launch (Q1 C).

**Consequences.**
- Positive: the quality data keeps its full value for internal work, and the record cannot be joined back to a session.
- Negative: the privacy risk R-04 stays open until counsel's review. If counsel asks for generalisation, the record's assembly changes, but no other block does.
- Security and compliance: this is an accepted risk under the DPDP Act (CR-R1, CR-R2), listed with AS6 and OQ6 for the pre-launch counsel review. The store is encrypted at rest, and aggregates include consented records only (FR12.3).

**Alternatives Rejected.**
- Generalise rare combinations before storing, keeping hospital and exact procedure only when at least 5 consented records share them (Q1 A): lower risk, but more complex and less useful for early quality work.
- Never store the hospital name (Q1 B): lowest risk, but it loses grouping by hospital, which FR12.3 asks for.

---

## ADR-009: Withdrawn test-set bills: IDs and scores only, caches cleared, 30-day backups

**Status:** Accepted

**Context.** Contributors can withdraw their bills at any time, and a withdrawn bill must be deleted from the set and from future runs (FR16.9). The User Stories review (R-07) noted that accuracy-run outputs, caches and backups could still hold copies.

**Decision.** Accuracy-run outputs keep only bill IDs and scores, never bill content. On withdrawal, the redacted original, labels and reconciliation are deleted, caches are cleared, backups expire within 30 days, and a withdrawn bill is never restored from a backup (Q2 A). TestSetQuality records each step of the withdrawal.

**Consequences.**
- Positive: past accuracy runs stay valid as score history and hold nothing to delete. The 30-day ceiling gives a clear promise to contributors.
- Negative: for up to 30 days a backup can still hold the bill. Restore procedures must check the withdrawal records before restoring anything.
- Security and compliance: it meets the contributors' right to withdraw under the DPDP Act, with a bounded and recorded tail.

**Alternatives Rejected.**
- Editing backups to remove the bill straight away (Q2 B): no tail at all, but backups are usually not editable without risk to their integrity.
- Leaving it to NFR Requirements (Q2 C): it would leave R-07 open through another stage.

---

## ADR-010: The clinically sensitive list for the Complex label, held as data

**Status:** Accepted

**Context.** Findings in clinically sensitive categories must be marked "Complex — consider professional advice", because only a clinician can judge them. A value threshold may be set later without a release (FR8.16, US4.13). The category list was left to this stage (requirements OQ3).

**Decision.** The clinically sensitive list is: ICU/HDU; OT & Procedure; Anaesthesia; Implants & Devices (the choice of implant, not its price); Blood & Blood Products; and any finding that questions whether a test, drug or procedure was medically needed (Q6 A). The list and the later value threshold, unset at launch, are data in RuleLibrary's labelling settings. FindingsReport reads them from the version pinned for the run.

**Consequences.**
- Positive: price findings on implants and drugs stay with the billing desk, where they can be settled, and clinical judgments are routed away from it. The list can change without a release.
- Negative: Pharmacy and Radiology & Imaging choices are not marked Complex, so a dispute over the choice of a drug or scan could still reach the billing desk. The wording guard and the medical-need rule limit this.
- Security and compliance: it supports CR-R4 (no AI clinical opinion). A Complex finding gives no clinical opinion (AC4.13.5).

**Alternatives Rejected.**
- Only findings that question medical need, in any category (Q6 B): a narrower list, but ICU and implant choices would go to the billing desk.
- List A plus Pharmacy and Radiology & Imaging (Q6 C): wider coverage, but many routine drug and scan findings would be marked Complex, which weakens the label.

---

## ADR-011: Package inclusions come only from the hospital's own words

**Status:** Accepted

**Context.** The package check (US4.7) looks for items billed separately that the package normally includes. No source had been named. Requirement FR6.6 reads package inclusions and exclusions "where the bill shows them or the user provides them".

**Decision.** Inclusions come only from the hospital's own words, wherever they appear: an uploaded package letter, an estimate or the bill itself (Q7 A, Q7a A). BillModel records the source document on PackageTerms. When none is present, CheckEngine records the inclusion check as "could not run" and the report names the package letter.

**Consequences.**
- Positive: every package finding rests on the hospital's own stated terms, which is the strongest evidence a family can bring to the billing desk.
- Negative: the check runs less often, only when the family has the letter or the bill prints it. Public scheme definitions such as PM-JAY are not used, even as context.
- Security and compliance: it avoids presenting outside package definitions as the hospital's obligations, which supports the no-accusation rule (CR-R6).

**Alternatives Rejected.**
- The hospital's letter plus public scheme definitions as context (Q7 B): more context, but it risks families treating a public scheme's inclusions as binding on a private hospital.
- A reference list built from the Labelled Test Set (Q7 C): it could cover more cases later, but the check would stay in shadow mode for a long time.

---

## ADR-012: Internal tools behind their own sign-in; data owners log access

**Status:** Accepted

**Context.** Internal tools must have their own strong sign-in, role-based least-privilege access (ops, domain admin, support) and a log of every access to personal data (FR15.2, CR-T11). The sign-in method is set in NFR Requirements (D-US4). Families have no accounts (FR2.2).

**Decision.** InternalAccess owns staff accounts, roles, sign-in and the access log. InternalToolsUI checks the role before every route. TestSetQuality records each access to test-set data, and RuleLibrary records each change and approval, through InternalAccess.

**Consequences.**
- Positive: the access log is written by the block that knows what was accessed, so it is specific, not just page views.
- Negative: TestSetQuality and RuleLibrary each depend on InternalAccess, so the log call is part of their interfaces.
- Security and compliance: it meets FR15.2 and NFR7. No family data path depends on InternalAccess, so a fault there cannot expose family sessions.

**Alternatives Rejected.**
- Enforcing access and logging only in the screen layer: it is simpler, but the log would miss accesses that do not come through a screen, such as an accuracy run.
- Reusing the family session mechanism for staff: there would be one session system, but it would mix anonymous sessions with identified staff accounts.

---

## ADR-013: Mockup fixes R-15, R-16 and R-17

**Status:** Accepted

**Context.** Three small gaps were accepted at the Refined Mockups approval without being fixed. By project practice, they are raised here first.

**Decision.** All three are decided here (Q3 A):
- R-15: when the line editor is opened from the confirm screen, Back and Save return to the confirm screen and nothing regenerates (FamilyJourneyUI).
- R-16: the heading "What to do next time" stays for every family; for a family whose patient has died, only the tips are rephrased (LettersGuidance, as a text variant).
- R-17: the report screen shows the same "This is taking too long" state as the progress screen whenever it waits on a job (FamilyJourneyUI).

**Consequences.**
- Positive: the three gaps are closed before Functional Design, and each has one owning block.
- Negative: none significant. R-16 adds one text variant to keep under the wording guard.
- Security and compliance: none.

**Alternatives Rejected.**
- Leaving them for Functional Design (Q3 B): it defers small, settled choices and risks losing them, which the project practice on carried findings warns against.

---

## ADR-014: Closed-beta originals are handed from intake to the test set

**Status:** Accepted

**Context.** During the closed pre-launch beta only, testers agree separately that their bills are kept for testing. Beta bills go to a store separate from product data and are blacked out before they enter the test set. The beta ends at launch (FR2.11, D-US8, US9.5).

**Decision.** SessionConsent records the separate beta consent. When an original from a session with beta consent would be deleted, DocumentIntake instead hands it to TestSetQuality, which blacks it out and stores it as a beta test bill. After launch, this hand-over is switched off, and every original is deleted once read.

**Consequences.**
- Positive: the beta path reuses the normal journey, and only the last step for the original differs.
- Negative: DocumentIntake depends on TestSetQuality for the beta period. The switch-off at launch must be part of the launch checklist, and a test must show no original is kept after launch.
- Security and compliance: beta originals are personal data held with separate consent, in the test-set store, with access logged. FR16.13 still applies: real bills never reach git or the AI coding assistant.

**Alternatives Rejected.**
- A separate beta upload path: it is cleaner after launch, but it would be a second journey to build and test for a short beta.
- Copying the original before deletion: it is simpler, but for a moment a copy would exist outside both stores' rules.
