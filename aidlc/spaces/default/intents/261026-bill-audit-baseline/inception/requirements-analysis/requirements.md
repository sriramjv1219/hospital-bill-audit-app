# Requirements — Hospital Bill Check (Project RS, Phase 1)

This document turns the Phase 1 scope into testable requirements. Each functional requirement has a stable `FR{n}` / `FR{n}.{m}` ID, and each non-functional requirement has a stable `NFR{n}` ID. Later stages keep these IDs as they are. Every requirement names the PRD requirement it comes from, so the PRD's own IDs (ON-, SD-, DU-, DP-, AA-, RE-, RP-, AD-, FB-, TS-, KB-, OP-, LT-, NFR-) can still be traced.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- [Q1]–[Q16]: answers in `requirements-analysis-questions.md` in this folder, confirmed through the consolidated summary.
- PRD: `PRDs/app-baseline.md` v0.14 (PRD-v2), read through the document library (`aidlc/spaces/default/knowledge/documents/app-baseline.md`, document id 01a17987): §8.4 metric targets, §12 functional requirements, §13 audit framework and check catalogue, §14 non-functional requirements, §15 MoSCoW, §17 constraints. The extracted text ends partway through decision D30; the unread end was accepted at intent capture (`intent-statement.md`).
- Intent: `ideation/intent-capture/intent-statement.md` (problem, customers, success metrics, product boundary).
- Scope: `ideation/scope-definition/scope-document.md` (Must Have and Should Have lists, the 18 launch checks, PRD gaps resolved there).
- Constraints: `ideation/feasibility/constraint-register.md` (CR-R, CR-D, CR-T, CR-O, CR-M).
- Agreed rough-mockup changes R-01 to R-08: `ideation/approval-handoff/initiative-brief.md`.
- Code knowledge base: `aidlc/spaces/default/codekb/hospital-bill-audit-app/business-overview.md` (the app is an unmodified Create Next App scaffold with no business features), `architecture.md` (Cache Components and partial prefetching are on, telemetry and `agentFeedback` need a decision), `code-structure.md` (single `app/` directory, no tests).
- Team practices: `inception/practices-discovery/team-practices.md` (testing posture, data handling during development, deployment gates).

## 1. Intent analysis

**What families need.** A family holding an Indian hospital bill wants to know which charges are questionable and to raise them politely with the billing desk, with evidence, before or after paying (`intent-statement.md`). The bill may be unreadable or only a summary, there is little time at discharge, and the family may be under emotional stress or abroad.

**What this build delivers.** The Phase 1 Hospital Bill Check: a free, anonymous, single-session web check (`scope-document.md` §1). It reads the bill line by line, runs objective checks that have passed testing, explains each finding with evidence and says whom to ask. It ends with a downloadable Audit Report, a one-page Billing Desk Summary and ready-to-edit letters. The internal tools it needs come with it in their leanest form: the rule library, the Labelled Test Set and the ops views.

**How success is judged.** By the PRD Phase 1 metric tree (`intent-statement.md`, Success Metrics). The targets that bind the build are carried here as NFR4 (accuracy), NFR1 and NFR2 (speed), and the zero-tolerance guardrails in FR13 and NFR8. Whether billing desks later correct bills is not measured in this build [Q13].

**Where the codebase starts.** The repository holds one static template page and no product code (`business-overview.md`, `code-structure.md`). Every requirement below is new work on the existing Next.js 16 / React 19 / TypeScript stack (CR-T9).

**Depth.** Comprehensive: the scope is large, the domain is regulated, and the data is health data.

### Resolutions made in this stage

| Item | Resolution | Source |
|---|---|---|
| G-10 session timeout | 30 minutes idle, warning and "Keep going" at 25 minutes | [Q1] |
| Q-P2 "Complex" threshold | Clinically sensitive categories only at launch; no value threshold yet; the category list is drafted in Domain Design | [Q2], [Q16] |
| §13.1.2 fold-away threshold | Every low-confidence finding is folded away, whatever the amount | [Q3] |
| G-16 minimum test-set size | At least 10 bills with the error and 30 without, per check | [Q4] |
| G-09 abuse controls / S9 | None in this build; identity-free abuse controls become a Phase 1.5 requirement | [Q5] |
| G-17 beta after launch | The closed beta ends at launch; later top-ups come from the personal network only | [Q6] |
| S1 share-in (DU-04) | Dropped from this build; upload guidance covers saving a file from WhatsApp first | [Q7] |
| NFR-13 accessibility standard | WCAG 2.2 level AA | [Q8] |
| NFR-12 device and network | An 8 GB RAM Android phone on typical 4G is the only target; upload resume and a script limit per page are kept | [Q9], [Q14] |
| Upload limits | 20 files, 25 MB per file, 60 pages per session | [Q10] |
| G-19 unnumbered recommendations | Report watermark, public data charter and replaceable bill reader are decisions only, not requirements | [Q11] |
| G-18 "Who to ask" routes | User route shown as "You can sort this out"; Expert route not shown in Phase 1 | [Q12] |
| G-02 bill-correction measure | Not handled in this build; FB-02 keeps only "what do you plan to do" | [Q13], [Q15] |

## 2. Functional requirements

Priority uses the PRD's MoSCoW codes: M (Must Have), S (Should Have). Every requirement in this section is in this build.

