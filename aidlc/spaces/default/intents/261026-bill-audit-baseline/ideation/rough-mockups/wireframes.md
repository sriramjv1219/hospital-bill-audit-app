# Rough Wireframes — Hospital Bill Check (Project RS, Phase 1)

These are low-fidelity text sketches of the user-facing screens for the Phase 1 Hospital Bill Check. They are grounded in:

- the approved intent statement (`intent-statement.md`);
- the scope in `scope-document.md` and `intent-backlog.md` (`ideation/scope-definition/`).

The path through these screens is in `user-flow.md`. All amounts, names and bill lines shown are **illustrative placeholders**, not real data.

## Sources

- [Q1]–[Q5]: answers in `rough-mockups-questions.md` in this folder.
- PRD references: `PRDs/app-baseline.md`.
  - Journey J1: §11.2.
  - Onboarding and consent: ON-01–ON-08.
  - Session and storage: SD-01–SD-08.
  - Upload: DU-01–DU-11.
  - Recommendations: RE-01–RE-09.
  - Report: RP-01–RP-08.
  - Actions: AD-01.
  - Feedback: FB-01, FB-02.
  - Finding classes, confidence and "Who to Ask": §13.1.
  - Billing desk design rules: §9.3.

## Design basis

| Topic | Decision | Source |
|---|---|---|
| Look | Plain and neutral; no brand yet; "Project RS" as a placeholder name | [Q1] |
| Screen sizes | Phone and desktop designed equally. Each screen shows the phone layout, with a note on the desktop layout | [Q2] |
| Accessibility | WCAG 2.1 level AA: text contrast at least 4.5:1, touch targets at least 44 × 44 px, every action reachable by keyboard, visible focus, meaning never shown by colour alone, large-text support | [Q3]; PRD NFR-13 |
| Language | English only; plain words at about a school-leaver reading level; no "fraud", "scam", "cheating" or "illegal" | PRD D9, RP-04, §13.1.5 |
| Scope | User-facing screens only; internal tools are described in a paragraph at the end | [Q4] |
| Form | Text sketches only | [Q5] |

Common to every screen:

- **Header:** placeholder name, plus the progress steps "Upload → Review → Report" (PRD CM-03).
- **Footer:** the line "Information only, not legal or medical advice" (PRD TS-01).
- **Accessibility:** a "Skip to content" link is the first thing the keyboard reaches.

---

## S1. Start

```
+--------------------------------------+
| Project RS                           |
|--------------------------------------|
|  Is your hospital bill right?        |
|                                      |
|  We read your bill line by line and  |
|  show what may be worth asking the   |
|  hospital about, with the evidence.  |
|                                      |
|  - Free. No sign-up.                 |
|  - Nothing about you is saved.       |
|  - Takes about 10 minutes.           |
|                                      |
|  What we do NOT do:                  |
|  - legal or medical advice           |
|  - check what insurance will pay     |
|                                      |
|  [  Check my bill  ]                 |
|--------------------------------------|
| Information only. Not legal advice.  |
+--------------------------------------+
```

- **Purpose:** explain what the product does and does not do before anything is shared (PRD ON-08).
- **Desktop:** the same content centred, with a short "How it works" strip of three steps beside it.
- **Accessibility:** h1 "Is your hospital bill right?"; landmarks header / main / footer; keyboard entry at the "Check my bill" button.

## S2. Privacy and consent

```
+--------------------------------------+
| Project RS     Upload > Review > Rpt |
|--------------------------------------|
|  Before you upload                   |
|                                      |
|  In short:                           |
|  - We use your documents only to     |
|    check this bill.                  |
|  - Your files are deleted after we   |
|    read them.                        |
|  - We never ask your name or number. |
|  [ Read the full notice v ]          |
|                                      |
|  Whose bill is this?                 |
|  ( ) Mine        ( ) My parent       |
|  ( ) My spouse   ( ) My child        |
|  ( ) Other family ( ) Authorised rep |
|  ( ) Professional                    |
|                                      |
|  [ ] I am allowed to share this      |
|      patient's documents             |
|  [ ] I agree to the processing of    |
|      these documents for this check  |
|                                      |
|  [  Continue  ]                      |
+--------------------------------------+
```

