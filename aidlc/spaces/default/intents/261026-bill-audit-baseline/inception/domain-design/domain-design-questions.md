# Domain Design — Questions

This stage names the logical building blocks of the product: the parts we write code for. It also settles which part owns each kind of data and how the parts talk to each other. It does not decide how they are deployed; that is the next stage, Units Generation.

## Already settled, not asked again

These are recorded as design decisions in `decisions.md`, with no question:

- **The bill-reading service sits behind a replaceable adapter,** so a vendor can be swapped without touching the checks (Requirements Analysis Q11; constraint CR-T8).
- **Each check is code**, with its live, shadow or off state and its numbers held as data in the rule library. A change to how a check works goes through a release (User Stories Q23).
- **Processing runs as a background job with progress** (story US3.9). Cases are processed in arrival order (User Stories Q8).
- **The report PDF is built on the device.** The server provides the report content only (User Stories Q15).
- **Data kept with consent lives apart from session data,** with its own 3-month deletion (stories US7.2 and US7.7).
- **Internal tools sit behind their own sign-in and role checks** (US10.2). The method is set in NFR Requirements.

## Carried forward from earlier stages

Five findings were accepted at earlier approvals without being fixed. Per the project rule, they are raised here first:

- **From User Stories:**
  - R-04: rare hospital-and-procedure combinations in stored data;
  - R-07: copies left behind when a test-set bill is withdrawn.
- **From Refined Mockups:**
  - R-15: where the line editor returns when opened from the confirm screen;
  - R-16: the heading for a family whose patient has died;
  - R-17: a "taking too long" state on the report screen as well.

---

## Q1. Stored data that could point to one patient (User Stories review R-04)

With consent, a stored record keeps hospital, city, procedure, diagnosis, age band, gender, admission month and length of stay. For a rare procedure at a small hospital, that combination could point to one person, even with names and numbers removed.

A. Generalise before storing: keep the hospital name and the exact procedure only when at least 5 consented records share that hospital and procedure; otherwise store the procedure group (for example "cardiac procedure") and the hospital's city and size band instead
B. Never store the hospital name; keep the city and a hospital size band only, and the exact procedure
C. Store as planned and record it as an accepted privacy risk for counsel to review before launch
X. Other (please specify)

[Answer]: C

## Q2. Copies left behind when a test-set bill is withdrawn (User Stories review R-07)

When a contributor withdraws a bill, its redacted original, labels and reconciliation record are deleted. Accuracy-run outputs, caches and backups could still hold copies.

A. Accuracy-run outputs keep only bill IDs and scores, never bill content; caches are cleared on withdrawal; backups expire within 30 days, and a withdrawn bill is never restored from a backup
B. The same, but backups are also edited to remove the bill straight away
C. Leave it to NFR Requirements to decide
X. Other (please specify)

[Answer]: A

## Q3. The three small gaps from Refined Mockups (R-15, R-16, R-17)

Each has one obvious fix:

- R-15: when the line editor is opened from the confirm screen, "Back" and "Save" return to the confirm screen and nothing regenerates.
- R-16: keep the heading "What to do next time" for every family, and rephrase only the tips for a family whose patient has died.
- R-17: show the same "This is taking too long" state on the report screen whenever it is waiting on a job.

A. Record all three fixes as decisions here, so later stages build them
B. Leave them for Functional Design to settle
X. Other (please specify)

[Answer]: A

## Q4. How finely to split the product into building blocks

Units Generation later groups these blocks into what gets built and deployed together, so a finer split here does not mean more deployments.

A. About 12 blocks by capability:
   - session and consent;
   - document intake;
   - bill reading (behind the adapter);
   - the bill model and categories;
   - checks;
   - findings and report;
   - letters and guidance;
   - feedback;
   - consented insights;
   - rule library;
   - test set and quality;
   - internal access
B. About 6 coarser blocks:
   - family journey;
   - bill processing (intake, reading, categories);
   - checking (engine and rules);
   - reporting (findings, report, letters);
   - insights and feedback;
   - internal tools
C. One block per story group (11 blocks, following the journey steps)
X. Other (please specify)

[Answer]: A

## Q5. Who owns a check's data during the session

During a check, several kinds of data exist: uploaded-file records, lines as read, findings, the user's marks and ratings. "Discard everything" and the 30-minute timeout must delete all of it at once.

A. Each block owns its own data, keyed by the session. The session block runs the deletion by asking every owner to delete that session's data, and it confirms each one finished
B. One session record holds everything for the check, and every block reads and writes through it; deletion removes that one record
X. Other (please specify)

[Answer]: A