### FR1 Anonymous entry, privacy notice and consent (PRD M1)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR1.1 | The system shall let a user start and finish a check without registering, signing in, or giving a mobile number, email or identity proof, and shall go straight from consent to upload with no sign-up step. | ON-01, ON-02 | M | No screen in the user journey asks for a name, phone number, email, password or ID; consent leads directly to upload. |
| FR1.2 | Before any upload, the system shall show a layered privacy notice in plain language covering what is collected, why, retention, sharing and the user's rights. | ON-03 | M | Upload controls are unreachable until the notice has been shown; the notice contains all five topics. |
| FR1.3 | The system shall ask for explicit consent to process the documents for the audit before any upload. | ON-03(a) | M | Upload is blocked until the user gives processing consent. |
| FR1.4 | The system shall ask for the separate, optional consent to store de-identified structured data only after the report has been shown. | ON-03(c), SD-04 | M | The storage consent appears only after the report; declining it does not change the report or block the download. |
| FR1.5 | The system shall require the user to declare their relationship to the patient (self, spouse, parent, child, other family, authorised representative, professional) and to confirm they are authorised to share the patient's data. | ON-04 | M | Upload is blocked until a relationship is chosen and the authorisation box is confirmed. |
| FR1.6 | When the patient is a minor, the system shall require the uploader to declare that they are the parent or lawful guardian, and shall proceed on that declaration. | ON-05 | M | Choosing "patient is under 18" shows the guardian declaration; the check cannot continue without it. |
| FR1.7 | When the patient is an adult other than the user, the system shall require the user to declare that they have the patient's consent, or authority where the patient is incapacitated or deceased. | ON-06 | M | Choosing an adult third-party patient shows the declaration; the check cannot continue without it. |
| FR1.8 | The system shall let the user withdraw consent and discard all session data at any point in the session. | ON-07 | M | After "Discard everything", the session's files and extracted data no longer exist in storage, and the user is returned to the start. |
| FR1.9 | The system shall show an onboarding explainer of what the product does and does not do (no guarantee, no legal or medical advice). | ON-08 | M | The explainer appears before consent and contains the three statements. |
| FR1.10 | All screens, reports and templates shall be in English only. | ON-09 | M | No language switcher exists; all user-facing text is English. |
| FR1.11 | When the user declines processing consent, the system shall explain that the check cannot run without it, upload nothing, and offer to go back or leave. | R-01 to R-08 list, R-06 | M | Declining consent shows the declined-consent state, and no file is accepted. |
| FR1.12 | The privacy wording on the start screen ("Nothing about you is saved") and on "before you go" ("anonymous") shall be checked by the founder or counsel before launch, because optional 3-month storage keeps de-identified, not anonymous, data. | R-02 | M | A recorded founder or counsel approval of both texts exists before launch. |

### FR2 Session and data lifecycle (PRD M17)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR2.1 | The system shall complete upload, review and download in one session, and a case shall not be reopened after its session ends. | SD-01, CM-03 | M | After the session ends, no route, link or back-navigation shows that case again. |
| FR2.2 | The system shall not collect or store any user profile data: no name, mobile number, email, login or device-linked profile. | SD-02 | M | A data inventory of every store shows none of these fields and no persistent device identifier. |
| FR2.3 | The system shall delete each original uploaded file once its content is extracted, and in any case when the session ends, including when extraction fails. No copy shall remain in temporary folders, logs or error reports. | SD-03 | M | Automated tests on synthetic bills show the original is gone after extraction, after a failed extraction and at session end, and that temp folders, logs and error reports hold no file content (`team-practices.md`, Testing Posture). |
| FR2.4 | When storage consent is not given, nothing from the session shall be kept after the session ends. | SD-06 | M | After a session without storage consent, no record linked to that session exists in any store. |
| FR2.5 | When storage consent is given, the system shall store only structured data: line items, categories, quantities, rates, amounts, dates, hospital, city, findings and the user's ratings. | SD-04 | M | The stored record contains only these fields. |
| FR2.6 | Before storing, the system shall remove the patient's name, UHID/IP number, phone, address and policy number, and keep age band, gender, hospital, city, admission month, length of stay and all line items. | SD-05 | M | Stored records from synthetic bills that contain all five identifiers hold none of them; age is stored as a 10-year band. |
| FR2.7 | Before the session ends, the system shall prompt the user to download the report and state that it cannot be retrieved later. | SD-07 | M | Leaving or ending the session without a download shows the prompt first. |
| FR2.8 | A session idle for 30 minutes shall end and its in-progress data shall be discarded. At 25 minutes idle the system shall warn the user and offer a "Keep going" button that resets the idle timer. | SD-08 | M | At 25:00 idle the warning appears; "Keep going" resets the timer; at 30:00 idle the session ends and its data is gone [Q1]. |
| FR2.9 | Stored structured data shall be deleted automatically 3 months after its session. | SD-10 | M | No stored record is older than 3 months; a daily check finds zero such records (guardrail in `intent-statement.md`). |
| FR2.10 | A user's bill data and findings shall never be placed in a shared cache, a prebuilt page or prefetched output. | — (`architecture.md`; `team-practices.md`, Code Style) | M | Tests show pages with bill data are rendered per request and are not served to a second session; no shared cache entry holds bill data. |
| FR2.11 | The only exception to FR2.3 is the closed pre-launch beta, where testers explicitly agree that their bills are kept for testing. The beta ends at launch. | SD-03, LT-04 | M | After launch no upload path keeps an original file [Q6]. |

### FR3 Case (PRD M2)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR3.1 | Each session shall create one case for one hospitalisation, recording hospital name and city, admission and discharge dates, treatment type (planned or emergency), payment mode (cashless, reimbursement, self-pay) and insurer or TPA if any. The patient is not named in the case. | CM-01 | M | The case holds these fields and no patient name. |
| FR3.2 | The system shall infer the case details from the uploaded documents and ask the user to confirm or correct them. | CM-02 | M | On a synthetic bill, the fields are pre-filled from the document and editable; no field requires typing when the document contains it. |
| FR3.3 | The session shall show progress (upload, review, report) and the next recommended step. | CM-03 | M | Every user screen shows the current step and a next-step action. |