- **Purpose:** a layered privacy notice, the uploader's relationship to the patient, and the authority to share (PRD ON-03, ON-04, ON-06).
- **Extra declarations:**
  - If "My child" is chosen, a declaration that the uploader is the parent or lawful guardian appears (ON-05).
  - For another adult, a declaration of the patient's consent appears, or of the uploader's authority where the patient is incapacitated or has died (ON-06).
  - The wording for a deceased patient is gentle; bereaved families are a primary customer (`intent-statement.md`).
- **Consent to store de-identified data is NOT asked here.** It is asked after the report (PRD ON-03(c), D19).
- **Desktop:** the notice summary on the left, the choices on the right.
- **Accessibility:** h1 "Before you upload"; landmark main; the radio group has a visible legend; keyboard entry at the first radio option; "Continue" stays disabled until both boxes are ticked, with a text explanation of why.

## S3. When are you checking?

```
+--------------------------------------+
|  When are you checking this bill?    |
|                                      |
|  [ In hospital (interim bill)     ]  |
|  [ At discharge (final bill)      ]  |
|  [ Already paid                   ]  |
|                                      |
|  Insured? We check the hospital's    |
|  billing only, not what insurance    |
|  will pay.                           |
+--------------------------------------+
```

- **Purpose:** the user picks the moment, which tailors the checklist and the actions (PRD §11.2 stage 1).
- **The insurance note** is always visible (PRD D20).
- **Desktop:** the three choices side by side as cards.
- **Accessibility:** h1 "When are you checking this bill?"; three large buttons (at least 44 px tall); keyboard entry at the first choice.

## S4. Upload and checklist

```
+--------------------------------------+
|  Add your documents                  |
|                                      |
|  [ Take photo ]  [ Choose files ]    |
|                                      |
|  Checklist                           |
|  [x] Hospital bill (3 pages)         |
|  [!] Itemised bill  - needed         |
|      "This looks like a summary      |
|       bill. Ask the hospital for the |
|       itemised bill."                |
|      [ Get request letter ]          |
|  [ ] Pharmacy bills    (optional)    |
|  [ ] Implant invoice   (if any)      |
|  [ ] Discharge summary (optional,    |
|      lets us check more)             |
|  [ ] Estimate / package letter       |
|  [ ] Payment receipts                |
|                                      |
|  Page 3 of 7 seems missing. [ Add ]  |
|                                      |
|  [  Check my bill  ]                 |
+--------------------------------------+
```

- **Purpose:**
  - guided capture with a per-case checklist (PRD DU-01–DU-03, DU-10);
  - summary-vs-itemised detection with a request letter (DU-08, RE-06a);
  - prompts about missing pages and unreadable pages (DU-06, DU-07).
- **Photo tips** appear before the camera opens: lay the page flat, avoid glare, keep all edges in view (DU-03).
- **Interim bills:** a note says "Upload interim and final bills together so we can spot amounts counted twice" (PRD gap G-20, check DUP-07).
- **Desktop:** a drag-and-drop area on the left (with a "Choose files" button as the keyboard alternative) and the checklist on the right.
- **Accessibility:** h1 "Add your documents"; landmark main; checklist status is given in text ("needed", "added"), not only icons; upload progress is announced politely to screen readers; keyboard entry at "Take photo" (phone) or "Choose files" (desktop).

### S4 states

| State | What the user sees |
|---|---|
| Empty | The checklist with nothing ticked and the line "Start with the hospital bill" |
| Uploading on a weak network | Per-file progress; "Weak connection — we'll keep trying" (uploads resume within the session, PRD NFR-12) |
| Unreadable page | "Page 2 is blurry. Please retake it." with [ Retake ] (DU-07) |
| Password-protected PDF | A password field. The password is not stored (DU-09, Should Have) |
| Upload failed | Plain explanation plus [ Try again ]; nothing already added is lost |

## S5. Reading your bill / Quick summary

