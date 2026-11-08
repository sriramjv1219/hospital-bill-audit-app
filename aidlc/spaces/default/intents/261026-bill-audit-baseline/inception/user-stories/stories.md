# User Stories — Hospital Bill Check (Project RS, Phase 1)

These stories turn the requirements in `inception/requirements-analysis/requirements.md` into user-facing behaviour with testable acceptance criteria. Stories carry stable `US{group}.{seq}` IDs and acceptance criteria carry `AC{group}.{seq}.{n}` IDs. Later stages keep both as they are. Each story lists the requirement IDs it covers. The full coverage map is in `traceability.json`.

## Sources

- [Q1]–[Q28]: answers in `user-stories-questions.md` in this folder, confirmed through the consolidated summary (the latest confirmation covers all 28).
- Requirements: `inception/requirements-analysis/requirements.md` (FR1–FR16, NFR1–NFR21), approved with review findings R-01 to R-08 accepted as open risks (`inception/requirements-analysis/reviews/review-01.md`). Questions Q8–Q16 settle those findings here.
- Personas: `personas.md` in this folder (C1 Priya, C2 Meera, C3 Rohit, C4 Anil, I1 builder and founder; S1 Sunita as stakeholder).
- Contributions from the designer, developer and quality engineer: `contributions/` in this folder. Their corrections are folded in below. Their judgment calls were put to you as Q17–Q28.
- Approved rough mockups: `ideation/rough-mockups/wireframes.md` and `user-flow.md` (screens S1–S11).
- PRD check catalogue §13.6 and §13.8 (`PRDs/app-baseline.md`, read through the document library, id 01a17987), for the meaning of each launch and document check.
- Team practices: `inception/practices-discovery/team-practices.md` (synthetic bills only in tests; Jest and Playwright).

## How to read these stories

- **Format.** "As a [persona], I want [goal], so that [benefit]." Acceptance criteria use Given/When/Then and include at least one error or edge case per story.
- **Priority.** MoSCoW is taken from the requirements: Must Have, Should Have, or Won't Have (this time) [Q7]. Delivery Planning draws the MVP line.
- **Grouping.** Groups follow the family's journey, and the internal tools have groups of their own [Q2]: 1 Start and consent; 2 Upload; 3 Review and confirm; 4 Findings; 5 Report and letters; 6 Act and give feedback; 7 Privacy and access throughout the session; 8 Rule library; 9 Labelled Test Set; 10 Ops and quality; 11 Won't Have (this time).
- **Size.** One story per user-visible capability [Q3]. The launch checks are grouped into one story per check family, with one criterion per check [Q4]. Internal-tool stories are lighter. They have full criteria only for promotion to live, rule-change approval, sign-in and bill withdrawal [Q5].
- **Non-functional requirements** appear as criteria on the stories they affect. Availability, scalability, observability and the other system-wide targets are deferred to later stages [Q6] (see `traceability.json`).
- **Fixed text.** Quoted UI labels ("Keep going", "Confirm a few details", "Save or share", "Discard everything") are fixed text. Refined Mockups keeps them in one label list, and the tests reuse it.
- **Launch-checklist and review criteria.** Criteria marked "(launch checklist)" or "(review)" are checked by a recorded sign-off, not by an automated test.

### Common test conditions

- **Synthetic bills only.** Every "synthetic bill" below is made up. Each one comes from a seeded generator, carries its answer key, a visible `SYNTHETIC` marker and a unique marker string (a canary), and is listed in a fixture manifest. An automatic check fails the merge if any fixture file is missing from the manifest. No test uses a real bill (`team-practices.md`).
- **Phone and network.** Tests in CI emulate the 8 GB Android target with a throttled connection (about 9 Mbps down, 1.5 Mbps up, 170 ms latency) and a 4× CPU slowdown [Q10]. One pass on a real 8 GB Android phone is recorded before launch (launch checklist). "Usable" means the screen's main action can be used.
- **Script size.** The 300 KB limit is compressed (gzip) JavaScript loaded on first load of each user route, checked by one automatic check on every merge. Code loaded only after a tap has its own limit: the on-device PDF tool is at most 200 KB compressed [Q21].
- **Checks in a fixed state.** Each check family is tested with its checks set to `live`, or to `shadow` for the shadow criteria, in a test copy of the rule library. Check criteria run on structured synthetic bill data, so no bill reading is involved. Whether a check is live in production is decided only by US10.1.
- **Bill reader replaced in the code suite.** The Jest and Playwright suites swap the bill-reading service for recorded outputs of synthetic bills. The real reader is used only in timing runs and in the offline accuracy run (US9.4).
- **Gates enforced on the server.** The consent and declaration gates (US1.4 to US1.7) and the session checks (US7.9) are enforced on the server. Each one is tested with a direct request that skips the screens, and that request must be refused.

## Decisions taken in this stage

These refine the approved requirements without renumbering them. Each one is tagged with the answer or source it comes from.

