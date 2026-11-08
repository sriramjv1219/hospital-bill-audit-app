**Collaborator:** aidlc-design-agent

## Contribution

Design view: user experience, persona fidelity, journey flow, accessibility, and wording and tone. The drafts are strong on coverage and on wording guardrails. The items below are the gaps that matter most for Priya at the billing desk on a phone, for Anil while grieving, and for Meera working from abroad. They are in priority order. Each one cites its origin, so nothing enters without a source: the approved wireframes and user flow (`ideation/rough-mockups/`), the personas, or a requirement. Proposed criterion numbers continue each story's existing sequence.

### A. Critical for the billing-desk moment (C1 Priya)

**A1. US7.4: the idle timeout can wipe the report while Priya waits at the counter.** Priya may show the Billing Desk Summary, put the phone down in a queue, or wait up to 15 minutes for the full report (NFR2) without touching the screen. If her screen locks, she never sees the 25-minute warning, and at 30 minutes everything is gone. FR2.8 does not say what "idle" means. Proposed:
- AC7.4.4 Given the system is still processing my documents (from upload completion until the full report is ready), when the idle timer is checked, then the processing time does not count as idle. *(This is a judgment call; see Positions.)*
- AC7.4.5 Given the idle warning appears, when it is shown, then it is announced to screen readers, keyboard focus moves to "Keep going", and it states how many minutes are left and that the documents will be deleted.
- AC7.4.6 Given the session ended while my phone was locked, when I return to the tab, then I see "Your session ended and your documents were deleted. If you saved the report, it is still on your device." with a "Start a new check" button, and no error code (wireframes, session-wide states).

**A2. US7.1 and US5.4: prompt to save as soon as the report is ready, not only when leaving.** This is the real protection against A1 and against a dropped tab. Proposed:
- AC5.4.5 Given the full report has just become ready, when the report screen is shown, then a "Save or share" (phone) or "Download" (desktop) action is visible without scrolling, together with the line "Save a copy now. We can't send it to you later."

**A3. US7.1: AC7.1.1 cannot be met as written.** Browsers do not reliably run custom code when a tab is closed. Mobile Safari and Android Chrome often skip the leave-page prompt entirely, and no browser shows custom text in it. *(This is a knowledge point.)* Proposed rewrite:
- AC7.1.1 Given I have not saved the report, when I choose any in-product exit ("Start a new check", "Discard everything" or "Finish"), then I am reminded to save it first and told it cannot be retrieved later.
- AC7.1.3 Given I have not saved the report, when I reload or close the tab, then the browser's own leave-page prompt is requested (best effort; its text is set by the browser).
- AC7.1.4 Given the phone share menu opened and I cancelled it, when the "saved" state is checked, then the report is not counted as saved.

**A4. US5.3: the Billing Desk Summary must work across the desk and on a dropped connection.** The wireframe (S9) has "Show full screen" for showing the phone to the billing executive, but no criterion covers it. Proposed:
- AC5.3.5 Given I am on a phone, when I choose "Show full screen", then only the summary is shown, in large text, with no other controls except a "Close" button, and it stays readable at 200% text size.
- AC5.3.6 Given the summary has been shown once, when the connection drops, then the summary and "Show full screen" still work.

**A5. US5.3 and US6.1 conflict on what the summary contains.** Wireframe S9 and FR11.1 build the summary from the findings the user marked "Will ask", while AC5.3.4 picks the "highest-ranked items". There is no rule for the case where nothing is marked. Proposed:
- AC5.3.7 Given I marked one or more findings "Will ask", when the summary is generated, then it holds exactly those findings, with "please correct" items first.
- AC5.3.8 Given I marked none, when the summary is generated, then it holds the Top 3 (US5.1) and says "You can choose which points to raise" with a link back to the findings.
- AC5.3.9 Given a finding is marked "Not pursuing" or is low-confidence and not marked "Will ask", when the summary is generated, then it is not included.

### B. Journey flow gaps against the approved wireframes and user flow