### FR4 Document upload (PRD M3, M4, S5)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR4.1 | The system shall accept hospital bills (final, interim, summary, itemised), pharmacy bills, consumable and implant invoices and stickers, cost estimate or package letter, discharge summary, investigation reports and payment receipts. | DU-01 | M | Each document type can be uploaded and is recognised. |
| FR4.2 | The system shall accept digital and scanned PDFs, phone photos (single and multi-page) and screenshots, with several files per document. | DU-02 | M | Each input format uploads successfully. |
| FR4.3 | The system shall let users upload from the phone camera, with capture guidance (flat page, no glare, no cut-off edges), and from files. The upload screen shall explain how to save a bill received on WhatsApp to the phone first. | DU-03 | M | Camera capture shows the three guidance points; the WhatsApp saving guidance is on the upload screen [Q7]. |
| FR4.4 | A session shall accept at most 20 files, 25 MB per file and 60 pages in total. An upload over a limit shall be refused with a message naming the limit, and files already accepted shall stay. | — | M | The 21st file, a 26 MB file or a 61st page is refused with the named limit; earlier files remain [Q10]. |
| FR4.5 | The system shall classify each file by document type and let the user correct the type. A file that is not a bill or hospital document shall be flagged as such, with a way to remove it or keep it. | DU-05, R-06 | M | Synthetic files of each type are classified; a non-bill image triggers the "not a bill" state. |
| FR4.6 | The system shall detect page order, duplicate pages and missing pages (for example "Page 3 of 7 missing", or page totals that do not add up to the grand total) and prompt the user. | DU-06 | M | Synthetic bills with a missing page, a duplicate page and shuffled pages each trigger the matching prompt. |
| FR4.7 | The system shall check each page's legibility and ask for a re-upload of unreadable pages, naming the page. | DU-07 | M | A blurred synthetic page triggers a re-upload request that names that page. |
| FR4.8 | The system shall detect a summary bill and explain how to request the itemised bill, pharmacy breakup and implant invoices, with a request template. | DU-08, RE-06(a) | M | A synthetic summary bill triggers the explanation and the itemised-bill request template. |
| FR4.9 | The system shall unlock a password-protected PDF with a password the user types in, and shall not store the password. | DU-09 | S | A protected synthetic PDF opens with its password; the password appears in no store or log. |
| FR4.10 | The system shall show a per-case document checklist of what is present, missing or optional. For interim bills: during the stay, only the current interim bill is checked and nothing carries over; at discharge, the checklist asks for earlier interim bills together with the final bill, so carry-forward checks have both bills. | DU-10, R-04, G-20 | M | The checklist updates as documents arrive; at discharge it lists earlier interim bills. |
| FR4.11 | Users shall be able to add documents at any point in the session, including after the report appears ("Add a document"), and the report shall refresh. Documents cannot be added after the session ends. | DU-11, R-01 | M | Adding a discharge summary after the report produces an updated report in the same session. |

### FR5 Bill reading (PRD M5)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR5.1 | The system shall extract header data: patient name, age and gender, UHID/IP number, hospital name, address and registration, GSTIN, admission and discharge date and time, ward or room category, treating doctors, bill number and date, payer and policy or claim number. | DP-01 | M | Measured against the Labelled Test Set under NFR4. |
| FR5.2 | The system shall extract every line item with date of service, description, hospital code, quantity, unit, unit rate, gross amount, discount, tax, net amount and the bill section it appears under. | DP-02 | M | Measured under NFR4. |
| FR5.3 | The system shall extract bill-level totals: section subtotals, gross total, discounts, taxes, advances and deposits, insurer-approved amount, patient payable and amount paid. | DP-03 | M | Measured under NFR4. |
| FR5.4 | The system shall extract from the discharge summary: diagnosis, procedures performed, procedure dates, ICU and ward stay periods, implants used, condition at discharge and discharge medicines. | DP-04 | M | Measured under NFR4. |
| FR5.5 | The system shall handle Indian bill features: mixed fonts, tables across pages, merged cells, handwritten notes, stamps, lakh and crore amounts with Indian digit grouping, and common abbreviations. | DP-07 | M | The Labelled Test Set contains each feature, and NFR4 is met on it. |
| FR5.6 | Lines partly in Hindi or a regional script shall be listed as "could not be read" and excluded from all checks. | DP-08 | M | Synthetic lines in Devanagari or another Indian script appear in the unread list and in no finding. |
| FR5.7 | Every extracted field shall carry a confidence level, and low-confidence fields shall be highlighted for the user to confirm. | DP-09 | M | Every field has a confidence value; low-confidence fields are visibly marked. |
| FR5.8 | Users shall be able to view and correct extracted line items, including after the report appears ("Fix a line"). Corrections shall be recorded and the audit re-run. | DP-10, R-01 | M | Correcting a line amount changes the affected findings and the report in the same session. |

### FR6 Normalisation and categorisation (PRD M6)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR6.1 | The system shall map each line description to a standard catalogue entry while keeping the original text. | DP-11 | M | Each line shows both the original and the standard description. |
| FR6.2 | The system shall identify drugs by generic name, strength, dosage form and, where possible, brand. | DP-12 | M | Measured on the drug lines of the Labelled Test Set. |
| FR6.3 | The system shall identify consumables and disposables and map them to IRDAI Lists II–IV, used only as the unbundling reference. | DP-13, G-22 | M | Mapped consumables cite a List II, III or IV entry; no List I mapping exists. |
| FR6.4 | The system shall identify implants and devices and capture make, model and batch where present. | DP-14 | S | Implant lines on synthetic bills carry make, model and batch when printed. |
| FR6.5 | The system shall identify professional fees by type (surgeon, assistant surgeon, anaesthetist, consultant visit, specialist referral) and by doctor. | DP-15 | M | Fee lines on synthetic bills carry type and doctor. |
| FR6.6 | The system shall identify packages and their stated inclusions and exclusions, where the bill shows them or the user provides them. | DP-16 | M | A synthetic package bill yields its inclusion and exclusion lists. |
| FR6.7 | The system shall assign each line to one of the PRD's 21 standard categories (Room & Boarding through Other). | DP-17 | M | Every line has exactly one category from the list. |
| FR6.8 | The system shall tag each line with whether it is commonly considered part of room, procedure or treatment charges, using IRDAI Lists II–IV as an industry reference. | DP-18 | M | Lines matching a List II–IV entry carry the tag. |