```
+--------------------------------------+
|  Reading your bill...                |
|  [#########.........]  about 3 min   |
|  You can stay on this page.          |
|--------------------------------------|
|  Quick summary  (ready)              |
|  Total billed        Rs 2,48,600     |
|  Advances paid       Rs 1,50,000     |
|  Balance due         Rs   98,600     |
|                                      |
|  Biggest items                       |
|  - Room charges      Rs 62,000       |
|  - Pharmacy          Rs 48,300       |
|  - Consumables       Rs 31,900       |
|                                      |
|  First things worth a look: 3        |
|  [  See the full check  ]            |
+--------------------------------------+
```

- **Purpose:** a quick read while the family waits at the desk. It shows the total, the category split, the largest items, the top flags and the balance due after advances (PRD §11.2 stage 3).
- **Speed target:** within 5 minutes for typed or PDF bills (`intent-statement.md`).
- **Amounts** use Indian formatting: lakh and crore, ₹ (PRD NFR-14).
- **Desktop:** the summary figures in a row of tiles.
- **Accessibility:** h1 changes from "Reading your bill" to "Quick summary"; progress is announced politely; the figures are a real table for screen readers; keyboard entry at "See the full check".

## S6. Please confirm

```
+--------------------------------------+
|  Please check a few things           |
|                                      |
|  Hospital:  City Care Hospital, Pune |
|  Admitted:  03 Oct   Discharged: 09 Oct|
|  Paying by: ( ) Self ( ) Reimburse   |
|             ( ) Cashless             |
|  [ Edit ]                            |
|                                      |
|  We were not sure about 2 lines:     |
|  p.4 line 12  "Inj. ....rone 1g"     |
|     Qty [ 6 ]  Amount [ 1,840 ]      |
|  p.5 line 3   (in Hindi - not read)  |
|     [ Skip ]  [ Type it in ]         |
|                                      |
|  [  Confirm and continue  ]          |
+--------------------------------------+
```

- **Purpose:**
  - case details are inferred from the documents and confirmed, not typed in (PRD CM-01, CM-02);
  - the user confirms low-confidence lines (DP-19–DP-22, M7);
  - lines in Hindi or regional scripts are listed as not read, never guessed (PRD D9, DP-08).
- **Desktop:** the case details on the left; the unclear lines on the right, each beside a small image of the bill line.
- **Accessibility:** h1 "Please check a few things"; every input has a visible label; the bill-line image has alt text naming the page and line; keyboard entry at the first unclear line.

## S7. Your bill check (Bill Breakdown and findings)

```
+--------------------------------------+
|  Your bill check                     |
|  Rs 2,48,600 billed                  |
|                                      |
|  Bill Breakdown                      |
|  Likely error      Rs  7,420  (3)    |
|  Worth asking      Rs 12,300  (4)    |
|  Explained         Rs 2,21,180       |
|  Not assessable    Rs  7,700         |
|                                      |
|  Top 3 things to do now              |
|  1 Ask: room charged 7 days, stay    |
|    was 6 days          Rs 4,000      |
|  2 Ask: same scan billed twice       |
|                        Rs 2,600      |
|  3 Ask: drug above printed MRP       |
|                        Rs   820      |
|                                      |
|  Findings  [All][Likely][Worth asking]|
|  > Room days - Likely error  High    |
|  > Duplicate scan - Likely error     |
|  > ...                               |
|  > Other things you may ask about (5)|
|                                      |
|  [ Billing Desk Summary ]            |
|  [ Download report (PDF) ]           |
+--------------------------------------+
```

- **Bill Breakdown** splits the total into likely errors / worth asking / explained / not assessable (PRD §13.1.1, AA-15).
- **"Top 3 things to do now"** (RE-05).
- **Findings** are listed with their class label and confidence. Low-confidence findings below the value threshold are collapsed under "Other things you may ask about" (§13.1.2).
- **Labels are always text** ("Likely error", "Worth asking"), never colour alone.
- **Complex or clinical findings** carry "Complex — consider professional advice" (§13.1.3).
- **Desktop:** two columns, with the Bill Breakdown and Top 3 on the left and the findings list on the right.
- **Accessibility:** h1 "Your bill check"; h2 for "Bill Breakdown", "Top 3 things to do now" and "Findings"; the filter is a tab list operated with the arrow keys; keyboard entry at the first finding.