| ID | Decision | Effect on requirements | Source |
|---|---|---|---|
| D-US1 | No discharge-urgent queue priority in this build; all cases are processed in arrival order. The priority returns with the Phase 1.5 urgent-situation work | NFR2's second sentence is not built (US11.3) | [Q8], review R-01 |
| D-US2 | With storage consent, the stored record also holds the procedure and diagnosis from the discharge summary and the "what do you plan to do next" answer. The "I want the Insurance Check" count is an anonymous total, not linked to any session | Adds three fields to the FR2.5 list; closes the FR12.3 grouping gap | [Q9], review R-02 |
| D-US3 | Phone tests use the throttled 4G profile above, and every user screen must be usable within 4 seconds. The narrower device target (8 GB Android, not the PRD's low and mid-range phones) is an accepted risk, revisited if drop-off on weaker phones shows up after launch | Gives NFR11 a measurable test | [Q10], review R-03 |
| D-US4 | The meaning of "strong sign-in" for internal tools is set in NFR Requirements | FR15.2's sign-in method is deferred | [Q11], review R-04 |
| D-US5 | A glossary of medical and insurance terms is built. Every glossary term in a report has an inline explanation, and report text scores at least 60 on Flesch reading ease, checked automatically | Gives FR10.4 a measurable test; closes OQ4 | [Q12], review R-04 |
| D-US6 | The process for tracking IRDAI and NPPA changes and applying them within a window moves to Phase 2. The rule library still lets the builder or founder fix a rule, change a reference price and switch a check between live, shadow and off without a release | FR14.3 is not built (US11.5); FR14.1 and FR14.4 stay | [Q13], [Q16], review R-04 |
| D-US7 | Launch goes ahead with the launch checks that have passed their test. The rest stay in shadow mode, and the report lists which checks did not run | Relaxes FR8.2's "all 18 live at launch" and the scope document's launch condition | [Q14], review R-05 |
| D-US8 | Closed-beta bills have their own consent screen, go to a store separate from product data, and are blacked out before they enter the test set | Fills FR2.11 and FR16.4 for beta bills | [Q15], review R-06 |
| D-US9 | The report PDF is built on the device on both phone and desktop | Clarifies FR10.3 | [Q15], review R-07 |
| D-US10 | The 3-month deletion test is "no stored record is older than 3 months"; NFR Requirements fixes the exact length and the deletion-run tolerance | Rewords FR2.9's test | [Q15], review R-07 |
| D-US11 | Bill-reading accuracy is measured on at least 100 PDF bills and 100 photographed bills | Gives NFR4's reading targets a minimum sample | [Q15], review R-08 |
| D-US12 | "When are you checking?" (still in hospital, at discharge, already paid) is asked at the top of the upload screen, before the first file is added | Supplies the bill stage FR4.10 depends on; wireframe S3 | [Q17], [Q28] |
| D-US13 | The idle timer pauses while a report is being prepared. Once it is ready, only real taps and typing count as activity, and background progress checks never keep a session alive | Defines "idle" in FR2.8 | [Q18] |
| D-US14 | The first look shows the number of things worth a look and their topics, with no "Likely error" label and no rupee total until details are confirmed | Narrows what FR7.5's quick summary shows | [Q19] |
| D-US15 | At most 2 screens before upload: (1) what this does plus the privacy notice; (2) consent, relationship and any declaration | Wireframes S1–S2 | [Q20] |
| D-US16 | Upload resumes while the page stays open. After a reload, the user picks only the unfinished files again, and finished files are not re-sent | Scopes NFR11's resume | [Q22] |
| D-US17 | "Without a release" covers data and settings only (reference prices, thresholds, check settings, live/shadow/off). Changes to how a check works go through a normal release | Scopes FR14.1 | [Q23] |
| D-US18 | The person who makes a rule change may approve it, and the self-approval is logged | Applies NFR15 to a two-person team | [Q24] |
| D-US19 | Ratings offer fixed reasons only, with no free text | Narrows FR12.1's optional reason | [Q25] |
| D-US20 | Low-confidence findings never appear in the Top 3 or the Billing Desk Summary, and count under "worth asking about" in the Bill Breakdown | Clarifies FR8.19 against FR9.3, FR10.2 and FR8.15 | [Q26] |
| D-US21 | Stored line items carry the day of the stay (day 1, day 2, ...) instead of the calendar date | Resolves the FR2.5 and FR2.6 conflict (dates versus admission month) | [Q27] |
| D-US22 | The Billing Desk Summary holds the findings marked "Will ask". When none is marked, it holds the Top 3 | Aligns FR10.2 with FR11.1 and wireframe S9 | Design contribution A5 |
| D-US23 | Reproducibility (NFR6) is promised from the confirmed line items onwards, because AI bill reading need not return the same result twice | Clarifies NFR6's test | Developer and quality contributions |

---

## Group 1 — Start and consent

### US1.1 Start a check without an account

**As** Priya (C1), **I want** to start a bill check straight away with no sign-up, **so that** I can use it while standing at the billing desk.

- Priority: Must Have
- Covers: FR1.1, FR1.10, NFR11
- Depends on: US7.9

- AC1.1.1 Given I open the product on my phone, when I go from the start screen to the upload screen, then no screen asks for my name, phone number, email, password or ID, and consent leads directly to upload.
- AC1.1.2 Given the phone and network test conditions, when the start screen loads, then it is usable within 4 seconds (see AC3.2.3 and AC3.2.4 for every screen).
- AC1.1.3 Given any user screen, when I look for a language choice, then there is none and every user-facing text is in English.
- AC1.1.4 Given I try to reach the upload screen by its address without going through the start, when the page loads, then I am sent back to the start of the journey.
- AC1.1.5 Given any relationship to the patient, when I go from the start screen to upload, then I pass through at most two screens: (1) what this does and the privacy notice; (2) consent, relationship and any declaration [Q20].

INVEST: independent of other stories; small; testable end to end with Playwright.

### US1.2 Understand what the check does and does not do

**As** Anil (C4), **I want** a short explanation of what this check does and does not do before I start, **so that** I know it gives no guarantee and is not legal or medical advice.

- Priority: Must Have
- Covers: FR1.9, FR13.1
- Depends on: US1.1

- AC1.2.1 Given I am on the start screen, when the explainer appears, then it states that the check is informational only, is not legal, medical or financial advice, gives no guaranteed outcome, and that I am responsible for my own communications.
- AC1.2.2 Given I have read the explainer, when I continue, then I can reach consent only from this screen, which also shows the privacy notice (US1.3).
- AC1.2.3 Given I want to stop, when I am on the explainer, then a clear "Leave" option ends the visit without uploading anything.

INVEST: small; testable by text and navigation checks.

### US1.3 Read the privacy notice before uploading

**As** Meera (C2), **I want** a short, layered privacy notice before I upload my father's bill, **so that** I know what is collected, why, for how long and who sees it.

- Priority: Must Have
- Covers: FR1.2, FR1.12
- Depends on: US1.2

- AC1.3.1 Given I have not yet uploaded anything, when the privacy notice is shown, then its first layer covers what is collected, why, how long it is kept, who it is shared with and my rights, each in one or two plain sentences.
- AC1.3.2 Given the first layer, when I open "More detail", then the full notice opens without leaving the journey.
- AC1.3.3 Given a request to the upload endpoint from a session that has not been shown the notice, when it arrives, then it is refused.
- AC1.3.4 (launch checklist) Given the start-screen text "Nothing about you is saved" and the "before you go" text "anonymous", when launch is prepared, then a recorded founder or counsel approval of both texts exists, and without it the launch checklist fails.

INVEST: small.

### US1.4 Give consent to process my documents, or decline

**As** Priya (C1), **I want** to agree explicitly to my documents being processed, or to decline, **so that** nothing is read without my permission.

- Priority: Must Have
- Covers: FR1.3, FR1.11
- Depends on: US1.3

- AC1.4.1 Given I have seen the privacy notice, when I tick the processing consent and continue, then the upload screen opens.
- AC1.4.2 Given I have not given consent, when a file is sent to the upload endpoint by any route, including a direct request that skips the screens, then no file is accepted.
- AC1.4.3 Given I decline consent, when I choose "No", then the screen explains that the check cannot run without consent, nothing is uploaded, and I can go back or leave.
- AC1.4.4 Given I declined and then go back, when I give consent, then the journey continues normally.

INVEST: small; testable with component, integration and end-to-end tests.

### US1.5 Say whose bill this is

**As** Priya (C1), **I want** to say whose bill I am checking and confirm I may share it, **so that** the check runs on a lawful basis.

- Priority: Must Have
- Covers: FR1.5
- Depends on: US1.4

- AC1.5.1 Given I am at the relationship step, when I look at the choices, then they are phrased from my side under "Whose bill is this?": Mine; My spouse's; My parent's; My child's; Another family member's; I am an authorised representative; I am a professional helping the family.
- AC1.5.2 Given I have not chosen a relationship, or not ticked the authorisation box, when I press "Continue", then I stay on the screen, a text message next to the missing item says what is needed, focus moves to it, and the message is announced to screen readers.
- AC1.5.3 Given I choose "Mine", when I continue, then no further patient declaration is asked for.
- AC1.5.4 Given I choose anyone other than "Mine", when I continue, then I am asked "Is the patient under 18?", and the answer leads to the guardian declaration (US1.6) or the adult declaration (US1.7).

INVEST: small; the minor and adult paths are separate stories.

### US1.6 Declare that I am the guardian of a child patient

**As** Priya (C1), **I want** to declare that I am the parent or lawful guardian when the patient is a child, **so that** I can check my child's bill.

- Priority: Must Have
- Covers: FR1.6
- Depends on: US1.5

- AC1.6.1 Given I answered that the patient is under 18, when the declaration appears, then it asks me to declare that I am the parent or lawful guardian.
- AC1.6.2 Given the guardian declaration is not confirmed, when I try to continue or send a file directly, then the check does not continue, and the missing declaration is named in text.
- AC1.6.3 Given I confirm the declaration, when I continue, then upload opens and no proof document is requested.

INVEST: small.

### US1.7 Declare consent or authority for an adult patient, including one who has died

**As** Anil (C4), **I want** to declare that I have authority to share my late mother's bill, **so that** I can check it without proof documents.

- Priority: Must Have
- Covers: FR1.7
- Depends on: US1.5

- AC1.7.1 Given the patient is an adult other than me, when I continue, then I am asked to declare that I have the patient's consent, or authority because the patient is incapacitated or has passed away.
- AC1.7.2 Given the patient has died, when the declaration is shown, then the choice reads "My [relationship] has passed away", the text contains none of "deceased", "death certificate", "legal heir" or "proof", and it asks only for a declaration.
- AC1.7.3 Given the declaration is not confirmed, when I try to continue or send a file directly, then the check does not continue.
- AC1.7.4 Given I chose "has passed away", when the report and tips are generated, then no text refers to the patient's recovery or future care (for example "get well", "follow-up appointment", "your next stay"), and "What to do next time" (US5.9) is phrased for the family.
- AC1.7.5 Given I chose "has passed away", when a record is stored with storage consent, then that answer is not stored; it only sets the wording.

INVEST: small; wording is also covered by the wording checks in US5.10.

---

## Group 2 — Upload

### US2.1 Upload bills and documents from files

**As** Meera (C2), **I want** to upload the PDFs, photos and screenshots relatives sent me, **so that** I can check my father's bill from my laptop.

- Priority: Must Have
- Covers: FR4.1, FR4.2, FR4.3
- Depends on: US1.4, US7.9

- AC2.1.1 Given synthetic examples of each document type (final, interim, summary and itemised hospital bills, pharmacy bills, consumable and implant invoices and stickers, cost estimate or package letter, discharge summary, investigation reports, payment receipts), when I upload each one, then each is accepted.
- AC2.1.2 Given a digital PDF, a scanned PDF, a single photo, a multi-page set of photos and a screenshot, when I upload each, then each is accepted, and several files can make up one document.
- AC2.1.3 Given I am on the upload screen, when I look for help, then it explains how to save a bill received on WhatsApp to the device first (Requirements Analysis Q7).
- AC2.1.4 Given a file in an unsupported format (for example a .docx), when I upload it, then it is refused with a message naming the accepted formats and suggesting a next step ("Save it as a PDF or a photo and try again"), and other files stay.

INVEST: small. Files up to 25 MB need chunked or direct-to-storage upload, so the full version depends on the hosting and storage decision (OQ2). Bolt 1 can use a single small file.

### US2.2 Photograph a bill with guidance

**As** Priya (C1), **I want** tips before I photograph each page with my phone camera, **so that** the pages can be read.

- Priority: Must Have
- Covers: FR4.3
- Depends on: US1.4

- AC2.2.1 Given I choose "Take a photo" on my phone, when the tips appear before the camera opens, then they show three tips (lay the page flat, avoid glare, keep all edges in the frame) and an "Open camera" button, and I can choose not to see the tips again in this session.
- AC2.2.2 Given I have taken a photo, when I confirm it, then it is added as a page and I can take the next page.
- AC2.2.3 Given the camera cannot be opened or I cancel it, when I return, then I am offered file upload instead and nothing already added is lost.

INVEST: small; the phone's own camera app is used, so tips cannot appear inside it.

### US2.3 Stay within the upload limits

**As** Meera (C2), **I want** to be told clearly when I hit an upload limit, **so that** I know what to remove and nothing I already uploaded is lost.

- Priority: Must Have
- Covers: FR4.4
- Depends on: US2.1

- AC2.3.1 Given 20 files are already uploaded, when I add a 21st, then it is refused with a message naming the 20-file limit and suggesting "Remove a file to add another", and the 20 files stay (Requirements Analysis Q10).
- AC2.3.2 Given a file larger than 25 MB (25 × 1,048,576 bytes), when I upload it, then it is refused with a message naming the 25 MB per-file limit.
- AC2.3.3 Given the session already holds 60 pages, when I add another page, then it is refused with a message naming the 60-page limit, and the pages already accepted stay.
- AC2.3.4 Given a file of exactly 25 MB and the 20th file, when I upload them, then both are accepted (boundary values).
- AC2.3.5 Given 55 pages are accepted, when I add a 10-page PDF, then the whole file is refused with the 60-page limit named, it is deleted at once, and the 55 pages stay. Pages are counted on the server after upload.

INVEST: small; boundary tests are unit-testable.

### US2.4 Have each file recognised and corrected if needed

**As** Rohit (C3), **I want** each file labelled with its document type and to fix the label if it is wrong, **so that** the right checks use the right documents.

- Priority: Must Have
- Covers: FR4.5
- Depends on: US2.1

- AC2.4.1 Given synthetic files of each document type, when they are uploaded, then each is shown with its recognised type.
- AC2.4.2 Given a file labelled with the wrong type, when I change the type, then the new type is used by every later check.
- AC2.4.3 Given an image that is not a bill or hospital document (for example a photo of a street), when it is uploaded, then it is flagged "This doesn't look like a hospital document" with options to remove it or keep it.

INVEST: small; classification accuracy itself is measured under US9.4.

### US2.5 Be told about missing, duplicate or shuffled pages

**As** Rohit (C3), **I want** to know when pages are missing, repeated or out of order, **so that** I can fix the upload before the checks run.

- Priority: Must Have
- Covers: FR4.6, FR8.3
- Depends on: US2.4, US3.8

- AC2.5.1 Given a synthetic bill marked "Page 3 of 7" with page 3 missing, when it is processed, then I see "Page 3 of 7 is missing" and can add it (document check DOC-02).
- AC2.5.2 Given a synthetic bill whose page totals do not add up to the grand total, when it is processed, then I am told pages may be missing.
- AC2.5.3 Given the same page uploaded twice, when it is processed, then I am told about the duplicate and it is counted once.
- AC2.5.4 Given shuffled pages, when they are processed, then they are put in order and I am shown the order used.

INVEST: small; each detection is its own test.

### US2.6 Re-upload an unreadable page

**As** Priya (C1), **I want** to be told exactly which page could not be read, **so that** I can take a better photo of just that page.

- Priority: Must Have
- Covers: FR4.7
- Depends on: US2.2, US3.10

- AC2.6.1 Given a blurred synthetic page, when it is processed, then I see a request to re-upload it that names the page (for example "Page 4 of the pharmacy bill") and shows a small preview of that page from my device, so I can find the right paper.
- AC2.6.2 Given I re-upload that page, when the new page is readable, then it replaces the blurred one and the request disappears.
- AC2.6.3 Given I skip the re-upload, when the checks run, then the report lists the checks that could not use that page.

INVEST: small.

### US2.7 Get help when I only have a summary bill

**As** Priya (C1), **I want** to be told when my bill is only a summary and how to ask for the itemised one, **so that** the checks can see every line.

- Priority: Must Have
- Covers: FR4.8, FR8.3
- Depends on: US2.4, US3.8

- AC2.7.1 Given a synthetic summary-only bill, when it is processed, then I see an explanation that line-by-line checks need the itemised bill, pharmacy breakup and implant invoices (document check DOC-01).
- AC2.7.2 Given that explanation, when I open the request template, then it is filled with the hospital name and bill number from my bill and can be edited and copied.
- AC2.7.3 Given a fully itemised bill, when it is processed, then the summary-bill explanation does not appear.

INVEST: small; the letter is the itemised-bill request from US5.6.

### US2.8 Open a password-protected PDF

**As** Rohit (C3), **I want** to type the password of a protected PDF so that it can be read, **so that** I don't have to remove the protection myself.

- Priority: Should Have
- Covers: FR4.9
- Depends on: US2.1

- AC2.8.1 Given a password-protected synthetic PDF, when I upload it, then I am asked for its password.
- AC2.8.2 Given I enter the right password, when it is accepted, then the file is read, and the password appears in no store, log or error report.
- AC2.8.3 Given I enter a wrong password the maximum number of times (the number is set in Functional Design), when the last attempt fails, then I am offered to skip that file, and other files stay.
- AC2.8.4 Given I skip a protected file, or the session ends before I enter its password, when the skip or end is handled, then that file and any decrypted copy are deleted.

INVEST: small.

### US2.9 See which documents I have and which are missing

**As** Rohit (C3), **I want** a checklist of the documents I have uploaded and the ones that would help, **so that** I know what to ask the hospital for.

- Priority: Must Have
- Covers: FR4.10
- Depends on: US2.4, US2.11

- AC2.9.1 Given I have uploaded some documents, when I view the checklist, then each expected document is marked present, missing or optional, and the list updates as documents arrive.
- AC2.9.2 Given I said I am still in hospital and uploaded an interim bill, when the checks run, then only the current interim bill is checked and nothing carries over.
- AC2.9.3 Given I said I am at discharge or have already paid and uploaded a final bill, when I view the checklist, then it asks for the earlier interim bills, so the carry-forward check (DUP-07) has both.

INVEST: small.

### US2.10 Keep my upload going on a patchy connection

**As** Priya (C1), **I want** my upload to carry on after the connection drops, **so that** I don't have to start again in a hospital corridor.

- Priority: Must Have
- Covers: NFR11
- Depends on: US2.1

- AC2.10.1 Given three files where two have finished uploading, when the connection drops and comes back while the page is still open, then the third file continues from where it stopped and the first two are not sent again [Q22].
- AC2.10.2 Given the phone and network test conditions, when the upload screen loads, then it is usable within 4 seconds (AC3.2.3).
- AC2.10.3 Given the connection stays down, when I wait, then I am told the upload is paused and will continue when the connection returns.
- AC2.10.4 Given the page was reloaded during an upload, when it opens again within the session, then the files already received are listed as uploaded and I am asked to choose only the files that did not finish [Q22].
- AC2.10.5 Given files are uploading, when a file finishes, fails or pauses, then its status is written next to it and announced politely to screen readers.

INVEST: small once storage is chosen; blocked by the hosting and storage decision (OQ2).

### US2.11 Say where I am in the hospital stay

**As** Priya (C1), **I want** to say whether the patient is still in hospital, at discharge or the bill is already paid, **so that** the check asks for the right documents and suggests the right next steps.

- Priority: Must Have
- Covers: FR4.10, FR3.1
- Depends on: US1.4

- AC2.11.1 Given I reach the upload screen, when it opens, then the question "When are you checking?" with the choices "Still in hospital", "At discharge" and "Already paid" is at the top, and the first file cannot be added until it is answered [Q17], [Q28].
- AC2.11.2 Given I answered the question, when the case details are shown (US3.1), then my answer is shown and can be changed.
- AC2.11.3 Given my documents disagree with my answer (for example I said "Still in hospital" but uploaded a final bill with payment receipts), when the case details are shown, then the difference is pointed out and I am asked to confirm which is right.

INVEST: small; adds one question to the upload screen, not a new screen [Q20].

---

## Group 3 — Review and confirm

### US3.1 Confirm the case details read from my documents

**As** Priya (C1), **I want** the hospital, dates and payment details filled in from my bill for me to confirm, **so that** I don't have to type them.

- Priority: Must Have
- Covers: FR3.1, FR3.2
- Depends on: US2.4, US3.8

- AC3.1.1 Given a synthetic bill, when the case is created, then it holds hospital name and city, admission and discharge dates, treatment type (planned or emergency), payment mode, insurer or TPA if any, and the stay stage from US2.11, and it holds no patient name.
- AC3.1.2 Given the document contains a field, when the case details screen opens, then the field is pre-filled and editable, and I do not have to type it.
- AC3.1.3 Given a field the documents do not contain, when the screen opens, then the field is empty and marked "Please fill in".
- AC3.1.4 Given I change the stay stage or the payment mode, when the checks and next steps update, then the checklist (US2.9) and the order of letters (US5.6) follow the new value.

INVEST: small.

### US3.2 See where I am and what to do next

**As** Priya (C1), **I want** to see which step I am on and what to do next, **so that** I am never lost.

- Priority: Must Have
- Covers: FR3.3, NFR11
- Depends on: US1.1

- AC3.2.1 Given any user screen, when it is shown, then it displays the current step (upload, review or report) and one next-step action.
- AC3.2.2 Given I am waiting for processing, when the screen is shown, then it says what is happening and shows an estimated wait in minutes; given processing has failed, then the next-step action is "Try again".
- AC3.2.3 Given the phone and network test conditions, when each user screen in the journey is opened (start, consent, upload, case details, first look, confirm details, findings, report, letters, before you go), then its main action can be used within 4 seconds of navigation start [Q10].
- AC3.2.4 Given a production build, when the script-size check runs on every merge, then no user route loads more than 300 KB of compressed JavaScript on first load, and the merge is blocked if any route does.
- AC3.2.5 Given processing takes longer than the time shown, when the estimate passes, then the message changes to "Taking longer than usual. Still working." and never shows a stalled bar.
- AC3.2.6 Given processing status changes, when it changes, then the change is announced politely to screen readers at milestones only (reading pages, running checks, report ready), not on every progress tick.
- AC3.2.7 Given the step indicator, when a screen reader reads it, then it announces the step as text (for example "Step 2 of 3: Review") and marks the current step.

INVEST: small; a shared layout element.

### US3.3 See a first look while I confirm a few details

**As** Priya (C1), **I want** a quick first summary soon after uploading, **so that** I know early whether there is anything worth asking about.

- Priority: Must Have
- Covers: FR7.5, NFR1
- Depends on: US3.1, US3.9

- AC3.3.1 Given the quick summary is shown, when I read it, then it is labelled "First look — may change after you confirm a few details" and its button reads "Confirm a few details".
- AC3.3.2 Given synthetic typed or PDF bills in the test environment, when timing is measured, then the summary and first flags appear within 5 minutes of upload completion at the 90th percentile.
- AC3.3.3 Given synthetic photographed multi-page bills, when timing is measured, then the summary appears within 10 minutes at the 90th percentile.
- AC3.3.4 Given the first look is shown, when I read it, then it shows the total billed, advances paid, balance due, the three largest categories, and the number and topics of things worth a look.
- AC3.3.5 Given flags are shown before I confirm details, when they are labelled, then they read "worth a look", and carry neither the "Likely error" class nor a rupee amount for the flag [Q19].

INVEST: small; timing is a test-environment measurement that waits on hosting.

### US3.4 Confirm the details the system is unsure about

**As** Rohit (C3), **I want** to confirm or correct fields the system read with low confidence, **so that** findings are not built on misread numbers.

- Priority: Must Have
- Covers: FR5.7, FR7.2, FR7.4
- Depends on: US3.1

- AC3.4.1 Given extracted fields, when they are shown, then each has a confidence level and low-confidence fields are marked in text, not only by colour.
- AC3.4.2 Given a low-confidence misread digit that breaks the bill's arithmetic, when the checks run, then it is sent to me to confirm and is not reported as a billing error.
- AC3.4.3 Given I leave a low-confidence field unconfirmed, when the report is shown, then findings that depend on it are hidden and the report says why.
- AC3.4.4 Given a high-confidence reading whose arithmetic does not add up, when the checks run, then a calculation finding is raised and no confirmation is requested.

INVEST: small.

### US3.5 See and fix the lines read from my bill

**As** Rohit (C3), **I want** to see each line as read, next to its standard description, and fix any mistakes, even after the report appears, **so that** the findings reflect my real bill.

- Priority: Must Have
- Covers: FR5.8, FR6.1, FR10.5
- Depends on: US3.4

- AC3.5.1 Given the extracted lines, when I view them, then each shows the original text and the standard catalogue description.
- AC3.5.2 Given the report is shown, when I choose "Fix a line" and change a line amount, then the affected findings and the report update in the same session.
- AC3.5.3 Given I correct a line, when the report regenerates, then the correction is recorded with the line, and the report shows it was corrected by the user.
- AC3.5.4 Given I had marked or rated findings, when the report regenerates, then those marks and ratings stay on every finding that still exists.
- AC3.5.5 Given the report regenerates, when it is shown, then a short notice says what changed (for example "1 finding removed, total changed by ₹1,840") and is announced to screen readers.

INVEST: small; regeneration is shared with US6.7.

### US3.6 Be told which lines could not be read

**As** Meera (C2), **I want** a list of lines that could not be read, including lines in Hindi, **so that** I know what the check did not cover.

- Priority: Must Have
- Covers: FR5.6
- Depends on: US3.8

- AC3.6.1 Given synthetic lines in Devanagari or another Indian script, when the bill is read, then they appear in a "Could not be read" list.
- AC3.6.2 Given a line on that list, when the checks run, then it appears in no finding, and its amount counts under "could not be assessed" (AC4.11.5).
- AC3.6.3 Given a bill with no unreadable lines, when the review screen is shown, then the list does not appear.

INVEST: small.

### US3.7 Be warned when my documents don't match each other

**As** Rohit (C3), **I want** to be told when the name, UHID, dates or hospital differ between my documents, **so that** I can check I uploaded the right papers.

- Priority: Must Have
- Covers: FR7.1, FR8.3
- Depends on: US2.4, US3.8, US3.11

- AC3.7.1 Given a synthetic set where the UHID differs between the bill and the discharge summary, when the documents are checked, then I see a mismatch note naming both documents (document check DOC-03).
- AC3.7.2 Given admission dates that differ between the bill and the discharge summary, when the documents are checked, then I see a date mismatch note (DOC-04).
- AC3.7.3 Given all documents match, when the documents are checked, then no mismatch note appears.

INVEST: small.

### US3.8 Read header, lines and totals from a digital PDF

**As** Rohit (C3), **I want** every header field, line and total read from my PDF bill, **so that** the checks start from an accurate picture of it.

- Priority: Must Have
- Covers: FR5.1, FR5.2, FR5.3
- Depends on: US2.4

- AC3.8.1 Given a synthetic digital PDF bill with an answer key, when it is read, then the header data, every line item with its fields, and the bill totals listed in FR5.1 to FR5.3 match the answer key.
- AC3.8.2 Given a synthetic bill with a table that runs across pages, when it is read, then the lines on both pages are captured once, in order.
- AC3.8.3 Given the bill-reading service returns an error, when reading fails, then the user is told reading failed, a retry is offered (FR7.3), and the original file is still deleted (US7.5).

INVEST: one to five days with the bill reader chosen (OQ8). Accuracy on real bills is a launch gate in US9.4.

### US3.9 Have my documents processed in the background while I see progress

**As** Priya (C1), **I want** my documents processed in the background while I see progress, **so that** I can wait without the page freezing or timing out.

- Priority: Must Have
- Covers: FR3.3, FR7.3, NFR1, NFR2
- Depends on: US7.9

- AC3.9.1 Given an upload has completed, when processing starts, then the screen shows the current stage and updates without a reload.
- AC3.9.2 Given processing is running, when I choose "Discard everything" or the session ends, then processing is cancelled and nothing it produces afterwards is stored.
- AC3.9.3 Given a processing step fails, when the failure is handled, then the completed parts are kept for the session, a retry is offered, and the original file is still deleted.
- AC3.9.4 Given several sessions upload at once, when they are processed, then they are processed in arrival order, with no priority queue [Q8].

INVEST: a foundation story (origin: FR3.3, FR7.3, NFR1, NFR2). A full report can take up to 15 minutes, so it needs a job runner, which depends on hosting (OQ2).

### US3.10 Read photographed and scanned bills

**As** Meera (C2), **I want** the photos and scans my relatives send me read as well as a PDF, **so that** a blurry WhatsApp photo still gives me a useful check.

- Priority: Must Have
- Covers: FR5.1, FR5.2, FR5.3, FR5.5
- Depends on: US3.8

- AC3.10.1 Given a synthetic photographed bill with an answer key, when it is read, then the header data, lines and totals match the answer key.
- AC3.10.2 Given lakh and crore amounts with Indian digit grouping, merged cells, stamps and handwritten notes on synthetic photos, when they are read, then each is captured as in the answer key, or the line is marked low confidence (US3.4).
- AC3.10.3 Given a page too blurred to read, when it is processed, then it goes to the re-upload request in US2.6 instead of producing lines.

INVEST: one to five days after US3.8.

### US3.11 Read the discharge summary

**As** Rohit (C3), **I want** the discharge summary read for diagnosis, procedures, stay periods and implants, **so that** the checks can compare the bill with what was actually done.

- Priority: Must Have
- Covers: FR5.4
- Depends on: US3.8

- AC3.11.1 Given a synthetic discharge summary with an answer key, when it is read, then diagnosis, procedures performed, procedure dates, ICU and ward stay periods, implants used, condition at discharge and discharge medicines match the answer key.
- AC3.11.2 Given no discharge summary was uploaded, when the checks run, then the checks that need it are listed as "could not run — missing document" (AC4.14.2).

INVEST: small.

### US3.12 Sort each line into its category and add its details

**As** Rohit (C3), **I want** each line sorted into the right category with its drug, consumable, implant, fee or package details, **so that** the right checks run on it.

- Priority: Must Have
- Covers: FR6.2, FR6.3, FR6.4, FR6.5, FR6.6, FR6.7, FR6.8
- Depends on: US3.8, US8.4

- AC3.12.1 Given a synthetic bill, when lines are sorted, then every line has exactly one of the 21 standard categories.
- AC3.12.2 Given drug, consumable, implant, professional-fee and package lines, when they are sorted, then they carry their details as in the answer key: generic name, strength and form; IRDAI List II–IV mapping; make, model and batch when printed; fee type and doctor; inclusions and exclusions.
- AC3.12.3 Given a consumable, when it is mapped, then it cites a List II, III or IV entry and never a List I entry.
- AC3.12.4 Given a line that matches a List II–IV entry, when it is sorted, then it is tagged as commonly part of room, procedure or treatment charges.

INVEST: one to five days; drug-identification accuracy on real bills is reported in US9.4.

---

## Group 4 — Findings

Check criteria are tested on structured synthetic bill data with the checks set to `live` in a test copy of the rule library (see Common test conditions). Each check family can be built once the bill data model (US3.8, US3.12) and the check runner (US8.3) exist. Showing a check to users also needs its promotion in US10.1.

### US4.1 Calculation checks

**As** Rohit (C3), **I want** the bill's arithmetic checked, **so that** I can ask for plain calculation mistakes to be corrected.

- Priority: Must Have
- Covers: FR8.1, FR8.2, FR8.9
- Depends on: build US3.12, US8.3; release US10.1

- AC4.1.1 (CAL-01 Line amount) Given a synthetic line where quantity × rate does not equal the amount, when the check runs, then a "Likely error" names the line and page and shows the correct amount.
- AC4.1.2 (CAL-02 Section subtotals) Given lines that do not add up to their section subtotal, when the check runs, then a "Likely error" names the section and the difference.
- AC4.1.3 (CAL-03 Grand total) Given sections that do not add up to the bill total, when the check runs, then a "Likely error" shows the difference.
- AC4.1.4 (CAL-04 Discounts) Given a stated discount that was applied wrongly, when the check runs, then a "Likely error" shows the discount stated and the discount applied.
- AC4.1.5 (CAL-06 Credits) Given an advance or deposit on a receipt that is not credited on the bill, when the check runs, then a "Likely error" names the receipt.
- AC4.1.6 (CAL-07 Balance due) Given a balance due that differs from total minus discounts, advances, payments and any printed insurer-approved amount, when the check runs, then a "Likely error" shows the expected balance.
- AC4.1.7 Given a correct synthetic bill, when the calculation checks run, then no calculation finding appears.
- AC4.1.8 Given a calculation check that has not passed its test, when a bill is checked, then it runs in shadow mode and shows nothing [Q14].
- AC4.1.9 Given a difference within the rounding tolerance set in the rule library (for example 3 × ₹33.33 billed as ₹100.00), when a calculation check runs, then no finding appears; given a difference one paisa above the tolerance, then a "Likely error" appears. The tolerance value is set in Functional Design.

INVEST: one story per check family [Q4]; each criterion is an independent test.

### US4.2 Duplicate-charge checks

**As** Rohit (C3), **I want** charges billed twice to be found, **so that** I don't pay for the same thing twice.

- Priority: Must Have
- Covers: FR8.2, FR8.5
- Depends on: build US3.12, US8.3; release US10.1

- AC4.2.1 (DUP-01 Exact duplicates) Given the same item, date, quantity and amount repeated, when the check runs, then a "Likely error" lists the repeated lines.
- AC4.2.2 (DUP-06 Cross-bill duplicates) Given the same item on the hospital bill and on a separate pharmacy bill, when the check runs, then a "Likely error" names both lines.
- AC4.2.3 (DUP-07 Interim carry-forward) Given interim amounts counted twice in the final bill, when the check runs, then a "Likely error" names the interim bill and the final-bill line.
- AC4.2.4 Given the same investigation repeated on two days with a matching clinical note, when the checks run, then no duplicate finding appears.
- AC4.2.5 Given the same professional visit billed twice, when the checks run, then the repeated visit is found. This is not one of the 18 launch checks, so it runs in shadow mode until it passes its test.
- AC4.2.6 Given the same investigation repeated on two days with no matching clinical note, when the checks run, then the repetition is found. This also runs in shadow mode until it passes its test.

INVEST: one story per check family.

### US4.3 Length-of-stay checks

**As** Priya (C1), **I want** room days and charge dates checked against the stay, **so that** I am not billed for days outside it.

- Priority: Must Have
- Covers: FR8.2, FR8.9
- Depends on: build US3.12, US8.3; release US10.1

- AC4.3.1 (LOS-01 Room days vs stay) Given more room days billed than the stay allows under the hospital's day convention, when the check runs, then a finding shows the days billed and the days of stay.
- AC4.3.2 (LOS-03 Out-of-window charges) Given a charge dated before admission or after discharge, when the check runs, then a "Likely error" names the line and its date.
- AC4.3.3 Given a stay whose room days match, when the checks run, then no stay finding appears.
- AC4.3.4 Given admission at 23:30 and discharge at 00:30 the next day, when LOS-01 runs, then the days of stay follow the day convention recorded in the rule library, and the finding states that convention.

INVEST: small; the day convention is set in Functional Design.

### US4.4 Room-rate check

**As** Priya (C1), **I want** the room rate checked for consistency across the stay, **so that** I can ask about unexplained rate changes.

- Priority: Must Have
- Covers: FR8.2
- Depends on: build US3.12, US8.3; release US10.1

- AC4.4.1 (RM-02 Room rate consistency) Given the room rate changes within the same room category during the stay, when the check runs, then a "Likely error" lists each rate and its dates.
- AC4.4.2 Given the rate changes because the room category changed, when the check runs, then no finding appears.

INVEST: small.

### US4.5 Drug-price checks

**As** Rohit (C3), **I want** drug prices checked against the printed MRP and government ceiling prices, **so that** I can ask for overcharges to be corrected.

- Priority: Must Have
- Covers: FR8.2, FR8.11
- Depends on: build US3.12, US8.3, US8.4; release US10.1

- AC4.5.1 (RX-01 Above MRP) Given a drug charged above its printed MRP, when the check runs, then a "Likely error" names the line, the MRP and the amount above it.
- AC4.5.2 (RX-02 Above ceiling price) Given a scheduled formulation above its NPPA ceiling price, when the check runs, then a "Likely error" cites the ceiling price and its reference-data version.
- AC4.5.3 (RX-03 Rate inconsistency) Given the same drug at different unit rates in the same bill, when the check runs, then a "Likely error" lists the rates.
- AC4.5.4 Given any drug-price finding, when it is shown, then it cites no CGHS or PM-JAY rate and no internal benchmark as the reason.
- AC4.5.5 Given a drug charged exactly at its MRP or ceiling price, when RX-01 or RX-02 runs, then no finding appears.

INVEST: one story per check family.

### US4.6 Consumable and implant price checks

**As** Rohit (C3), **I want** consumable and implant prices checked against MRP and government caps, **so that** I can question overpriced items.

- Priority: Must Have
- Covers: FR8.2, FR8.11
- Depends on: build US3.12, US8.3, US8.4; release US10.1

- AC4.6.1 (CON-04 Consumables above MRP) Given a consumable charged above its printed MRP, when the check runs, then a "Likely error" names the line and the difference.
- AC4.6.2 (IMP-02 Price cap) Given a coronary stent or knee implant above its NPPA cap, or another implant above its MRP, when the check runs, then a "Likely error" cites the cap or MRP.
- AC4.6.3 Given an implant line without an invoice or sticker, when the checks run, then document guidance asks for the invoice (DOC-05) and the price check says it could not run.
- AC4.6.4 Given a consumable or implant charged exactly at its MRP or cap, when CON-04 or IMP-02 runs, then no finding appears.
- AC4.6.5 Given any consumable or implant finding, when it is shown, then it cites no CGHS or PM-JAY rate and no internal benchmark as the reason.

INVEST: small.

### US4.7 Package check

**As** Rohit (C3), **I want** the billed package price checked against my package quote, **so that** I can ask why the bill is higher than agreed.

- Priority: Must Have
- Covers: FR8.2, FR8.8
- Depends on: build US3.12, US8.3; release US10.1

- AC4.7.1 (PKG-01 Package rate) Given a billed package price that differs from the package letter or estimate, when the check runs, then a "Likely error" shows both amounts.
- AC4.7.2 Given items billed separately that the package normally includes, or a stay beyond the package days without explanation, or both package and itemised billing, when the checks run, then each is found. These are not launch checks, so they run in shadow mode until they pass their test.
- AC4.7.3 Given no package letter or estimate was uploaded, when the checks run, then the report says the package check could not run and names the missing document.

INVEST: small for PKG-01. The data source for "items the package normally includes" is not yet named (Domain Design).

### US4.8 Get guidance when documents or details are missing

**As** Rohit (C3), **I want** to be told which documents or details are missing from my bill, **so that** I know what to ask for before I can check everything.

- Priority: Must Have
- Covers: FR8.3, FR8.10, FR9.8
- Depends on: US3.8

- AC4.8.1 Given synthetic bills each missing one item (implant documentation DOC-05, drug batch, MRP or quantity DOC-06, discharge-summary details DOC-07, payment evidence DOC-08), when the document checks run, then each produces guidance naming what is missing.
- AC4.8.2 Given a bill with no dates, no quantities or no doctor names for professional fees, when the checks run, then each omission is reported as guidance.
- AC4.8.3 Given a finding that is not enough to act on, when it is shown, then it names the document that would settle it (for example the implant invoice).

INVEST: small; DOC-01 to DOC-04 are covered by US2.5, US2.7 and US3.7.

### US4.9 Understand each finding

**As** Priya (C1), **I want** each finding to explain what the item is, what was noticed, why it may be wrong and what to ask, **so that** I can raise it confidently.

- Priority: Must Have
- Covers: FR8.12, FR8.18, FR9.1, FR9.2, NFR5, NFR6
- Depends on: US4.1

- AC4.9.1 Given any finding, when it is shown, then it has check ID, category, class ("Likely error", "Worth asking" or "Explained"), confidence (High, Medium or Low), rupee amount potentially affected, evidence (bill line and page, regulatory reference), a "Who to ask" route and a plain-language explanation.
- AC4.9.2 Given any finding, when it is expanded, then it shows Explanation, Observation, Potential Concern and Follow-up Actions (what to ask, whom, what to attach, by when).
- AC4.9.3 Given any follow-up action, when it names whom to ask, then it is the billing desk, patient relations or the medical superintendent, and never an insurer, TPA or HR.
- AC4.9.4 Given the same confirmed line items and the same rule-library and reference-data versions, when the checks run twice, then the findings are identical (same check IDs, classes, confidence, amounts and evidence), and each finding traces to its source page and line, its rule ID and its reference-data version.
- AC4.9.5 Given any finding, when it is shown, then its class and confidence are written as text, and any colour or icon is extra.
- AC4.9.6 Given any finding in the list or the Top 3, when it is shown, then it uses a plain-language name (for example "Room charged for an extra day"), and the check ID appears only in the evidence detail and the report appendix.
- AC4.9.7 Given a findings list, when it is shown, then "Likely error" findings come before "Worth asking", which come before "Explained", and within each class the larger amount potentially affected comes first.
- AC4.9.8 Given a finding's evidence, when it is shown, then it gives the page, the line and the text as read; a bill-line image appears only if it can be drawn from the file still held on my own device in this browser tab, and no server-side image or crop is kept to show it.
- AC4.9.9 Given a finding missing any of the eight fields in AC4.9.1, when the report is built, then the finding is not shown and the failure is logged without bill content.
- AC4.9.10 Given a report regenerated after the rule library moved to a new version, when it is shown, then it uses one rule version throughout and shows that version.

INVEST: small; a shared finding layout.

### US4.10 See similar findings grouped, with no rupee counted twice

**As** Rohit (C3), **I want** similar findings grouped and the total potentially affected counted once, **so that** I am not shown an inflated figure.

- Priority: Must Have
- Covers: FR8.13, FR8.14
- Depends on: US4.9

- AC4.10.1 Given 12 duplicate glove lines, when the findings are shown, then they form one finding with 12 evidence lines.
- AC4.10.2 Given two findings that cover the same line, when the total is shown, then that line's amount is counted once.
- AC4.10.3 Given a grouped finding, when its amount is shown, then it equals the sum of its evidence lines to the paisa.

INVEST: small.

### US4.11 See the Bill Breakdown

**As** Anil (C4), **I want** the bill total split into likely errors, things worth asking, explained charges and what could not be assessed, **so that** I see the whole picture at a glance.

- Priority: Must Have
- Covers: FR8.15
- Depends on: US4.10

- AC4.11.1 Given a checked bill, when the Bill Breakdown is shown, then it has four parts: likely errors to correct; charges worth asking about; explained, with no billing concern; could not be assessed.
- AC4.11.2 Given any checked bill, when the four parts are added, then they equal the bill total exactly, to the paisa.
- AC4.11.3 Given a bill with no findings, when the breakdown is shown, then "likely errors" and "worth asking" are zero, the message is reassuring, and it lists which checks ran and which could not, so that nothing found is not taken to mean the bill was fully checked.
- AC4.11.4 Given the Bill Breakdown, when it is read by a screen reader or printed, then the four parts and their amounts are available as text in a table or list, not only as a chart.
- AC4.11.5 Given lines that could not be read (US3.6), when the breakdown is shown, then their amounts are in "could not be assessed".
- AC4.11.6 Given low-confidence findings, when the breakdown is shown, then their amounts count under "charges worth asking about", never under "likely errors" [Q26].

INVEST: small.

### US4.12 Have low-confidence findings folded away

**As** Priya (C1), **I want** findings the system is unsure about kept apart from the main list, **so that** I focus on the strongest points.

- Priority: Must Have
- Covers: FR8.19
- Depends on: US4.9

- AC4.12.1 Given a low-confidence finding of any amount, when the findings are shown, then it appears only inside the collapsed "Other things you may ask about" group.
- AC4.12.2 Given I open that group, when it expands, then each finding in it shows its low confidence in text.
- AC4.12.3 Given the group, when it is shown, then its label includes the count (for example "(5)"), it opens and closes with Enter or Space, and its open or closed state is announced.
- AC4.12.4 Given low-confidence findings, when the Top 3 (US5.1) and the Billing Desk Summary (US5.3) are built, then neither includes them [Q26].

INVEST: small.

### US4.13 Know whom to ask, and when to get professional advice

**As** Anil (C4), **I want** each finding to say who can sort it out, and to flag clinically sensitive ones, **so that** I don't argue about things the billing desk cannot settle.

- Priority: Must Have
- Covers: FR8.16, FR8.20
- Depends on: US4.9, US8.1

- AC4.13.1 Given any output, when the "Who to ask" routes are listed, then only "Ask the hospital", "Explained — no billing concern", "Complex — consider professional advice" and "You can sort this out" appear, and no Expert route appears.
- AC4.13.2 Given a finding in a category on the clinically sensitive list (drafted in Domain Design), when it is shown, then it carries "Complex — consider professional advice".
- AC4.13.3 Given a value threshold is set later in the rule library, when the next report runs, then labelling follows it without a release.
- AC4.13.4 Given a missing document only the family can supply, when the finding is shown, then its route is "You can sort this out".
- AC4.13.5 Given a "Complex" finding, when it is shown, then it says this check cannot judge it, suggests asking the treating doctor or the hospital's patient relations team for an explanation, and gives no clinical opinion.

INVEST: small.

### US4.14 Know which checks ran and which could not

**As** Meera (C2), **I want** to see which checks ran and which could not, and why, **so that** I know how complete the check was.

- Priority: Must Have
- Covers: FR7.3, FR8.1, FR8.2
- Depends on: US4.1, US8.3

- AC4.14.1 Given a synthetic bill, when the checks run, then the set of checks run equals the live checks whose inputs are present.
- AC4.14.2 Given the discharge summary is removed, when the report is regenerated, then it lists the checks that need the discharge summary as "could not run — missing document".
- AC4.14.3 Given a launch check is still in shadow mode at launch, when a report is shown, then the report lists it as "not yet available" and shows no finding from it [Q14].
- AC4.14.4 Given processing fails partway, when I see the result, then it shows what completed and which checks did not run, and offers a retry.
- AC4.14.5 Given some checks did not run, when they are listed, then they appear under plain names grouped by reason ("Still being tested", "Needs a document you can add"), each "Needs a document" entry has an "Add a document" action, and no check code is the only label.

INVEST: small.

---

## Group 5 — Report and letters

### US5.1 See the top three things to do now

**As** Priya (C1), **I want** the three most useful things to do now, **so that** I know exactly what to ask at the counter.

- Priority: Must Have
- Covers: FR9.3
- Depends on: US4.9

- AC5.1.1 Given a report with findings, when it is shown, then a "Top 3 things to do now" list appears at the top.
- AC5.1.2 Given findings with different expected value, likelihood of success and effort, when the list is built, then its order follows the ranking formula and tie-break set in Functional Design, and a tie between two findings is broken the same way every time.
- AC5.1.3 Given fewer than three actionable findings, when the list is shown, then it shows only those, with no filler items.
- AC5.1.4 Given low-confidence findings, when the list is built, then none is included [Q26].

INVEST: small.

### US5.2 Get the full Audit Report

**As** Rohit (C3), **I want** a complete report with every finding, its evidence and the checks done, **so that** I have one document to rely on.

- Priority: Must Have
- Covers: FR8.17, FR10.1, FR10.6, FR13.1, NFR2
- Depends on: US4.11, US4.14, US3.9

- AC5.2.1 Given a checked bill, when the report is generated, then it has seven parts: summary; Bill Breakdown; findings by category with evidence; line-item appendix with explanations; checks performed and not possible, with reasons; next steps and escalation ladder; disclaimers.
- AC5.2.2 Given any report, when its first page is shown, then it states the limitations: based on the documents provided; not legal or medical advice; no guarantee; insurance coverage not assessed.
- AC5.2.3 Given any report, when it is shown, then it states "this checks hospital billing only, not insurance coverage" and shows the rule-library and reference-data versions used.
- AC5.2.4 Given synthetic bills in the test environment, when timing is measured, then the full report is ready within 15 minutes of upload completion at the 90th percentile, with cases processed in arrival order [Q8]. Build and Test sets the sample size.
- AC5.2.5 Given any report, when it is generated, then its disclaimer section states that it is informational only, not legal, medical or financial advice, gives no guaranteed outcome, and that the user is responsible for their own communications.
- AC5.2.6 Given the full check is still running, when I am on the report screen, then the parts already ready are shown, the rest are marked "Still checking", and "Full report ready" is announced when it arrives.

INVEST: small for layout; the parts reuse earlier stories.

### US5.3 Get a one-page Billing Desk Summary

**As** Priya (C1), **I want** a one-page summary to show the billing executive, **so that** I can raise the points politely without a long document.

- Priority: Must Have
- Covers: FR10.2
- Depends on: US5.2, US6.1

- AC5.3.1 Given a report, when the Billing Desk Summary is generated as a PDF, then it is exactly one A4 page.
- AC5.3.2 Given the summary, when it is read, then each item quotes the bill number and date and the bill line and page, objective errors come first, and "please correct" items are separate from "please explain" questions (stakeholder S1).
- AC5.3.3 Given the summary, when its text is checked, then it contains no confidence labels and none of the banned words in US5.10.
- AC5.3.4 Given more chosen items than fit on one page, when the summary is generated, then it keeps them in ranking order and says how many more are in the full report.
- AC5.3.5 Given I am on a phone, when I choose "Show full screen", then only the summary is shown, in large text, with a "Close" button and no other controls, and it stays readable at 200% text size.
- AC5.3.6 Given the summary has been shown once, when the connection drops, then the summary and "Show full screen" still work.
- AC5.3.7 Given I marked one or more findings "Will ask", when the summary is generated, then it holds exactly those findings, with "please correct" items first.
- AC5.3.8 Given I marked none, when the summary is generated, then it holds the Top 3 (US5.1) and says "You can choose which points to raise", with a link back to the findings.
- AC5.3.9 Given a finding marked "Not pursuing", or a low-confidence finding, when the summary is generated, then it is not included [Q26].

INVEST: small; content rule per D-US22.

### US5.4 Download the report on desktop

**As** Meera (C2), **I want** to download the report as a PDF on my laptop, **so that** I can forward it to my family and keep a copy.

- Priority: Must Have
- Covers: FR10.3
- Depends on: US5.2

- AC5.4.1 Given I am on desktop, when I choose "Download", then a PDF of the report is built in the browser on my device and downloaded [Q15].
- AC5.4.3 Given I choose "Download" or "Save or share", when the PDF is built, then it is built from report data already in the browser, and no network request made from that moment on carries the PDF file or the report body, to our servers or anyone else.
- AC5.4.5 Given the report screen, when it first loads, then the PDF tool is not loaded; it loads only when I choose "Download" or "Save or share", and it is at most 200 KB compressed [Q21].
- AC5.4.6 Given a synthetic report with amounts in lakh and crore, when the PDF is opened in a PDF reader, then every amount shows the ₹ sign and the grouping used on screen (for example ₹1,20,000).
- AC5.4.7 Given the full report has just become ready, when the report screen is shown, then a "Download" (desktop) or "Save or share" (phone) action is visible without scrolling, with the line "Save a copy now. We can't send it to you later."
- AC5.4.8 Given the PDF, when it is opened, then its text is real, selectable text in reading order, not an image, and the on-screen report offers the same content.
- AC5.4.9 Given the PDF is being built, when it takes more than a moment, then "Preparing your report" is shown and announced; the time limit on the test device is set in NFR Requirements.
- AC5.4.10 Given the PDF file, when it is saved, then its name holds no patient name or identifier (for example `bill-check-2026-11-07.pdf`).

INVEST: small; this story builds the shared on-device PDF builder that US5.3, US5.11, US6.1 and US6.2 reuse. AC5.4.2 and AC5.4.4 moved to US5.11 when the phone path was split out.

### US5.5 Read the report in plain language

**As** Priya (C1), **I want** the report in simple English with medical and insurance terms explained where they appear, **so that** I can understand it without help.

- Priority: Must Have
- Covers: FR10.4, NFR13
- Depends on: US5.2

- AC5.5.1 Given the glossary of medical and insurance terms built for this product, when a report contains a glossary term, then the term has an inline explanation at its first use in each section [Q12].
- AC5.5.2 Given every explanation template and a fixed corpus of synthetic reports (excluding bill line descriptions), when they are scored automatically, then every one scores at least 60 on Flesch reading ease, not only the average [Q12].
- AC5.5.3 Given amounts and dates, when they are shown, then amounts use ₹ with lakh and crore grouping (including the boundaries ₹99,999, ₹1,00,000 and ₹1,00,00,000, and a negative credit amount), and times show the "IST" label.
- AC5.5.4 Given the user-facing text, when the lint check runs on every merge, then no user-facing string is hard-coded in components.

INVEST: small; the glossary is content built with this story.

### US5.6 Get letters I can edit and send

**As** Rohit (C3), **I want** ready-to-edit letters for the requests and queries I need to raise, **so that** I can send them without drafting from scratch.

- Priority: Must Have
- Covers: FR9.4, FR9.6
- Depends on: US4.9

- AC5.6.1 Given a synthetic case, when letters are generated, then these are available: itemised bill request; billing query list; request for the implant invoice or sticker and package inclusion list; request for a revised bill, a written explanation, or a refund or credit note.
- AC5.6.2 Given a letter, when I open it, then I can edit it before downloading or copying.
- AC5.6.3 Given any letter, when its wording is checked, then it cites the bill evidence, uses a respectful, factual tone and passes the wording checks in US5.10.
- AC5.6.4 Given any generated letter, when it is downloaded or copied, then it carries the disclaimer in AC5.2.5, and the disclaimer stays in place after I edit the letter body.
- AC5.6.5 Given the case is marked "Already paid" (US2.11), when the letters are listed, then the request for a revised bill, a refund or a credit note comes first.

INVEST: small.

### US5.7 Get an escalation letter and an NPPA complaint pointer

**As** Rohit (C3), **I want** a grievance escalation letter and a pointer for complaining to the NPPA about drug or device prices, **so that** I know how to take it further if the hospital does not respond.

- Priority: Should Have
- Covers: FR9.5
- Depends on: US5.6

- AC5.7.1 Given a finding above MRP or a ceiling price, when letters are generated, then an NPPA complaint pointer is included.
- AC5.7.2 Given any report with findings, when letters are generated, then a hospital grievance escalation letter is available.
- AC5.7.3 Given no price finding, when letters are generated, then no NPPA pointer appears.

INVEST: small.

### US5.8 See the escalation ladder and its deadlines

**As** Anil (C4), **I want** to see who to go to next if the billing desk does not help, and by when, **so that** I can raise issues even after paying.

- Priority: Must Have
- Covers: FR9.7
- Depends on: US5.2, US8.4

- AC5.8.1 Given any report, when the escalation ladder is shown, then it lists billing desk, patient relations, medical superintendent or grievance officer, State authority where applicable, NPPA for drug and device prices, and the Consumer Commission.
- AC5.8.2 Given the hospital-side timeline reference data, when the ladder is shown, then each step shows its deadline.
- AC5.8.3 Given any report, whether or not the bill is paid, when the escalation ladder is shown, then it states that issues can still be raised after the bill is paid, and nothing on the screen suggests delaying payment or discharge.

INVEST: small.

### US5.9 Get tips for next time

**As** Priya (C1), **I want** a few tips for a future hospital stay, **so that** it is easier next time.

- Priority: Should Have
- Covers: FR9.9
- Depends on: US5.2

- AC5.9.1 Given any report, when it is shown, then a "What to do next time" section appears.
- AC5.9.2 Given the tips, when they are read, then they are informational only and pass the wording checks in US5.10.
- AC5.9.3 Given the patient has passed away (US1.7), when the tips are shown, then they are phrased for the family and do not refer to the patient's future care.

INVEST: small.

### US5.10 Read respectful wording everywhere

**As** Anil (C4), **I want** every screen, report and letter to use calm, respectful wording, **so that** I am not pushed into a fight while grieving.

- Priority: Must Have
- Covers: FR13.2, FR13.3, FR13.4, NFR17
- Depends on: US5.2, US5.6

- AC5.10.1 Given every template and a generated corpus of synthetic reports and letters, when an automated wording test runs on every merge, then it finds zero uses of "fraud", "scam", "cheating" or "illegal", matched case-insensitively and including their forms ("fraudulent", "scammed", "cheated", "illegally").
- AC5.10.2 Given any non-objective finding, when its wording is checked, then it uses "appears", "may", "we could not find" or "worth asking", and cites the bill line and page.
- AC5.10.3 (review) Given every template and fixed text, when they are reviewed, then nothing encourages withholding payment in a way that blocks discharge or treatment.
- AC5.10.4 Given any screen or report, when it is checked, then it has no advertisement and no referral link from a hospital, insurer or lender.
- AC5.10.5 Given every template and the generated corpus, when an automatic phrase check runs on every merge, then it finds none of "do not pay", "refuse to pay" or "withhold payment".

INVEST: small.

### US5.11 Save or share the report from my phone

**As** Priya (C1), **I want** to save the report on my phone or share it straight to my family, **so that** whoever is at the hospital has it.

- Priority: Must Have
- Covers: FR10.3
- Depends on: US5.4

- AC5.11.1 (previously AC5.4.2) Given I am on a phone, when I choose "Save or share", then the PDF is built on the phone, and once it is ready a "Share" button opens the phone's own share menu from my tap.
- AC5.11.2 (previously AC5.4.4) Given the browser cannot share files, when I choose "Save or share", then the PDF downloads instead.
- AC5.11.3 Given the share menu opened and I cancelled it, when the "saved" state is checked, then the report is not counted as saved (US7.1).
- AC5.11.4 (launch checklist) Given a real phone, when "Save or share" is used once before launch, then the system share menu opens with the PDF attached.

INVEST: small; tested with a share-menu stub in Playwright and once by hand on a real phone.

---

## Group 6 — Act and give feedback

### US6.1 Choose which findings I will raise

**As** Priya (C1), **I want** to mark each finding "Will ask" or "Not pursuing", **so that** my report has a short checklist for the counter.

- Priority: Must Have
- Covers: FR11.1
- Depends on: US4.9

- AC6.1.1 Given a finding, when I mark it "Will ask", then it appears in the checklist in the downloaded report and in the Billing Desk Summary (US5.3).
- AC6.1.2 Given a finding marked "Not pursuing", when I download the report, then it is not in the checklist but stays in the full findings.
- AC6.1.3 Given I mark nothing, when I download the report, then the checklist section says no items were chosen.

INVEST: small.

### US6.2 Add a dispute pack to my report

**As** Rohit (C3), **I want** to add a dispute pack to my download, **so that** I have the findings, evidence, letters and escalation ladder together.

- Priority: Should Have
- Covers: FR11.2
- Depends on: US5.6, US5.8

- AC6.2.1 Given I choose the dispute pack, when I download, then it includes the findings, an evidence index, the letters and the escalation ladder.
- AC6.2.2 Given I do not choose it, when I download, then only the report is included.

INVEST: small.

### US6.3 Know that nothing is sent on my behalf

**As** Anil (C4), **I want** to be sure the product never contacts the hospital or anyone else for me, **so that** I stay in control of what is said.

- Priority: Must Have
- Covers: FR11.3
- Depends on: none

- AC6.3.1 Given the complete end-to-end journey and the server's outbound request log, when they are checked, then every outbound destination is on an allow-list of our own services and approved data processors, and none is a hospital, insurer, email or SMS service.
- AC6.3.2 Given the report screen, when I read the next steps, then it says the product does not contact anyone for me.

INVEST: small.

### US6.4 Rate each finding

**As** Meera (C2), **I want** to say whether each finding was helpful, not helpful or wrong, **so that** the checks get better.

- Priority: Must Have
- Covers: FR12.1
- Depends on: US4.9

- AC6.4.1 Given a finding, when I rate it, then I can choose helpful, not helpful or wrong, and optionally pick a reason from a fixed list (for example "the hospital already explained this", "the amount is wrong"); there is no free-text box [Q25].
- AC6.4.2 Given I rate a finding, when the session ends, then the rating is kept only with storage consent (US7.2).

INVEST: small.

### US6.5 Say what I plan to do next

**As** Priya (C1), **I want** to say what I plan to do with the findings, **so that** the team learns what families do with the check.

- Priority: Must Have
- Covers: FR12.2
- Depends on: US5.2

- AC6.5.1 Given the report is shown, when the outcome question appears, then it asks only what I plan to do next and does not ask whether an already-paid bill was revised.
- AC6.5.2 Given I answer and later give storage consent, when the session ends, then the answer is stored with the de-identified record [Q9].
- AC6.5.3 Given I answer but do not give storage consent, when the session ends, then the answer is not kept.

INVEST: small.

### US6.6 Say I want the Insurance Check

**As** Rohit (C3), **I want** to show interest in a future Insurance Check without giving contact details, **so that** the team knows it is wanted.

- Priority: Should Have
- Covers: FR12.4
- Depends on: US5.2

- AC6.6.1 Given my payment mode is cashless or reimbursement, when I click "I want the Insurance Check", then one anonymous count is added and no other data is collected; for self-pay cases the button is not shown.
- AC6.6.2 Given the count, when it is stored, then it is a single total with no session ID, case ID or exact timestamp [Q9].
- AC6.6.3 Given I click twice in one session, when the count is updated, then it increases once; the flag that prevents double counting lives only in the session and is discarded when it ends.

INVEST: small.

### US6.7 Add a document after the report

**As** Meera (C2), **I want** to add the discharge summary after I have seen the report, **so that** the report updates without starting again.

- Priority: Must Have
- Covers: FR4.11, FR10.5
- Depends on: US5.2

- AC6.7.1 Given the report is shown, when I choose "Add a document" and upload a discharge summary, then an updated report is produced in the same session.
- AC6.7.2 Given the session has ended, when I try to add a document, then I cannot, and I am told to start a new check.
- AC6.7.3 Given I had corrected lines and marked findings, when the report regenerates, then my corrections and marks are kept for every finding that still exists.
- AC6.7.4 Given a finding I marked "Will ask" no longer exists after regeneration, when the change notice is shown (AC3.5.5), then it names that finding.

INVEST: small.

---

## Group 7 — Privacy and access throughout the session

### US7.1 Be reminded to save before I go

**As** Priya (C1), **I want** a reminder to save the report before I leave, **so that** I don't lose it.

- Priority: Must Have
- Covers: FR2.1, FR2.7
- Depends on: US5.4, US7.9

- AC7.1.1 Given I have not saved the report, when I choose any in-product exit ("Start a new check", "Discard everything" or "Finish"), then I am reminded to save it first and told it cannot be retrieved later.
- AC7.1.2 Given the session has ended (by timeout, "Discard everything" or finishing), when I press back, open an old link or return to the tab, then the start screen is shown and no bill data from the ended session is rendered, including from the browser's back-forward cache or router cache.
- AC7.1.3 Given I have not saved the report, when I reload or close the tab, then the browser's own leave-page prompt is requested (best effort; its text is set by the browser).
- AC7.1.4 Given I saved the report and then it was regenerated, when I try to leave, then I am reminded that my saved copy is out of date.
- AC7.1.5 Given I finish a check, when I choose "Start a new check", then after the save reminder the old session ends, and nothing from it (files, findings or marks) appears in the new one.

INVEST: small.

### US7.2 Choose whether to share de-identified data after the report

**As** Meera (C2), **I want** to decide, after seeing the report, whether de-identified data from it may be kept for 3 months, **so that** I can help improve the checks without giving up privacy.

- Priority: Must Have
- Covers: FR1.4, FR2.4, FR2.5, FR2.6, FR12.3, NFR8
- Depends on: US5.2

- AC7.2.1 Given the report has been shown, when the storage question appears, then it is optional, and declining changes neither the report nor the download.
- AC7.2.2 Given I decline, when the session ends, then no record linked to the session exists in any store.
- AC7.2.3 Given I agree, when the record is stored, then it holds only: line items with their categories, quantities, rates and amounts, each dated by day of the stay (day 1, day 2, ...) and not by calendar date [Q27]; hospital; city; findings; ratings with their fixed reasons; age band; gender; admission month; length of stay; procedure; diagnosis; and the "plan" answer [Q9].
- AC7.2.4 Given synthetic bills containing the patient's name, UHID or IP number, phone, address and policy number, including when the name or phone number appears inside a line-item description, when the record is stored, then none of those appears, and age is stored as a 10-year band.
- AC7.2.5 Given stored records, when they are grouped for internal quality work by hospital, city, procedure and finding type, then only consented records are included.
- AC7.2.6 Given the report has been shown and I neither agree nor decline, when the session ends (I close the tab, it times out or I discard), then this counts as declining and no record is kept.
- AC7.2.7 Given the storage question, when it is shown, then it describes the data in plain words ("bill lines with names, numbers and addresses removed, deleted after 3 months"), does not use the word "anonymous", and shows "Yes, keep it" and "No thanks" as buttons of equal size and weight, with neither pre-selected.

INVEST: five to seven days; removing identifiers (AC7.2.4) may become its own story in Units Generation.

### US7.3 Discard everything at any point

**As** Anil (C4), **I want** to stop and delete everything at any moment, **so that** I can walk away if it becomes too much.

- Priority: Must Have
- Covers: FR1.8
- Depends on: US1.4, US3.9

- AC7.3.1 Given any point in the session, when I choose "Discard everything" and confirm, then the session's files and extracted data no longer exist in storage and I am returned to the start.
- AC7.3.2 Given I choose "Discard everything" but then cancel, when the dialog closes, then nothing is deleted.
- AC7.3.3 Given processing is still running, when I choose "Discard everything" and confirm, then the running job stops or its output is thrown away, and no extracted data, finding or original file from the session exists afterwards.

INVEST: small.

### US7.4 Have an idle session end safely

**As** Priya (C1), **I want** a warning before an idle session ends, and the data cleared when it does, **so that** my bill is not left open on a shared phone.

- Priority: Must Have
- Covers: FR2.8
- Depends on: US7.9

- AC7.4.1 Given 25 minutes without activity, when the timer reaches 25:00, then a warning appears with a "Keep going" button.
- AC7.4.2 Given the warning, when I press "Keep going", then the idle timer resets.
- AC7.4.3 Given 30 minutes without activity, when the timer reaches 30:00, then the session ends and its in-progress data is gone.
- AC7.4.4 Given 24 minutes 59 seconds without activity, when the timer is checked, then no warning is shown.
- AC7.4.5 Given a report is being prepared, when the idle timer is checked, then the time spent preparing does not count as idle; once the report is ready, only my taps and typing count as activity, and the page's background progress checks never do [Q18].
- AC7.4.6 Given my phone slept, the tab was in the background or I closed the tab for more than 30 minutes of idle time, when I return, then the server has already ended the session and deleted its data, whether or not the warning appeared.
- AC7.4.7 Given the warning appears, when it is shown, then it is announced to screen readers, keyboard focus moves to "Keep going", and it states how many minutes are left and that the documents will be deleted.
- AC7.4.8 Given the session ended while I was away, when I return to the tab, then I see "Your session ended and your documents were deleted. If you saved the report, it is still on your device." with a "Start a new check" button, and no error code.

INVEST: small; tested with a mocked clock.

### US7.5 Have my original files deleted once read

**As** Meera (C2), **I want** the files I upload to be deleted once they have been read, **so that** copies of my father's medical bill do not linger anywhere.

- Priority: Must Have
- Covers: FR2.3, NFR8
- Depends on: US2.1

- AC7.5.1 Given a synthetic bill, when its content has been extracted, then the original file no longer exists.
- AC7.5.2 Given extraction fails, when the failure is handled, then the original file is still deleted.
- AC7.5.3 Given the session ends, when storage is checked, then no original file from it remains.
- AC7.5.4 Given synthetic bills that carry a unique marker string, when the deletion tests have run, then the marker appears in no temporary folder, log, error report or cache directory.
- AC7.5.5 Given an upload that is abandoned, discarded or timed out partway, when the session ends, then no partial upload or chunk of it remains in any store.
- AC7.5.6 Given a synthetic bill, when its content has been extracted, then every derived page image, thumbnail and decrypted copy is deleted along with the original.
- AC7.5.7 Given the bill-reading service, when a file is sent to it, then the call uses the service's no-retention option or is followed by its delete call, and the service's no-retention and no-training terms are recorded.
- AC7.5.8 Given a file that was refused (unsupported format, over a limit, too many wrong passwords, or skipped), when the refusal is handled, then no copy of it remains in storage, temporary folders, logs or error reports.

INVEST: small; a zero-tolerance guardrail tested on every merge, with the marker test set up in Bolt 1.

### US7.6 Keep my bill visible only to me

**As** Priya (C1), **I want** my bill and findings never shown to anyone else or kept in shared storage, **so that** another person cannot see them.

- Priority: Must Have
- Covers: FR2.2, FR2.10
- Depends on: US5.2, US7.9

- AC7.6.1 Given two concurrent sessions, when each views its report, then neither can see the other's bill data, including through a cached or prefetched page.
- AC7.6.2 Given every route that shows bill data, when its responses and the build output are checked, then the responses are marked private and not cacheable, those routes are rendered per request and not prebuilt, and no cached function reads bill data.
- AC7.6.3 Given every store, when a data inventory is taken, then it holds no name, mobile number, email, login, device-linked profile or persistent device identifier.
- AC7.6.4 Given every route that shows bill data or findings, when the build output and response headers are inspected, then none of those routes is prerendered or uses `use cache`, and their responses carry `Cache-Control: no-store`.
- AC7.6.5 Given links to screens with bill data, when a user screen loads, then those screens are not prefetched with bill data in them.

INVEST: small; a code-scan rule blocking `use cache` in bill-data modules enforces this on every merge.

### US7.7 Have stored data deleted after 3 months

**As** Meera (C2), **I want** any de-identified data I agreed to share deleted automatically after 3 months, **so that** it is not kept forever.

- Priority: Must Have
- Covers: FR2.9
- Depends on: US7.2

- AC7.7.1 Given stored records created at known times (injected clock), when the stores are checked at any moment, then no record is older than 3 months as defined in NFR Requirements, allowing for how often deletion runs [Q15].
- AC7.7.2 Given a record that reaches that age, when the next deletion run happens, then the record is removed; given a deletion run fails, then an alert is raised.

INVEST: small; the deletion job is scheduled work, which depends on hosting (OQ2).

### US7.8 Use the check with large text, a keyboard or a screen reader

**As** Meera (C2), helping a parent such as Kamala, **I want** the screens to work with large text, keyboards and screen readers on phone and desktop, **so that** older relatives can follow along.

- Priority: Must Have
- Covers: NFR12
- Depends on: US1.1

- AC7.8.1 Given every user screen, when automated accessibility checks run, then they find no WCAG 2.2 level A or AA failure.
- AC7.8.2 (review) Given a manual screen-reader pass with TalkBack on Android and NVDA or VoiceOver on desktop, when the journey is completed, then every step can be done and every finding read.
- AC7.8.3 Given text enlarged to 200%, when the screens are used, then no content is cut off and no action is hidden.
- AC7.8.4 Given only a keyboard, when I complete the whole journey from start to download, then every action can be reached, focus is always visible and never hidden behind a sticky header or footer, and the first stop on every screen is "Skip to content".
- AC7.8.5 Given a screen 320 CSS px wide, when any user screen is shown, then nothing scrolls sideways except bill-line tables, which scroll inside their own area.
- AC7.8.6 Given any tap target on a phone, when it is measured, then it is at least 44 × 44 CSS px.
- AC7.8.7 Given text and controls, when contrast is measured, then text is at least 4.5:1, and control borders and focus rings at least 3:1.
- AC7.8.8 Given the "reduce motion" setting is on, when progress or transitions are shown, then no animation runs beyond a simple state change.
- AC7.8.9 Given a finding opens, when it opens and closes, then focus moves to its heading and then back to the list item it came from.

INVEST: small; the automated checks run on every user-facing story.

### US7.9 Keep my check tied to this browser only

**As** Priya (C1), **I want** my check tied to this browser and never reachable by a link someone else could use, **so that** nobody else can open my bill.

- Priority: Must Have
- Covers: FR2.1, FR2.2, FR1.8, NFR8
- Depends on: none

- AC7.9.1 Given a new visit, when the journey starts, then an unguessable session identifier is set in a cookie the page's scripts cannot read, sent only over secure connections and only to our own site, and it never appears in a URL.
- AC7.9.2 Given another browser with the same URL, when it opens a screen from my check, then it gets the start screen and no data from my check.
- AC7.9.3 Given a session identifier that has ended or never existed, when it is sent, then it is refused and no case is shown.

INVEST: a foundation story (origin: FR2.1, FR2.2, FR2.8, FR1.8, NFR8), built in Bolt 1.

---

## Group 8 — Rule library (internal)

### US8.1 Maintain rules and reference data without a release

**As** the builder or founder (I1), **I want** to fix a reference price, threshold or check setting in the rule library, **so that** a wrong or outdated value can be corrected without waiting for a release.

- Priority: Must Have
- Covers: FR14.1, FR14.2, FR14.5, FR14.6, FR8.16
- Depends on: US8.4, US10.2

- AC8.1.1 Given an approved change to a reference price, threshold or check setting, when the next report runs, then it uses the new value, with no deployment [Q23].
- AC8.1.2 Given a rule or reference item missing its source, effective dates, version, owner or review date, when I try to save it, then it cannot be saved.
- AC8.1.3 Given any rule, when its owner is shown, then it names the builder or the founder.
- AC8.1.4 Given the standard catalogue of drugs, consumables, procedures and investigations, when an entry is added, then it is versioned.
- AC8.1.5 Given a change to how a check works (its logic, not its data or settings), when it is needed, then it goes through a normal release and cannot be made from the rule library [Q23].
- AC8.1.6 Given a report already being generated when a rule change is approved, when it finishes, then it uses one version throughout.

INVEST: lighter internal story [Q5]; regulatory-change tracking is out of this build (US11.5) [Q16].

### US8.2 Approve a rule change before it takes effect

**As** the builder or founder (I1), **I want** every rule change approved before it goes live, **so that** a mistake in the library cannot reach families unchecked.

- Priority: Must Have
- Covers: NFR15
- Depends on: US8.1

- AC8.2.1 Given a saved rule change, when it has not been approved, then reports keep using the previous version.
- AC8.2.2 Given a rule change, when it is approved, then the change log records who changed it, what changed, why and the source.
- AC8.2.3 Given a change log entry missing any of who, what, why or source, when approval is attempted, then it is refused.
- AC8.2.4 Given the person who made a change approves it, when the approval is recorded, then it is allowed and logged as a self-approval [Q24].

INVEST: full criteria [Q5].

### US8.3 Run each check as live, shadow or off

**As** the builder or founder (I1), **I want** each check to run as live, shadow or off, **so that** only tested checks are shown and new ones can be measured quietly.

- Priority: Must Have
- Covers: FR14.4, FR8.4, FR8.6, FR8.7
- Depends on: US3.12

- AC8.3.1 Given a check set to shadow, when a synthetic bill is checked, then the check runs, its result is recorded for measurement, and nothing is shown to the user.
- AC8.3.2 Given a check set to off, when a bill is checked, then it does not run.
- AC8.3.3 Given a check set to live, when a bill is checked, then its findings are shown; moving a check to live is possible only through US10.1.
- AC8.3.4 Given a newly added check, including the unusual-charge, consumable-quantity, Wave 2 and Wave 3 checks, when it is first saved, then its state is shadow, and it cannot be created directly as live.

INVEST: the runner every check uses; it reads check states from stored settings and needs no admin screen (US8.5).

### US8.4 Load the launch reference data with its sources

**As** the builder or founder (I1), **I want** NPPA ceiling prices, device caps, IRDAI Lists II–IV and the grievance timelines loaded with their source and version, **so that** the price checks have data on day one.

- Priority: Must Have
- Covers: FR14.1, FR14.2, FR8.11
- Depends on: none

- AC8.4.1 Given the launch data files, when they are loaded, then every item carries source, effective dates, version, owner and review date, and an item missing any of these is rejected.
- AC8.4.2 Given loaded reference data, when a report runs, then it records the reference-data version it used.

INVEST: a foundation story (origin: FR14.1, FR14.2, FR14.5, FR8.11, PRD D22).

### US8.5 Switch a check's state from the internal tools

**As** the builder or founder (I1), **I want** a screen to switch a check between shadow and off, and to start a promotion, **so that** I don't need a release to pause a check.

- Priority: Must Have
- Covers: FR14.4
- Depends on: US8.3, US10.2

- AC8.5.1 Given a signed-in user with the domain admin role, when they switch a check between shadow and off, then the next report follows the new state and the change is logged.
- AC8.5.2 Given any user, when they try to switch a check to live from this screen, then they are sent to the promotion flow in US10.1.

INVEST: lighter internal story.

---

## Group 9 — Labelled Test Set (internal)

### US9.1 Add consented bills to the test set

**As** the builder or founder (I1), **I want** to add real bills from the personal network with written consent, and synthetic bills with their answer keys, **so that** the checks can be measured on realistic bills.

- Priority: Must Have
- Covers: FR16.1, FR16.2, FR16.3, FR16.4, FR16.6, FR16.8, FR16.11, FR16.13
- Depends on: US10.2

- AC9.1.1 Given a real bill, when it is added, then its source and consent are recorded, and a bill from a public post without the patient's consent is refused.
- AC9.1.2 Given a real bill, when it is added, then the patient's name, UHID or IP number, phone, address and policy number are blacked out before it is stored, and every real bill is checked for a visible identifier before it enters the set.
- AC9.1.3 Given a synthetic bill, when it is added, then it is linked to its answer key.
- AC9.1.4 Given test-set data, when storage is inspected, then it sits in a store separate from product data, outside git and out of the AI coding assistant's reach.
- AC9.1.5 Given the test set, when the coverage report is run, then it shows the count against the 300–500 target and the mix of hospital sizes, States, procedure types, bill formats and capture quality.
- AC9.1.6 Given there is no recorded founder decision on data residency, when a real bill is added, then it is refused.

INVEST: lighter internal story.

### US9.2 Label bills independently and reconcile

**As** the builder or founder (I1), **I want** both of us to label each real bill without seeing the other's labels or any AI output, then reconcile, **so that** labels are trustworthy.

- Priority: Must Have
- Covers: FR16.5, FR16.7
- Depends on: US9.1

- AC9.2.1 Given a real bill, when it is labelled, then the labelling tool hides each person's labels from the other until both have submitted, and the bill ends with two labels and a reconciliation record giving the reason for each difference.
- AC9.2.2 Given the labelling tool, when a label is entered, then nothing is pre-filled from the AI being tested.

INVEST: lighter internal story.

### US9.3 Withdraw a contributor's bill

**As** the builder or founder (I1), **I want** to remove a contributor's bill completely when they ask, **so that** their consent can be withdrawn at any time.

- Priority: Must Have
- Covers: FR16.9
- Depends on: US9.1

- AC9.3.1 Given a contributor withdraws a bill, when the withdrawal is processed, then the redacted original, its labels and its reconciliation record are deleted.
- AC9.3.2 Given a withdrawn bill, when the next accuracy run happens, then the bill is not in it.
- AC9.3.3 Given a past promotion record that used the withdrawn bill, when it is viewed, then it keeps its counts but shows that one bill has since been withdrawn.
- AC9.3.4 Given a withdrawal, when it is processed, then the action is recorded in the access log with who did it and when.

INVEST: full criteria [Q5].

### US9.4 Measure accuracy and coverage

**As** the builder or founder (I1), **I want** to run an accuracy measurement on the test set whenever prompts, models or rules change, **so that** we know whether reading and checks meet their targets before release.

- Priority: Must Have
- Covers: FR16.10, NFR4, NFR16
- Depends on: US9.2

- AC9.4.1 Given an accuracy run, when it completes, then it reports line-reading accuracy for digital PDFs and good-quality photos, and precision per check for "Likely error" and "Worth asking".
- AC9.4.2 Given a change to a prompt, model or rule, when a release is prepared, then the release record links an accuracy run on the changed version.
- AC9.4.3 Given the coverage report, when it is run, then it breaks the set down by State, hospital type, procedure, format and capture quality.
- AC9.4.4 Given a check below its precision target, when the run completes, then the check is marked for shadow mode.
- AC9.4.5 (launch gate; previously the accuracy criterion of the reading story) Given the Labelled Test Set, when an accuracy run is done, then lines are read correctly at least 95% of the time on digital PDFs and at least 90% on good-quality photos, each measured on at least 100 bills [Q15]. A line counts as read correctly when its amount, quantity and rate match exactly and its description matches after normalisation. "Good-quality photo" is a capture-quality label recorded in the test set.
- AC9.4.6 Given the Indian bill features (lakh and crore grouping, tables across pages, handwritten notes) and drug identification, when the accuracy run reports, then accuracy is reported for each feature and for drug lines, without a separate threshold.
- AC9.4.7 Given an accuracy run on real bills, when it starts, then it is refused unless every service it uses has a recorded data-processing agreement with a no-training term.

INVEST: lighter internal story; runs only on approved services.

### US9.5 Keep closed-beta bills with consent

**As** a closed-beta tester (a family member such as Priya, C1), **I want** to agree separately to my bill being kept for testing, **so that** I know my bill is used differently from a normal check.

- Priority: Must Have
- Covers: FR2.11
- Depends on: US1.4

- AC9.5.1 Given a beta session, when I upload, then a separate beta consent screen explains that originals are kept for testing, and upload is blocked until I agree [Q15].
- AC9.5.2 Given a beta bill, when it is kept, then it goes to a store separate from product data, and identifiers are blacked out before it enters the test set.
- AC9.5.3 Given the production configuration after launch, when it is checked, then the beta setting is off, and with it off every upload path deletes the original.

INVEST: small; the beta ends at launch.

---

## Group 10 — Ops and quality (internal)

### US10.1 Promote a check to live only with a recorded pass

**As** the builder or founder (I1), **I want** a check to go live only after it has passed its test-set gate, **so that** families only see checks that are accurate enough.

- Priority: Must Have
- Covers: FR15.1, FR16.12
- Depends on: US9.4, US8.3

- AC10.1.1 Given a check measured on at least 10 bills where its error is present and 30 where it is not, with precision of at least 90% for "Likely error" (or 75% for "Worth asking"), when I promote it in the ops view, then it goes live and the promotion record shows both counts and the precision. Precision is correct findings divided by findings shown, over the measured bills.
- AC10.1.2 Given exactly 10 and 30 bills and precision of exactly 90.0% for a "Likely error" check, when I promote it, then promotion is accepted.
- AC10.1.3 Given 9 bills where its error is present, or 29 where it is not, when I try to promote it, then promotion is refused and the reason names the shortfall.
- AC10.1.4 Given a "Likely error" check at 89% precision, or a "Worth asking" check at 74%, when I try to promote it, then promotion is refused.
- AC10.1.5 Given any route other than the ops view, when a check's state is changed to live, then the change is refused.
- AC10.1.6 (launch checklist) Given launch day, when some launch checks have not passed, then launch proceeds with those that have, and the others stay in shadow mode [Q14].

INVEST: full criteria [Q5].

### US10.2 Sign in to the internal tools with limited access

**As** the builder or founder (I1), **I want** the internal tools to require sign-in, give each role only what it needs, and log access to personal data, **so that** redacted real bills are protected.

- Priority: Must Have
- Covers: FR15.2, NFR7
- Depends on: none

- AC10.2.1 Given an internal route, when a request arrives without signing in, then it is refused.
- AC10.2.2 Given a signed-in user with the support role, when they try to open test-set bills, then access is refused (roles: ops, domain admin, support).
- AC10.2.3 Given any access to test-set data, when it happens, then a log entry records who, what and when.
- AC10.2.4 Given the strong sign-in definition set in NFR Requirements, when an internal user signs in with only part of it (for example a password alone), then sign-in is refused [Q11].
- AC10.2.5 (review) Given every store and connection, when configuration is reviewed, then data is encrypted at rest and TLS is used in transit.
- AC10.2.6 (launch checklist) Given launch is prepared, when the launch checklist is run, then a third-party security test report dated before launch exists, and every high or critical finding in it is fixed or accepted with a written reason.

INVEST: five to eight days; the access log can be built before the sign-in method is defined.

### US10.3 See product quality on a dashboard

**As** the builder or founder (I1), **I want** one dashboard of reading accuracy, precision by check, missed-issue rate and user ratings, **so that** I see where quality is slipping.

- Priority: Must Have
- Covers: FR15.4
- Depends on: US9.4, US6.4

- AC10.3.1 Given the latest test run and stored ratings, when the dashboard opens, then it shows reading accuracy, precision by check, missed-issue rate and user ratings.
- AC10.3.2 Given no test run has happened yet, when the dashboard opens, then it says so instead of showing zeros.

INVEST: lighter internal story.

### US10.4 Explain data practices in support material, with no case lookup

**As** the builder or founder (I1), **I want** support material that explains our data practices and no way to look up a user's case, **so that** support cannot reach anyone's bill.

- Priority: Must Have
- Covers: FR15.3
- Depends on: US10.2

- AC10.4.1 (review) Given the support material, when it is read, then it explains what is collected, how long it is kept and that cases cannot be looked up.
- AC10.4.2 Given every internal screen, when the route inventory is checked, then none can search for a user's case.

INVEST: lighter internal story.

---

## Group 11 — Won't Have (this time)

These stories record what this build deliberately leaves out [Q7], so the boundary is visible in the story list. They have no acceptance criteria. Each one names where the item goes.

| ID | Story | Why it is out | Where it goes |
|---|---|---|---|
| US11.1 | As the founder, I want identity-free abuse controls on the free tier, so that misuse stays low | "None at launch, add this as a requirement to Phase 1.5" (Requirements Analysis Q5); PRD risk RK26 stays open (AS3) | Phase 1.5 |
| US11.2 | As Priya (C1), I want to share a bill straight from WhatsApp into the check, so that I skip saving it first | Dropped for this build (Requirements Analysis Q7); US2.1 gives saving guidance instead | A later build |
| US11.3 | As Priya (C1), I want urgent-situation and pay-under-protest guidance, and my case to go first when I am being discharged, so that I am helped fastest under pressure | Moved at scope definition; the queue priority is dropped [Q8] (D-US1) | Phase 1.5 |
| US11.4 | As the founder, I want to know whether billing desks corrected bills, so that real-world impact is measured | Not handled in this build (Requirements Analysis Q13, Q15) | — |
| US11.5 | As the builder or founder (I1), I want a process that tracks IRDAI and NPPA changes and updates rules within a set window, so that rules stay current | Moved to Phase 2 [Q13], [Q16] (D-US6); values can still be corrected by hand (US8.1) | Phase 2 |
| US11.6 | As Meera (C2), I want an expert route and paid Expert Review, so that complex findings get professional help | Phase 1.5 (`intent-statement.md`) | Phase 1.5 |
| US11.7 | As Rohit (C3), I want the Could Have items (KB-06, IMP-04, OT-03, OT-04, OP-05) | Deferred (`scope-document.md` §3) | A later build |
| US11.8 | As any customer, I want accounts, notifications, Hindi and regional languages, insurance checks, a native app and user-visible price benchmarks | PRD Won't Have list W1–W22 | Later phases or never |

---

## Story dependencies

| Story | Depends on | Notes |
|---|---|---|
| US7.9 | — | Foundation: the anonymous session |
| US1.1 → US1.2 → US1.3 → US1.4 → US1.5 → US1.6, US1.7 | Each on the previous | The consent chain, shown on 2 screens [Q20] |
| US2.1, US2.2, US2.11, US9.5 | US1.4 | Upload needs processing consent |
| US2.3, US2.4, US2.8, US2.10 | US2.1 | US2.1 and US2.10 in full depend on storage (OQ2) |
| US3.8 | US2.4 | Reading needs a recognised document |
| US2.5, US2.7, US3.1, US3.6, US3.7, US3.10, US3.11, US3.12, US4.8 | US3.8 | Need extracted text |
| US2.6 | US3.10 | Legibility needs photo reading |
| US2.9 | US2.4, US2.11 | |
| US3.9 | US7.9 | Background processing; depends on hosting (OQ2) |
| US3.3 | US3.1, US3.9 | |
| US8.4 | — | Foundation: launch reference data |
| US8.3 | US3.12 | The check runner |
| US4.1–US4.7 | build: US3.12, US8.3 (and US8.4 for prices); release: US10.1 | Testable on structured fixtures before promotion |
| US4.9 → US4.10 → US4.11 | Each on the previous | |
| US5.2 | US4.11, US4.14, US3.9 | The report brings the findings together |
| US5.4 → US5.11 | | Shared on-device PDF builder, then the phone share path |
| US5.3 | US5.2, US6.1 | Built from "Will ask" or the Top 3 |
| US7.1 | US5.4, US7.9 | |
| US7.7 | US7.2 | |
| US8.1, US8.5, US9.1, US10.4 | US10.2 | Internal tools need sign-in |
| US9.1 → US9.2 → US9.4 → US10.1 | | Promotion needs a measurement on real bills |

**Critical path to a first useful report:** US7.9 → US1.4 → US2.1 → US2.4 → US3.8 → US3.12 → US8.3 → US4.1 → US4.9 → US4.11 → US5.2 → US5.4.

**Critical path to launch:** the hosting and residency decisions (OQ1, OQ2) gate chunked upload (US2.10), background processing (US3.9), real bills in the test set (US9.1), and so every promotion (US10.1). They are on the critical path to launch, not only to deployment.

## INVEST summary

- **Independent.** Stories inside each consent, upload and check family can be built and tested on their own. Check families are tested on structured fixtures in a fixed state, so they do not wait for promotion. The dependencies above are the real ones.
- **Negotiable.** Criteria state outcomes, not screens or technology, except where a privacy or caching guarantee needs a named mechanism (US7.6).
- **Valuable.** Every Must Have and Should Have story names a persona and a benefit. Group 11 records value deliberately deferred.
- **Estimable and small.** Most stories fit one to five days. US7.2 and US10.2 are five to eight days and may be split in Units Generation. The original reading story was split into US3.8, US3.10, US3.11 and US3.12, and the report download into US5.4 and US5.11.
- **Testable.** Every criterion is Given/When/Then with a pass/fail outcome on synthetic bills, a named measurement, or a recorded sign-off. The quality engineer's per-criterion verification map (Jest, Playwright, static check, offline run, timing test, manual) is in `contributions/aidlc-quality-agent.md` for Build and Test.

## Assumptions & Open Questions

- The clinically sensitive category list for the "Complex" label (US4.13) is drafted in Domain Design (requirements OQ3).
- The "strong sign-in" definition (US10.2) is set in NFR Requirements [Q11].
- Data residency and hosting (requirements OQ1, OQ2) must be decided before any real bill reaches an environment. They also block US2.10, US3.9, US7.7 and the test-set chain.
- The founder or counsel check of the privacy wording (US1.3, requirements OQ6) is needed before launch.
- Set in Functional Design: the calculation rounding tolerance (AC4.1.9), the hospital day convention (AC4.3.4), the Top 3 ranking formula and tie-break (AC5.1.2), and the number of password attempts (AC2.8.3).
- Set in NFR Requirements: the exact length of "3 months" and the deletion-run tolerance (US7.7), and the time limit for building the PDF on the test device (AC5.4.9).
- Named in Domain Design: the data source for the items a package normally includes (US4.7).
- Set in Build and Test: the number of timed runs behind each 90th-percentile figure (AC3.3.2, AC3.3.3, AC5.2.4).
- Refined Mockups should bring the approved wireframes in line with these stories: WCAG 2.2 instead of 2.1, no "anonymous copy", the fixed labels above, and the "When are you checking?" question on the upload screen.