### FR7 Validation and completeness (PRD M7)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR7.1 | The system shall check that patient name, UHID, dates and hospital match across bill pages, pharmacy bills and the discharge summary, and flag mismatches. | DP-19 | M | A synthetic set with a mismatched UHID triggers the flag. |
| FR7.2 | The system shall check internal arithmetic before the audit and separate extraction errors from billing errors. | DP-20 | M | A misread digit is routed to user confirmation, not reported as a billing error. |
| FR7.3 | The report shall state which checks could not run and why (missing document or data). If processing fails partway, the system shall show what completed, which checks did not run, and offer to retry. | DP-21, R-06 | M | Removing the discharge summary lists the checks that need it; a forced failure mid-run shows the partial state with a retry. |
| FR7.4 | When extraction confidence is low, the user shall be asked to confirm or correct the uncertain fields, and checks that depend on unconfirmed fields shall not be shown. | DP-22 | M | Leaving a low-confidence field unconfirmed hides the findings that depend on it. |
| FR7.5 | The quick summary shown before confirmation shall be labelled "First look — may change after you confirm a few details", and its button shall read "Confirm a few details". | R-03 | M | The label and button text appear on the quick summary. |

### FR8 Checks and findings (PRD M8, M9, M10, S3)

The check catalogue is PRD §13.6. A check is shown to users only when it is live (FR14.4, FR16.12).

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR8.1 | The system shall run every live check that applies to the case and its documents. | AA-01 | M | For each synthetic bill, the set of checks run equals the live checks whose inputs are present. |
| FR8.2 | At launch, the 18 launch checks shall be live: CAL-01, CAL-02, CAL-03, CAL-04, CAL-06, CAL-07, DUP-01, DUP-06, DUP-07, LOS-01, LOS-03, RM-02, RX-01, RX-02, RX-03, CON-04, IMP-02, PKG-01. Each goes live only after it passes FR16.12. | M8, §13.6 | M | At launch, these 18 are live and each has a recorded pass under FR16.12. |
| FR8.3 | The 8 document checks DOC-01 to DOC-08 shall run from launch as document guidance. | M8, §13.8 | M | Each DOC check produces guidance on a synthetic bill missing the matching document. |
| FR8.4 | The Wave 2 and Wave 3 checks shall go live one at a time after launch, each only once it passes FR16.12. | S3, KB-04 | S | No Wave 2 or 3 check is live without a recorded pass. |
| FR8.5 | The system shall find duplicate charges: same item, date and amount; the same investigation repeated without a matching clinical note; the same professional visit billed twice. | AA-02 | M | Planted duplicates in synthetic bills are found. |
| FR8.6 | The system shall find unusual charges inconsistent with the diagnosis, procedure or patient profile. This capability is built for launch; its checks are shown only once they pass FR16.12. | AA-03, G-04 | M | The capability runs in shadow mode on synthetic bills; no finding is shown until its check is live. |
| FR8.7 | The system shall find consumable quantities far above typical per day of stay or per procedure. Built for launch; shown only once its checks pass FR16.12. The typical-quantity source is a public source if one exists, otherwise ranges derived from the Labelled Test Set. | AA-04, G-03, G-04 | M | As FR8.6. |
| FR8.8 | The system shall find potential package violations: items billed separately that the package normally includes; stay beyond package days without explanation; package and itemised billing both present. | AA-05 | M | Planted package violations are found. |
| FR8.9 | The system shall find billing anomalies: arithmetic errors, dates outside the stay, charges after discharge, different rates for the same item, round-figure miscellaneous charges and unexplained "others". | AA-06 | M | Each planted anomaly is found. |
| FR8.10 | The system shall find missing billing details: no itemisation, no dates, no quantities, no batch or MRP for drugs, no implant invoice or sticker, no doctor names for professional fees. | AA-08 | M | Each planted omission is found. |
| FR8.11 | The system shall compare prices with the printed MRP and with NPPA ceiling prices and device caps. Government schedule rates (CGHS, PM-JAY) may appear only as context, never as a finding. Internal benchmarks are not shown to users in this build. | AA-11, W-list | M | Planted above-MRP and above-cap prices are found; no finding cites CGHS or an internal benchmark. |
| FR8.12 | Each finding shall include: check ID, category, finding class, confidence (High, Medium, Low), rupee amount potentially affected, evidence (bill line and page, regulatory reference), "Who to ask" route and a plain-language explanation. | AA-12 | M | Every finding in the output has all eight fields. |
| FR8.13 | Findings of the same kind shall be grouped (for example 12 glove lines become one finding with 12 evidence lines). | AA-13 | M | A bill with 12 duplicate glove lines yields one finding with 12 evidence lines. |
| FR8.14 | Totals shall never count the same rupee twice across findings; the report shows a reconciled "potentially affected" amount. | AA-14 | M | Where two findings cover the same line, the reconciled total counts that line's amount once. |
| FR8.15 | The system shall produce a Bill Breakdown splitting the bill total into: likely errors to correct; charges worth asking about; explained, with no billing concern; could not be assessed. | AA-15 | M | The four parts sum to the bill total. |
| FR8.16 | Findings in clinically sensitive categories shall be marked "Complex — consider professional advice". No value threshold applies at launch; the system shall allow a value threshold to be set later without a release. The category list is drafted in Domain Design and confirmed there. | AA-16, Q-P2 | M | A finding in a listed category carries the label; setting a threshold in the rule library changes labelling without a deployment [Q2], [Q16]. |
| FR8.17 | Each report shall record the rule-library version and reference-data versions it used. | AA-17 | M | The report shows both versions. |
| FR8.18 | Findings shall use the Phase 1 classes "Likely error", "Worth asking" and "Explained", with confidence High, Medium or Low as defined in PRD §13.1.1 and §13.1.2. | §13.1.1, §13.1.2 | M | Every finding has one class and one confidence level from these sets. |
| FR8.19 | Every low-confidence finding shall be shown collapsed under "Other things you may ask about", whatever its amount. | §13.1.2 | M | No low-confidence finding appears outside the collapsed group [Q3]. |
| FR8.20 | The "Who to ask" routes shown in Phase 1 shall be "Ask the hospital", "Explained — no billing concern", "Complex — consider professional advice" and "You can sort this out" (for issues only the family can resolve, such as supplying a missing document). The Expert route shall not be shown. | §13.1.3, G-18 | M | Only these four labels appear in any output [Q12]. |