### S7 states

| State | What the user sees |
|---|---|
| Partial (summary bill only) | "We could only check part of your bill because it is a summary bill." Lists the checks not possible and why (PRD DP-21, RP-01 part 5), with [ Get itemised-bill request ] |
| No findings | "We found nothing that looks wrong in what you gave us." Also shows what was checked, plus the explained items, so the family can stop worrying |
| Check still running | The findings appear as they are ready; the full report is due within 15 minutes (`intent-statement.md`) |

## S8. Finding detail

```
+--------------------------------------+
|  < Back                              |
|  Room days                           |
|  Likely error  -  High confidence    |
|  Ask the hospital                    |
|                                      |
|  What it is                          |
|  Daily charge for your room.         |
|                                      |
|  What we noticed                     |
|  7 room days billed (p.2, line 4).   |
|  Discharge summary: admitted 03 Oct, |
|  discharged 09 Oct = 6 days.         |
|  [ See the bill line ]               |
|                                      |
|  Why it may be wrong                 |
|  One day more than the stay.         |
|  Amount affected: Rs 4,000           |
|                                      |
|  What to do                          |
|  Ask the billing desk to check the   |
|  room days. Take the discharge       |
|  summary.                            |
|                                      |
|  ( ) Will ask   ( ) Not pursuing     |
|  Was this helpful? [Yes][No][Wrong]  |
+--------------------------------------+
```

- **Each finding has four parts:** what it is, what we noticed (with evidence), why it may be wrong, and what to do (PRD RE-01–RE-04).
- **Evidence** names the bill page and line (§13.1.5, NFR-06).
- **"Will ask / Not pursuing"** builds the checklist (AD-01). The rating is FB-01.
- **Desktop:** the finding opens in a side panel next to the list, and the bill-line image sits beside the evidence.
- **Accessibility:** h1 is the finding name; h2 for each of the four parts; focus moves to the h1 when the finding opens and returns to the list item on "Back"; keyboard entry at "Back".

## S9. Billing Desk Summary

```
+--------------------------------------+
|  Billing Desk Summary                |
|  Bill no. 4471  dated 09 Oct         |
|  City Care Hospital, Pune            |
|                                      |
|  Please correct                      |
|  1 p.2 l.4  Room days: 7 billed,     |
|    stay 6 days           Rs 4,000    |
|  2 p.6 l.9  CT scan listed twice     |
|    (also p.6 l.11)       Rs 2,600    |
|  3 p.8 l.2  Price above MRP Rs 820   |
|                                      |
|  Please explain                      |
|  4 p.5 l.7  Gloves x 120 for 6 days  |
|  5 p.3 l.1  Package: is the ICU      |
|    charge included?                  |
|                                      |
|  Thank you for checking these.       |
|                                      |
|  [ Show full screen ]  [ Share ]     |
+--------------------------------------+
```

- **One page** that shows "Please correct" (objective errors) first and "Please explain" (questions) second. It quotes the bill number, date, page and line, uses neutral and courteous wording, and contains no confidence jargon (PRD §9.3, RP-02).
- **It is built from** the findings the user marked "Will ask" (AD-01).
- **"Show full screen"** enlarges it so it can be shown across the desk.
- **Desktop:** the same single page, sized for printing.
- **Accessibility:** h1 "Billing Desk Summary"; h2 for "Please correct" and "Please explain"; each item is a real list item; keyboard entry at "Show full screen".

## S10. Next steps and templates

```
+--------------------------------------+
|  What you can do next                |
|                                      |
|  Ready-to-use letters (you can edit) |
|  - Itemised bill request             |
|  - Billing query list                |
|  - Implant / package document req.   |
|  - Revised bill or refund request    |
|  - Grievance escalation letter       |
|  - NPPA complaint (price above MRP)  |
|                                      |
|  If the billing desk can't help      |
|  1 Billing desk                      |
|  2 Patient relations                 |
|  3 Medical superintendent /          |
|    grievance officer                 |
|  4 State health authority (where     |
|    applicable)                       |
|  5 NPPA (drug or device prices)      |
|  6 Consumer Commission               |
+--------------------------------------+
```