**B1. No story covers the "When are you checking?" moment (wireframe S3; user-flow §2; R-04).** The stories rely on knowing whether the bill is interim or final (AC2.9.2, AC2.9.3) and whether it is already paid (AC5.8.3, and S10 leading with the refund request), but nothing says how the system knows. My recommendation: infer it rather than add a screen, because that is one less step for Priya. Proposed, on US3.1:
- AC3.1.4 Given my documents, when the case details are shown, then they include the bill stage (interim or final) and whether the bill is already paid, both inferred from the documents (bill type, payment receipts, amount paid) and editable.
- AC3.1.5 Given I change the bill stage or the paid status, when the checks and next steps update, then the checklist (US2.9) and the order of letters (US5.6) follow the new value.
- Origin: wireframe S3 and PRD §11.2 stage 1. *(This is a judgment call; see Positions.)*

**B2. US5.6: "Already paid" should lead with the revised-bill or refund request (user-flow §2; wireframe S10).** Proposed:
- AC5.6.4 Given the case is marked already paid, when the letters are listed, then the request for a revised bill, a refund or a credit note comes first.

**B3. US3.5 and US6.7: regeneration must keep the user's work and say what changed.** After "Fix a line" or "Add a document", the report changes under the user. Proposed:
- AC3.5.4 / AC6.7.3 Given I had marked findings "Will ask" or "Not pursuing" or had rated them, when the report regenerates, then those marks and ratings stay on every finding that still exists.
- AC3.5.5 / AC6.7.4 Given the report regenerates, when it is shown, then a short notice says what changed (for example "1 finding removed, total changed by ₹1,840"), and the notice is announced to screen readers.
- AC6.7.5 Given a finding I marked "Will ask" no longer exists after regeneration, when the notice is shown, then it names that finding.

**B4. US3.3: say what the first look contains, and keep it calm.** FR7.5 fixes only the label. The wireframe (S5) and NFR1 define the content. Proposed:
- AC3.3.4 Given the first look is shown, when I read it, then it shows the total billed, advances paid, balance due, the three largest categories and the number of first flags.
- AC3.3.5 Given first flags are shown before I confirm, when they are labelled, then they read "worth a look" and carry neither the "Likely error" class nor a rupee total. *(This is a judgment call.)* A misread digit that is later withdrawn after confirmation (FR7.2) should not have shown a frightened family a ₹40,000 "Likely error" in the meantime.

**B5. US3.2 and US5.2: progress and long waits.** Proposed:
- AC3.2.3 Given processing takes longer than the time shown, when the estimate passes, then the message changes to "Taking longer than usual. Still working." and never shows a stalled bar.
- AC3.2.4 Given processing status changes, when it changes, then the change is announced politely to screen readers at milestones only (reading pages, running checks, report ready), not on every progress tick.
- AC5.2.5 Given the full check is still running, when I am on the bill check screen, then the parts already ready are shown, the rest are marked "Still checking", and "Full report ready" is announced when it arrives (wireframe S7, "Check still running").

**B6. US1.1: keep the path to upload short.** US1.2 to US1.7 read as five or more separate screens. The approved wireframes combine them into S1 (start and explainer) and S2 (notice, relationship and both boxes). Proposed:
- AC1.1.5 Given I am the patient or I am checking for an adult who has given consent, when I go from the start screen to upload, then I pass through at most two screens before upload, plus one more only when a guardian or deceased-patient declaration is needed. *(This is a judgment call on the number; the wireframe implies two.)*

**B7. Starting another check (C2 Meera may check for several relatives).** Proposed, on US7.1:
- AC7.1.5 Given I finish a check, when I choose "Start a new check", then after the save reminder the old session ends, and nothing from it (files, findings or marks) appears in the new one.

### C. Consent wording and declarations

**C1. US1.5: relationship labels must be unambiguous.** In AC1.5.1, "parent" and "child" can be read either way. Proposed:
- AC1.5.1 (revised) Given I am at the relationship step, when I look at the choices, then they are phrased from my side as "Whose bill is this?": Mine; My spouse's; My parent's; My child's; Another family member's; I am an authorised representative; I am a professional helping the family.
- AC1.5.4 Given I choose anyone other than "Mine", when I continue, then I am asked "Is the patient under 18?", and the answer leads to the guardian declaration (US1.6) or the adult declaration (US1.7). An adult child or a younger sibling then takes the right path. US1.6 currently triggers only on "Given I mark the patient as under 18" with no step that asks.