### FR9 Recommendations and letters (PRD M11, M12, S2, S6)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR9.1 | For each finding, the system shall give an Explanation (what the item is), an Observation (what was noticed, with evidence), a Potential Concern (why it may be wrong and how sure we are) and Follow-up Actions (what to ask, whom, what evidence to attach and by when). | RE-01 to RE-04 | M | Every finding shows all four parts. |
| FR9.2 | Whom to ask in Phase 1 shall be the hospital billing desk, patient relations or the medical superintendent. | RE-04 | M | No follow-up names an insurer, TPA or HR. |
| FR9.3 | The system shall rank actions by expected value, likelihood of success and effort, and show a "Top 3 things to do now" list. | RE-05, §13.1.4 | M | The Top 3 list appears and its order follows the ranking. |
| FR9.4 | The system shall generate editable letters: itemised bill request; billing query list; request for implant invoice or sticker and package inclusion list; request for a revised bill, written explanation, or refund or credit note. | RE-06(a–d) | M | Each letter is generated from a synthetic case and can be edited before download. |
| FR9.5 | The system shall generate a hospital grievance escalation letter and an NPPA complaint pointer for MRP or ceiling-price breaches. | RE-06(e–f) | S | Both are generated when the matching finding exists. |
| FR9.6 | Letters shall use a respectful, factual tone, cite evidence and references, and never allege fraud. | RE-07 | M | Language guardrail tests (FR13.2) pass on every letter. |
| FR9.7 | The report shall show the hospital escalation ladder (billing desk, patient relations, medical superintendent or grievance officer, State authority where applicable, NPPA for drug and device prices, Consumer Commission) with the relevant deadlines from the hospital-side timeline reference data. | RE-08, G-11 | M | The ladder and its deadlines appear in every report. |
| FR9.8 | Where findings are not enough to act on, the system shall say so and name the document that would resolve the uncertainty. | RE-09 | M | A bill without an implant invoice yields a note naming the invoice. |
| FR9.9 | The system shall give informational "What to do next time" tips. | RE-10 | S | The tips section appears in the report. |

### FR10 Report and Billing Desk Summary (PRD M13)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR10.1 | The system shall generate an Audit Report with seven parts: summary; Bill Breakdown; findings by category with evidence; line-item appendix with explanations; checks performed and not possible, with reasons; next steps and escalation ladder; disclaimers, the statement "this checks hospital billing only, not insurance coverage" and the rule-version stamp. | RP-01 | M | A generated report contains all seven parts. |
| FR10.2 | The system shall generate a one-page Billing Desk Summary in a neutral tone with specific line references and no internal confidence jargon. | RP-02 | M | The summary fits one A4 page and contains no confidence labels. |
| FR10.3 | The user shall be able to download the report as a PDF during the session. On phones, "Save or share" shall create the PDF or image on the device and open the phone's own share menu, with nothing sent through our servers; desktop shall offer a download. | RP-03, R-05 | M | The PDF downloads on desktop; on a phone the system share sheet opens; no server request carries the file. |
| FR10.4 | Reports shall use plain language, with inline explanations of medical and insurance terms. | RP-04 | M | Every medical or insurance term from the glossary list has an inline explanation (the reading-level measure is an open question). |
| FR10.5 | The report shall regenerate within the session when documents or corrections are added. | RP-06, R-01 | M | See FR4.11 and FR5.8. |
| FR10.6 | The report shall show a visible statement of limitations: based on the documents provided; not legal or medical advice; no guarantee; insurance coverage not assessed. | RP-08 | M | The statement appears on the first page of the report. |

### FR11 Action support (PRD M14, S4)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR11.1 | Users shall be able to mark each finding "Will ask" or "Not pursuing", and the "Will ask" findings shall form a checklist in the downloaded report. | AD-01 | M | Marked findings appear in the report checklist. |
| FR11.2 | The downloaded report shall be able to include a dispute pack: findings, evidence index, letters and escalation ladder. | AD-05 | S | Choosing the dispute pack adds the four parts. |
| FR11.3 | The system shall never send any communication to a hospital or insurer on the user's behalf. | AD-06 | M | No outbound email, SMS or API call to third parties exists in the system. |

### FR12 Feedback and outcome capture (PRD M18, S8)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR12.1 | Users shall be able to rate each finding helpful, not helpful or wrong, with an optional reason. | FB-01 | M | Each finding has the three ratings and a reason field. |
| FR12.2 | Within the session, the system shall ask what the user plans to do next. It shall not ask whether an already-paid bill was revised. | FB-02 | M | The outcome question asks only about plans [Q13], [Q15]. |
| FR12.3 | With storage consent, stored structured data and in-session feedback shall be aggregated by hospital, city, procedure and finding type for internal quality work. | FB-04 | M | Aggregates contain only consented sessions. |
| FR12.4 | Insured users shall be able to click "I want the Insurance Check", which increments an anonymous count and collects no contact details. | S8, G-06 | S | Clicking stores one count increment and no other data. |