- **Templates:** RE-06 (a)–(d) are Must Have; (e) the grievance letter and (f) the NPPA pointer are Should Have and in scope (`scope-document.md`).
- **Escalation ladder:** RE-08, with hospital-side deadlines taken from the Phase 1 reference data (PRD gap G-11).
- **Wording:** letters are respectful and factual and never allege fraud (RE-07). Nothing on this screen advises withholding payment (constraint C10).
- **Already paid:** the screen leads with the revised-bill and refund request (PRD §11.2 stage 7).
- **Desktop:** the letters on the left and the ladder on the right.
- **Accessibility:** h1 "What you can do next"; h2 for each group; each letter opens in an editable text area with a "Copy" button; keyboard entry at the first letter.

## S11. Before you go

```
+--------------------------------------+
|  Before you go                       |
|                                      |
|  [  Download your report (PDF)  ]    |
|  We can't send it to you later -     |
|  nothing is saved to an account.     |
|                                      |
|  Did the hospital already revise     |
|  this bill?  ( ) Yes ( ) No ( ) N/A  |
|                                      |
|  Help improve bill checks?           |
|  Keep an anonymous copy of the bill  |
|  lines (no names, deleted after 3    |
|  months).                            |
|  [ Yes, keep it ]  [ No thanks ]     |
|                                      |
|  Insured? Want an insurance check    |
|  later?  [ I want this ]             |
|                                      |
|  [ Discard everything now ]          |
+--------------------------------------+
```

- **The download prompt** says plainly that the report cannot be retrieved later (PRD SD-07, RP-03).
- **In-session outcome question** (FB-02).
- **Consent to store de-identified data,** asked after the report, optional and separate (ON-03(c), D19, SD-04–SD-06).
- **An anonymous Insurance Check interest count** (S8, PRD gap G-06).
- **"Discard everything now"** withdraws consent and removes the data during the session (ON-07).
- **Desktop:** the same, centred.
- **Accessibility:** h1 "Before you go"; the download button is the first focus target; the consent buttons are real buttons with explicit labels; keyboard entry at "Download your report".

### Session-wide states

| State | What the user sees |
|---|---|
| Inactivity warning | "Still there? Your session will end in 2 minutes and your documents will be deleted." with [ I'm still here ]. The time value is set later (PRD gap G-10) |
| Session ended | "Your session ended and your documents were deleted. You can start again." |
| Something went wrong | A plain explanation plus a next step; never an error code |

---

## The downloaded report (PDF)

The PDF follows the PRD's seven parts (RP-01):

1. Summary: total, payments, balance due, findings by class, Top 3.
2. Bill Breakdown.
3. Findings by category, with evidence.
4. Line-item appendix.
5. Checks done and checks not possible.
6. Next steps and escalation ladder.
7. Disclaimers, the "hospital billing only" scope statement, and the rule-version stamp.

The Billing Desk Summary is a separate first page that can be printed on its own. Templates and the dispute pack are included (AD-05, Should Have).

## Internal tools (described, not sketched)

These follow your answer to Q4, and each is in its leanest form (`scope-document.md`, section 2.3). All of them sit behind their own strong sign-in.

- **Rule library.** A list of checks, each showing:
  - its status (live, shadow or off);
  - its source reference and version;
  - the date its reference data was last updated, for example NPPA prices.

  Changes record who made them, what changed and why (PRD M19, NFR-16).
- **Labelled Test Set and labelling.** A queue of bills to label. Each bill is labelled independently by two people (you and the founder) before either sees the other's labels or the AI's output. Results per check against the precision targets decide whether a check can go live (PRD M15, LT-01–LT-11, D29).
- **Ops and admin.** Processing-queue status, role-based access, a log of every access to personal data, and a quality dashboard (PRD M20).

## Assumptions & Open Questions

- All names, amounts and bill lines in the sketches are illustrative.
- The session-timeout value (PRD gap G-10) and the value threshold for collapsing low-confidence findings are set in Requirements Analysis.
- The exact wording for deceased-patient and minor declarations is unreviewed, because there is no legal review before launch (PRD D27).