**C2. AC1.5.2 and AC1.6.2: "highlighted" alone is colour-only feedback (WCAG 1.4.1 and 3.3.1).** Proposed rewrite:
- AC1.5.2 Given I have chosen a relationship but not ticked the authorisation box, when I press "Continue", then I stay on the screen, a text message next to the box says what is needed, focus moves to the box, and the message is announced.

**C3. US1.7 (C4 Anil): make "gentle wording" testable.** Proposed rewrite:
- AC1.7.2 Given the patient has died, when the declaration is shown, then the choice reads "My [relationship] has passed away", and the text contains none of "deceased", "death certificate", "legal heir" or "proof", and asks only for a declaration.
- AC1.7.4 Given I chose "has passed away", when the report and tips are generated, then no text refers to the patient's recovery or future care (for example "get well", "follow-up appointment", "your next stay"), and "What to do next time" (US5.9) is phrased for the family.
- AC1.7.5 Given I chose "has passed away", when a stored record is written with storage consent, then that answer is not stored (it only sets the wording; FR2.5 list).

**C4. US7.2: the storage consent screen must not say "anonymous", and must not steer the choice.** R-02 and FR1.12 note that the data is de-identified, not anonymous. Wireframe S11 still says "anonymous copy". Proposed:
- AC7.2.6 Given the storage question, when it is shown, then it describes the data in plain words ("bill lines with names, numbers and addresses removed, deleted after 3 months"), does not use the word "anonymous", and shows "Yes, keep it" and "No thanks" as buttons of equal size and weight, with neither pre-selected.

**C5. US5.8 (C4 Anil and C1 Priya): the reassurance should not depend on the bill being paid.** The persona needs "reassurance that issues can be raised after payment". Saying this to a family that has not yet paid is also the safest reading of C10, because it removes the pressure to hold back payment. Proposed rewrite:
- AC5.8.3 Given any report, when the escalation ladder is shown, then it states that issues can still be raised after the bill is paid, and nothing on the screen suggests delaying payment or discharge.

### D. Findings and report wording

**D1. US4.9: class and confidence are never shown by colour alone, and check IDs stay secondary.** Proposed:
- AC4.9.5 Given any finding, when it is shown, then its class and confidence are written as text ("Likely error", "High confidence"), and any colour or icon is extra.
- AC4.9.6 Given any finding, when it is shown in the list or in the Top 3, then it uses a plain-language name (for example "Room charged for an extra day"), and the check ID appears only in the evidence detail and the report appendix.

**D2. US4.9 or US5.2: objective errors first (personas C4 and S1; user-flow §4).** Proposed:
- AC4.9.7 Given a findings list, when it is shown, then "Likely error" findings come before "Worth asking", which come before "Explained", and within each class the higher amount potentially affected comes first.

**D3. US4.11: "no findings" must not read as "your bill is correct".** Proposed addition to AC4.11.3: "...and the message lists which checks ran and which could not, so that nothing found is not taken to mean the bill was fully checked."

**D4. AC4.14.3: the list of checks not yet available must be understandable and not alarming.** Proposed:
- AC4.14.5 Given some checks did not run (shadow mode or missing documents), when they are listed, then they appear under plain names grouped by reason ("Still being tested", "Needs a document you can add"), each "Needs a document" entry has an "Add a document" action, and no check code is the only label.

**D5. US4.11: the Bill Breakdown has a text equivalent.** Proposed:
- AC4.11.4 Given the Bill Breakdown is shown, when it is read by a screen reader or printed, then the four parts and their amounts are available as a table or list in text, not only as a chart.

**D6. US4.12: the collapsed group is a proper disclosure.** Proposed:
- AC4.12.3 Given the "Other things you may ask about" group, when it is shown, then its label includes the count (for example "(5)"), it opens and closes with Enter or Space, and its open or closed state is announced.