## Q6. Which categories count as "clinically sensitive" for the "Complex" label

Findings in these categories are marked "Complex — consider professional advice", because only a clinician can judge them (story US4.13; requirements open question OQ3). The PRD's 21 bill categories include ICU/HDU, OT & Procedure, Anaesthesia, Implants & Devices, Blood & Blood Products, Pharmacy and others.

A. ICU/HDU; OT & Procedure; Anaesthesia; Implants & Devices (choice of implant, not its price); Blood & Blood Products; and any finding that questions whether a test, drug or procedure was medically needed
B. Only findings that question whether something was medically needed, in any category
C. List A plus Pharmacy (choice of drug, not its price) and Radiology & Imaging
X. Other (please specify)

[Answer]: A

## Q7. Where "items the package normally includes" comes from

The package check (US4.7) looks for items billed separately that a package normally includes. No source for those inclusions is named yet.

A. Only the hospital's own package letter or estimate that the family uploads; when none is uploaded, the check does not run and asks for the letter
B. The hospital's letter first, plus public scheme package definitions (such as PM-JAY) shown only as context, never as a finding
C. Build a reference list from the Labelled Test Set over time; until then the check stays in shadow mode
X. Other (please specify)

[Answer]: A

## Q8. How a change flows through the check (fixing a line, adding a document)

When the user fixes a line ("Fix a line", FR5.8) or adds a document after the report ("Add a document", FR4.11), the lines, categories, findings and report must all be redone in the same session. Re-running with the same inputs and rule versions must give the same findings (NFR6).

A. One coordinator runs the steps in a fixed order (read, categorise, check, build findings, build report) and re-runs from the step that changed. Each block is called directly and returns its result
B. Each block announces a change ("lines changed") and the blocks after it react on their own, with no central coordinator
C. A coordinator for the main run, but small fixes (one line corrected) update only the affected findings in place, without re-running the later steps
X. Other (please specify)

[Answer]: A

## Q9. How the screens relate to the logic

The family-facing screens (start, consent, upload, confirm, report, letters) and the internal tools (rule library, test set, ops views) both need data from the logic blocks in Q4.

A. Screens are their own blocks: one for the family journey and one for internal tools. They hold no bill logic and reach the logic blocks only through those blocks' public interfaces
B. Each logic block owns its own screens (for example, the upload block owns the upload screen)
C. One screens block for everything, family and internal together, kept apart from the logic blocks
X. Other (please specify)

[Answer]: A

## Q7a. Follow-up: package inclusions printed on the bill itself

You chose the hospital's own package letter or estimate as the only source (Q7 A). Requirement FR6.6 also reads package inclusions and exclusions "where the bill shows them". Some hospitals print the package's inclusion list on the bill itself.

A. Yes: the hospital's own words count wherever they appear, on an uploaded package letter, an estimate or the bill itself. With none of these, the check does not run and asks for the letter
B. No: only a separately uploaded package letter or estimate counts; inclusions printed on the bill are shown to the user but not used by the check
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Q1 (R-04): stored consented records keep hospital, city, procedure, diagnosis, age band, gender, admission month and length of stay as planned; the re-identification risk for rare hospital-and-procedure combinations is recorded as an accepted privacy risk for counsel to review before launch (C).
- Q2 (R-07): accuracy-run outputs keep only bill IDs and scores, never bill content; caches are cleared on withdrawal; backups expire within 30 days, and a withdrawn bill is never restored from a backup (A).
- Q3 (R-15, R-16, R-17): all three mockup fixes are recorded as decisions here (A).
- Q4: about 12 logic blocks by capability: session and consent; document intake; bill reading (behind the adapter); bill model and categories; checks; findings and report; letters and guidance; feedback; consented insights; rule library; test set and quality; internal access (A).
- Q5: each block owns its own session data, keyed by the session; the session block runs deletion by asking every owner to delete and confirms each one finished (A).
- Q6: clinically sensitive categories are ICU/HDU, OT & Procedure, Anaesthesia, Implants & Devices (choice of implant, not its price), Blood & Blood Products, plus any finding that questions whether a test, drug or procedure was medically needed (A).
- Q7 and Q7a: package inclusions come only from the hospital's own words, wherever they appear: an uploaded package letter, an estimate or the bill itself; with none of these, the check does not run and asks for the letter (A, A).
- Q9: two screen blocks, one for the family journey and one for internal tools; neither holds bill logic, and both reach the logic blocks only through their public interfaces (A).
- Q8: one coordinator runs read, categorise, check, build findings and build report in a fixed order, and re-runs from the step that changed (A).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