### FR13 Trust and safety (PRD M21)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR13.1 | Disclaimers shall appear at onboarding, on reports and on letters: informational only; not legal, medical or financial advice; no guaranteed outcome; the user is responsible for their communications. | TS-01 | M | All three surfaces show the disclaimer. |
| FR13.2 | Findings, reports and letters shall never use "fraud", "scam", "cheating" or "illegal", shall use "appears", "may", "we could not find" or "worth asking" for non-objective findings, and shall reference the specific bill line and page. | TS-02, §13.1.5 | M | An automated wording test over all templates and a generated corpus finds zero banned terms; every finding cites line and page. Guardrail target: zero accusatory findings (`intent-statement.md`). |
| FR13.3 | No screen, report or letter shall encourage withholding payment in a way that blocks discharge or treatment. | C10, CR-R5 | M | A wording review of every template and fixed text finds no such encouragement. |
| FR13.4 | No advertisement or referral fee from a hospital, insurer or lender shall appear. | TS-04 | M | No ad slot or referral link exists. |

### FR14 Rule library (internal; PRD M19)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR14.1 | Rule owners shall be able to maintain the check catalogue and reference lists (IRDAI Lists II–IV, NPPA ceiling prices, device caps, hospital-side grievance timelines) without an engineering release. | KB-01 | M | Changing a reference price in the library changes the next report without a deployment. |
| FR14.2 | Every rule and reference item shall have a source, effective dates, version, owner and review date. | KB-02 | M | No rule or item can be saved with any of the five missing. |
| FR14.3 | A defined process shall monitor regulatory change (IRDAI circulars, NPPA notifications) and update rules within a target window. | KB-03 | M | The process document exists and names the window (open question). |
| FR14.4 | Each check shall be switchable between live, shadow (runs but is not shown) and off. | KB-04 | M | Each state behaves as named on a synthetic bill. |
| FR14.5 | A standard catalogue for drugs, consumables, procedures and investigations shall be maintained and expanded from cases. | KB-05 | M | Catalogue entries can be added and versioned. |
| FR14.6 | Each owner role shall map to a named person in Phase 1: the builder or the founder. | G-13 | M | Every rule's owner field names one of the two. |

### FR15 Operations and admin (internal; PRD M20)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR15.1 | The system shall provide ops views for quality measurement against the Labelled Test Set and for promoting a check to live. | OP-01 | M | A check can be promoted only from the ops view, and only with a recorded FR16.12 pass. |
| FR15.2 | Internal tools shall require their own strong sign-in, give role-based, least-privilege access (ops, domain admin, support) and log every access to personal data. | OP-02, CR-T11 | M | Unauthenticated requests to internal routes are refused; each access to test-set data creates a log entry. |
| FR15.3 | Support material shall explain data practices. There is no case lookup, because cases are not linked to users. | OP-03 | M | No internal screen can search for a user's case. |
| FR15.4 | A quality dashboard shall show extraction accuracy, finding precision by check, missed-issue rate and user ratings. | OP-04 | M | The dashboard shows all four for the latest test run. |

### FR16 Labelled Test Set (internal; PRD M15)

| ID | Requirement | PRD | Pri | Pass/fail test |
|---|---|---|---|---|
| FR16.1 | The test set shall target 300–500 bills, mixing hospital sizes, States, procedure types, bill formats (itemised, summary, interim, final) and capture quality (PDF, photo), with discharge summaries where available. | LT-01 | M | A coverage report shows the count and the mix. |
| FR16.2 | Bills shall come from the personal network with written consent, from synthetic bills with planted errors and a recorded answer key, and from the closed pre-launch beta with separate consent. The beta ends at launch. | LT-02, LT-03, LT-04 | M | Each bill records its source and consent [Q6]. |
| FR16.3 | Bills from public posts shall not be used without the patient's consent. | LT-05 | M | No bill has a public-post source without consent. |
| FR16.4 | On every real bill, the patient's name, UHID/IP number, phone, address and policy number shall be blacked out on receipt. | LT-06 | M | A spot check of real bills finds no visible identifier. |
| FR16.5 | Two people (the builder and the founder) shall label each real bill independently, reconcile differences against public sources and record the reason. | LT-07, CR-O4 | M | Each real bill has two labels and a reconciliation record. |
| FR16.6 | Synthetic bills shall use their planted answer key. | LT-08 | M | Each synthetic bill links to its answer key. |
| FR16.7 | Labels shall never be produced by, or pre-filled from, the AI being tested. | LT-09 | M | The labelling tool has no AI pre-fill. |
| FR16.8 | Redacted originals and labels shall be kept for the life of the test set, separately from the 3-month product retention rule. | LT-10 | M | Test-set data sits in a store separate from product data. |
| FR16.9 | Contributors shall be able to withdraw their bills at any time; withdrawn bills are deleted from the set and from future runs. | LT-11 | M | A withdrawn bill is absent from the next run. |
| FR16.10 | Coverage shall be reported by State, hospital type, procedure, format and capture quality. | LT-12 | S | The coverage report shows all five breakdowns. |
| FR16.11 | The set shall be topped up periodically with newly consented bills from the personal network. | LT-13 | S | Top-up bills record personal-network consent [Q6]. |
| FR16.12 | A check may go live only when it has been measured on at least 10 test bills where its error is present and 30 where it is not, and meets its precision target (90% for "Likely error", 75% for "Worth asking"). | G-16, NFR-05 | M | The promotion record shows both counts and the measured precision [Q4]. |
| FR16.13 | Real bills in the test set shall be kept outside git and out of reach of the AI coding assistant, and accuracy runs shall use only services that have passed the data-processor check. | — (`team-practices.md`) | M | No real bill exists in the repository or any folder the assistant can open. |