**D7. US4.13: "Complex" findings say what the user can do, given there is no expert route in Phase 1** (user-flow §3). Proposed:
- AC4.13.5 Given a "Complex — consider professional advice" finding, when it is shown, then it says this check cannot judge it, suggests asking the treating doctor or the hospital's patient relations team for an explanation, and gives no clinical opinion (CR-R4).

**D8. US3.5 and US4.9: bill-line images versus original-file deletion.** Wireframes S6 and S8 show "a small image of the bill line" and "See the bill line", but FR2.3 deletes the original once it has been read. *(This is a knowledge point.)* Proposed:
- AC4.9.8 Given a finding's evidence, when it is shown, then it gives the page, the line and the text as read. A bill-line image appears only if it can be drawn from the file still held on my own device in this browser tab; no server-side image or crop is kept to show it.

### E. Upload on a phone

**E1. AC2.2.1 cannot be met.** A web page cannot draw tips inside the phone's own camera app (the `capture` file input hands over to the native camera). The wireframe already says "Photo tips appear before the camera opens". *(This is a knowledge point.)* Proposed rewrite:
- AC2.2.1 Given I choose "Take a photo" on my phone, when the tips screen appears before the camera opens, then it shows the three tips (lay the page flat, avoid glare, keep all edges in the frame) and an "Open camera" button, and I can choose not to see the tips again in this session.

**E2. Limit and refusal messages say what to do, not only what went wrong** (US2.1, US2.3; UX heuristic 9). Proposed addition to AC2.1.4 and AC2.3.1 to AC2.3.3: "...and the message suggests a next step (for example 'Save it as a PDF or a photo and try again', or 'Remove a file to add another')."

**E3. US2.6: Priya identifies paper pages by sight, not by number.** Proposed addition to AC2.6.1: "...and shows a small preview of that page from my device, so I can find the right paper."

**E4. US2.10: upload status is announced.** Proposed:
- AC2.10.4 Given files are uploading, when a file finishes, fails or pauses, then the status is written next to the file and announced politely to screen readers.

### F. Accessibility (US7.8) — make it specific and testable

AC7.8.1 as written ("automated checks find no WCAG 2.2 A or AA failure") overclaims. Automated tools find only about a third of issues. *(This is a knowledge point.)* I propose keeping it as the automated gate and adding these criteria, each grounded in NFR12 and the wireframe design basis:
- AC7.8.4 Given only a keyboard, when I complete the whole journey from start to download, then every action can be reached, focus is always visible and is never hidden behind a sticky header or footer (WCAG 2.4.7 and 2.4.11), and the first stop on every screen is "Skip to content".
- AC7.8.5 Given a screen 320 CSS px wide, when any user screen is shown, then nothing scrolls sideways except bill-line tables, which scroll inside their own region (WCAG 1.4.10).
- AC7.8.6 Given any tap target on a phone, when it is measured, then it is at least 44 × 44 CSS px (wireframe design basis; stricter than the 24 px in WCAG 2.5.8).
- AC7.8.7 Given text and controls, when contrast is measured, then text is at least 4.5:1 and control borders and focus rings are at least 3:1.
- AC7.8.8 Given the "reduce motion" setting is on, when progress or transitions are shown, then no animation runs beyond a simple state change.
- AC7.8.9 Given a finding opens (wireframe S8), when it opens and closes, then focus moves to its heading and then back to the list item it came from.
- AC7.8.10 Given the step indicator (US3.2), when a screen reader reads it, then it announces the step as text (for example "Step 2 of 3: Review") and marks the current step.

**F1. US5.4: PDF accessibility.** A PDF built in the browser is usually untagged, so screen-reader support inside the PDF is limited. *(This is a knowledge point.)* Proposed:
- AC5.4.6 Given the PDF, when it is opened, then its text is real, selectable text (not an image), in reading order, and the on-screen report offers the same content as the accessible version.
- AC5.4.7 Given the PDF is being built on the device, when it takes longer than a moment, then "Preparing your report" is shown and announced. The time limit on the 8 GB test device is set in NFR Requirements.
- AC5.4.8 Given the PDF file name, when it is saved, then it holds no patient name or identifier (for example `bill-check-2026-11-07.pdf`).

### G. Microcopy consistency (cross-cutting)

The ACs quote button labels that differ from the approved wireframes. FR wording wins over the wireframe in each case: "Keep going" (FR2.8) replaces "I'm still here"; "Confirm a few details" (FR7.5) replaces "See the full check"; "Save or share" (FR10.3) replaces "Share"; "Discard everything" replaces "Discard everything now". I suggest the stories add one line under "How to read these stories": *quoted UI labels are fixed text, kept in one label list in Refined Mockups and reused in the tests.* That keeps the end-to-end tests, the wording test (US5.10) and the screens in step.

Also on US5.5: AC5.5.3 should say "times show the 'IST' label", so Meera in Singapore does not read escalation deadlines as local time.

## Positions

- AGREE: Grouping by journey step [Q2], with Kamala served through Meera's and Priya's accessibility stories, keeps the stories faithful to how families actually move through one session.
- AGREE: Using WCAG 2.2 AA (NFR12) rather than the wireframes' 2.1 is right. Refined Mockups should update the wireframe design basis to match.
- AGREE: The Won't Have list (Group 11) and the stage-level decisions table make the scope boundary visible. This helps when design questions come up later in the build.
- AGREE: US5.10 and the banned-word test are a sound basis for the tone needed by C4 Anil. My C3 and D items only make "gentle" testable.
- OBJECT: (judgment call) No story covers the wireframe S3 "When are you checking?" moment, yet AC2.9.2, AC2.9.3, AC5.8.3 and the refund-first ordering depend on it. I recommend inferring the bill stage and paid status from the documents and confirming them in US3.1 (B1), rather than adding a question screen. Asking is also legitimate if the founder prefers an explicit answer.
- OBJECT: (judgment call) FR2.8 "idle" is undefined. As written, a family waiting for processing or standing in a counter queue can lose everything at 30 minutes. I recommend that processing time does not count as idle (AC7.4.4), alongside the save-now prompt (AC5.4.5). Keeping a strict 30 minutes from the last tap is also defensible on privacy grounds, and the founder should decide.
- OBJECT: (judgment call) First-look flags shown before confirmation should not carry the "Likely error" class or a rupee total (AC3.3.5), so that a misread digit does not alarm a family and then vanish. Showing the class early gives faster insight, which is a legitimate opposite view.
- OBJECT: (judgment call) AC5.3.4 ("highest-ranked items") conflicts with wireframe S9 and FR11.1 ("built from 'Will ask'"). I recommend "Will ask" first, the Top 3 when nothing is marked, and "Not pursuing" never included (AC5.3.7 to AC5.3.9).
- OBJECT: (judgment call) The consent chain US1.2 to US1.7 implies five or more screens, whereas wireframes S1 and S2 imply two. I recommend capping the path to upload (AC1.1.5). The exact cap is the founder's choice.
- OBJECT: (knowledge point) AC2.2.1 is not achievable in a web app, because tips cannot be shown inside the native camera. They must appear before it opens, as wireframe S4 already says.
- OBJECT: (knowledge point) AC7.1.1 cannot be guaranteed for tab close on mobile browsers, and custom leave-page text is not shown by any browser. Rewrite it as in-product exits plus a best-effort browser prompt (A3).
- OBJECT: (knowledge point) The bill-line images in wireframes S6 and S8 conflict with FR2.3 deletion unless they are drawn only from the file still on the user's device. AC4.9.8 sets that boundary for Functional Design.
- OBJECT: (knowledge point) AC1.5.2 "the box is highlighted" is colour-only error feedback, which fails WCAG 1.4.1 and 3.3.1. It needs a text message, focus moved to the box, and an announcement. AC1.5.1's "parent" and "child" are ambiguous without "my ...'s" phrasing, and US1.6 has no step that asks whether the patient is under 18.
- OBJECT: (knowledge point) AC7.8.1 on its own cannot prove "no level A or AA failure", because automated scanners find only a minority of issues. Add the keyboard, reflow, target-size, focus and announcement criteria in F.
- OBJECT: (knowledge point) AC5.8.3 makes "you can still raise issues after paying" conditional on the bill being paid. Persona C4 states this need without any condition, and saying it to every family supports the C10 constraint. It should appear in every report.