## 3. Non-functional requirements

| ID | Attribute | Requirement | PRD | Pass/fail test |
|---|---|---|---|---|
| NFR1 | Speed — first insight | The summary and top flags appear within 5 minutes of upload completion at the 90th percentile for typed or PDF bills, and within 10 minutes for photographed multi-page bills. | NFR-01 | A timing test on synthetic bills in the test environment meets both P90 figures. |
| NFR2 | Speed — full report | The full report is ready within 15 minutes of upload completion at the 90th percentile. Cases marked discharge-urgent go first in any queue. | NFR-02 | As NFR1; urgent cases are dequeued first. |
| NFR3 | Availability | Upload, processing and report download are available 99.5% of each calendar month; planned maintenance is outside peak hours. | NFR-04 | Monthly uptime measured by an external probe is at least 99.5%. |
| NFR4 | Accuracy | Line items are read correctly at least 95% of the time on digital PDFs and 90% on good-quality photos; "Likely error" findings are correct at least 90% of the time and "Worth asking" at least 75%, all measured on the Labelled Test Set. Checks below target are hidden. | NFR-05, §8.4 | The accuracy run reports each figure; any check below target is in shadow mode. |
| NFR5 | Explainability | Every finding traces to its source page and line, rule ID and reference-data version. | NFR-06 | Every finding in a generated report carries all three. |
| NFR6 | Reproducibility | Re-running a report with the same inputs and rule versions gives the same findings. | NFR-07 | Two runs of the same synthetic case produce identical findings. |
| NFR7 | Security | Personal data is encrypted at rest and in transit; internal staff use strong authentication; access is role-based and least-privilege; every access to personal data is logged; third-party security testing happens before launch and periodically after. | NFR-08 | Configuration review shows encryption on every store and TLS on every connection; a security test report exists before launch. |
| NFR8 | Privacy | No user profile data is collected; originals are deleted after extraction; only de-identified structured data is kept, with consent asked after the report, for 3 months; no sale of data. Guardrail target: zero personal-data incidents. | NFR-09 | Covered by FR2.2 to FR2.9 tests; the data inventory shows no profile fields. |
| NFR9 | Data residency | Personal and health data are stored and processed in the location the founder decides (India only, or India first). No real bill reaches any environment until that decision is made. | NFR-10, G-14 | The founder decision is recorded; hosting and processors match it. |
| NFR10 | Incident response | A written breach-detection and notification process meets the DPDP Rules and CERT-In directions, including the 6-hour reporting rule for specified incidents, and can be carried out by one person. | NFR-11, CR-R3 | The incident plan exists and has been walked through once before launch. |
| NFR11 | Device and network | The target device is an 8 GB RAM Android phone on a typical 4G connection. Uploads resume within the session after a dropped connection, and files already uploaded are not sent again. Each page loads at most 300 KB of JavaScript. | NFR-12 | A dropped-connection test resumes the upload without re-sending completed files; a build report shows no page over 300 KB of scripts [Q9], [Q14]. |
| NFR12 | Accessibility | User screens meet WCAG 2.2 level AA on phone and desktop browsers, support large text and screen readers, and allow a family member to run the check and forward the PDF. | NFR-13, M16 | Automated accessibility checks plus a manual screen-reader pass find no level A or AA failure [Q8]. |
| NFR13 | Localisation | Amounts use ₹ and lakh/crore digit grouping; dates and times use IST. Text is kept separate from code so Hindi and regional languages can be added later. | NFR-14 | Rendered amounts and dates follow the format; no user-facing string is hard-coded in components. |
| NFR14 | Scalability | The system handles 10 times pilot volume (planned at about 10,000 audits a month) without missing NFR1 or NFR2. | NFR-15, CR-T12 | A basic load test at 10 times pilot volume meets NFR1 and NFR2 (`team-practices.md`, Testing Posture). |
| NFR15 | Auditability of rules | Every rule-library change records who, what, why and the source, and is approved before it takes effect. | NFR-16 | A rule change without approval cannot go live; the change log shows all four fields. |
| NFR16 | AI governance | Every prompt or model change and every new check is measured on the Labelled Test Set before release; output accuracy is monitored for drift. There is no human review of users' audits. | NFR-17 | Each release record links an accuracy run on the changed version. |
| NFR17 | Content safety | No accusatory language; disclaimers present; letters follow the language guardrails. | NFR-18 | Covered by FR13.1 and FR13.2 tests. |
| NFR18 | Observability | Processing queues are tracked in real time with alerting before NFR1 or NFR2 would be missed; every component reports at least one health metric and one error-rate metric. | NFR-19; `aidlc/spaces/default/memory/phases/operation.md` | Dashboards show queue length and age; an alert fires in a test when queue age passes its threshold. |
| NFR19 | Vendor risk | Every third party that processes personal data is contracted on data-processing terms, and third-party models are not trained on user data. | NFR-20, CR-D7 | Each processor has a signed data-processing agreement with a no-training term. |
| NFR20 | Testability | Application source keeps at least 80% line coverage with Jest unit and component tests and Playwright end-to-end tests, using synthetic bills only; tests run before every merge. | — (`team-practices.md`) | The coverage report shows at least 80%; no fixture contains a real bill. |
| NFR21 | Telemetry | Next.js usage reporting is off on every machine and host (`NEXT_TELEMETRY_DISABLED=1`), and `experimental.agentFeedback` is off. | — (`architecture.md`; `team-practices.md`) | Build and host configuration show both settings off. |

## 4. Constraints

- **Regulatory** (`constraint-register.md`): DPDP Act 2023 and Rules 2025 from day one (CR-R1); medical records are sensitive personal data (CR-R2); CERT-In 6-hour reporting (CR-R3, **[VALIDATE]**); no insurance selling, representation, legal advice or AI clinical opinion (CR-R4); no encouragement to withhold payment in a way that blocks discharge or treatment (CR-R5); no accusations (CR-R6); no legal or tax review before launch, an accepted risk (CR-R7); minors on a parent's declaration (CR-R8).
- **Data handling:** anonymous single session (CR-D1, CR-D2); originals deleted (CR-D3); identifiers stripped (CR-D4); 3-month deletion (CR-D5); test-set retention rules (CR-D6); no model training on user data (CR-D7).
- **Technical:** English only (CR-T5); shadow mode until a check passes (CR-T6); traceable, reproducible findings (CR-T7); avoid a single bill-reading vendor where practical (CR-T8, soft); build on the existing Next.js / React / TypeScript app (CR-T9; `architecture.md`); hosting undecided, with Vercel as a working assumption only (CR-T10); internal tools need their own sign-in (CR-T11).
- **Framework:** this Next.js version (16.4.3) differs from older versions; Cache Components and partial prefetching are on, so per-session data must stay request-scoped (`architecture.md`; FR2.10).
- **Organisational:** one builder plus the founder (CR-O1); no domain experts (CR-O2, CR-O3); real bills labelled by both people (CR-O4); no fixed timeline (CR-O5); no running-cost cap (CR-O6).
- **Development** (`team-practices.md`): real bills never in the repository or within the AI assistant's reach; synthetic bills only in tests; nothing deployed until hosting is decided; merges blocked on lint, type, format, dependency, signature and code-security failures.

## 5. Assumptions

| ID | Assumption | Why it is reasonable | Owner | Status |
|---|---|---|---|---|
| AS1 | Families using the product mostly have a phone at least as capable as the 8 GB target on typical 4G | Your choice [Q9], [Q14]; upload resume and the script limit protect weaker networks | Founder | Unvalidated; watch drop-off by device once live |
| AS2 | 10 bills with the error and 30 without are enough to judge a check's precision | Your choice [Q4]; some checks (implant caps, packages) have few qualifying bills | Builder | Unvalidated; revisit if a live check's ratings fall |
| AS3 | Without abuse controls at launch, misuse of the free tier stays low enough to absorb | Your choice [Q5]; PRD risk RK26 rated 6 (likelihood 3, impact 2) | Founder | Unvalidated until Phase 1.5 adds controls |
| AS4 | A public source of typical consumable quantities may exist; if not, Labelled Test Set ranges are used | `scope-document.md` §4 (G-03) | Builder | Open; affects only AA-04 checks (FR8.7) |
| AS5 | A parent's declaration is acceptable for minors' bills | PRD D15, RK24; accepted risk | Founder | **[VALIDATE]** with counsel later |
| AS6 | The PRD's de-identification fields (FR2.6) count as de-identified under the DPDP Act | PRD SD-05 | Founder | **[VALIDATE]** with counsel later |

## 6. Out of scope

| Item | Why | Where it goes |
|---|---|---|
| Identity-free abuse controls (S9, SD-09) | Your answer: "None at launch, add this as a requirement to Phase 1.5" [Q5] | Phase 1.5 |
| Share-in from messaging apps (S1, DU-04) | Dropped for this build [Q7] | A later build |
| Urgent-situation guidance (S7, TS-03) and pay-under-protest guidance (G-12) | Moved at scope definition (`scope-document.md` §3) | Phase 1.5 |
| Bill-correction measure (G-02) and the "was this bill revised?" question | Not handled in this build [Q13], [Q15] | — |
| Closed beta after launch | Ends at launch [Q6] | — |
| Report watermark, public data charter, replaceable bill reader as requirements | Logged as decisions only [Q11] | Design stages may still use them |
| Expert route and paid Expert Review | Phase 1.5 (`intent-statement.md`) | Phase 1.5 |
| Value threshold for the "Complex" label | Set later from usage data [Q2] | After launch |
| Could Have items: KB-06, IMP-04, OT-03, OT-04, OP-05 | Deferred (`scope-document.md` §3) | A later build |
| PRD Won't Have list W1–W22 and §12.17 Won't requirements, including user-visible price benchmarks, accounts, notifications, Hindi and regional languages, insurance checks and a native app | PRD §10.3 | Later phases or never |

### Decisions recorded here, not requirements

- **D-RA1 Report watermark.** The PRD recommends watermarking reports as a private, non-conclusive analysis; recorded as a decision only [Q11].
- **D-RA2 Public data charter.** The PRD recommends a public charter that data is never shared with insurers for underwriting; recorded as a decision only [Q11].
- **D-RA3 Replaceable bill reader.** The PRD recommends keeping the bill-reading service replaceable without rewriting checks; recorded as a decision only [Q11]. CR-T8 still recommends avoiding a single vendor.

## 7. Open questions

| ID | Question | Needed by | Owner |
|---|---|---|---|
| OQ1 | Data residency: India only or India first (G-14, NFR9) | Before hosting and before any real bill reaches an environment | Founder |
| OQ2 | Hosting choice; Vercel is a working assumption only (CR-T10) | Before the first deployment | Founder |
| OQ3 | The list of clinically sensitive categories for the "Complex" label (FR8.16) | Domain Design | Builder, confirmed by you |
| OQ4 | A measurable reading-level test for plain-language reports (FR10.4, PRD RP-04 "roughly school-leaver level") | NFR Requirements | Builder |
| OQ5 | The target window for applying regulatory changes to rules (FR14.3) | NFR Requirements | Founder |
| OQ6 | Founder or counsel check of the privacy wording on the start and "before you go" screens (FR1.12, R-02) | Before launch | Founder |
| OQ7 | Acceptance criteria for each requirement in Given/When/Then form (PRD gap G-21) | User Stories | Builder |
| OQ8 | Which bill-reading option wins the trial on synthetic and then real bills (`scope-document.md` §5) | Before connecting the bill reader | Builder |
