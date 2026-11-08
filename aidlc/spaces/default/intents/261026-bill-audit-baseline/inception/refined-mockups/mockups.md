# Refined Mockups — Hospital Bill Check (Project RS, Phase 1)

These mockups refine the rough sketches (`ideation/rough-mockups/wireframes.md`, S1–S11) into the screens the build implements. Each screen gives:

- the phone layout, drawn;
- the desktop layout, described in words (project rule);
- the exact wording;
- every state: empty, loading, error, partial and success;
- notes on focus order and screen-reader text;
- the user stories it delivers.

This version also fixes all 14 points raised in the first check: the line editor (M6a), the save reminder in the Discard dialog, the "Who sees it" privacy line, the idle-timer pause rules and ten label and wording mismatches.

Component names, timing and behaviour are in `interaction-spec.md`. Tokens and components are in `design-system-mapping.md`. The accessibility checks are in `accessibility-checklist.md`.

A clickable version of the main journey is in `prototype.html` in this folder. Open it in any browser; it needs no internet connection [Q4], [Q12].

All names, amounts and bill lines are **made up** for illustration. No real bill is shown or used anywhere.

## Sources

- [Q1]–[Q12]: answers in `refined-mockups-questions.md` in this folder.
- Rough mockups: `ideation/rough-mockups/wireframes.md` and `user-flow.md` (S1–S11, approved).
- User stories: `inception/user-stories/stories.md`. The US and AC IDs below refer to it, as do the stage decisions D-US1 to D-US23.
- Requirements: `inception/requirements-analysis/requirements.md` (FR and NFR IDs).
- Team practices: `inception/practices-discovery/team-practices.md` (no real bills; synthetic data only).
- PRD §9.3 (billing desk design rules) and §13.1 (finding classes and "Who to ask"), read through the document library (id 01a17987).

## Changes from the rough mockups

| Rough | Refined | Why |
|---|---|---|
| S7 "Fix a line" had no screen | M6a "Your bill lines" lists every line as read with its standard description and an edit form; the report regenerates in place | US3.5 |
| S1 Start and S2 Privacy were separate screens, with S3 "When are you checking?" as its own screen | M1 combines start and privacy; M2 holds consent, relationship and declarations; "When are you checking?" moves to the top of M3 Upload | At most 2 screens before upload (D-US15); the stay question sits on the upload screen (D-US12) |
| S2 relationship labels "My parent", "My child" | "Whose bill is this?" with "My parent's", "My child's" and so on, plus "Is the patient under 18?" | AC1.5.1, AC1.5.4 |
| S5 "First things worth a look: 3" with "See the full check" | Topics only, no class or amount; the button reads "Confirm a few details" | D-US14, FR7.5 |
| S7 and S9 to S10 as separate screens | One report page with "Jump to" links; the Billing Desk Summary also opens full screen | [Q8] |
| S7 desktop: two columns with the Breakdown on the left | Desktop: findings list on the left, the selected finding's detail on the right | [Q9] |
| S9 Billing Desk Summary built from "Will ask" only | "Will ask" findings, or the Top 3 when nothing is marked | D-US22 |
| S11 "Did the hospital already revise this bill?" | Removed; replaced with "What do you plan to do next?" | FR12.2 |
| S11 "Keep an anonymous copy" | "Keep the bill lines, with names, numbers and addresses removed" — the word "anonymous" is not used | AC7.2.7 |
| Inactivity warning "I'm still here" | "Keep going" at 25 minutes; the timer pauses while a report is prepared, for at most 15 minutes | FR2.8, D-US13, [Q1] |
| Accessibility basis WCAG 2.1 AA | WCAG 2.2 AA | Requirements Analysis Q8 |

## Common to every screen

```
+--------------------------------------+
| Project RS      [ Discard everything ]|  <- header (quiet button)
| ●Upload ─ ○Review ─ ○Report          |  <- ProgressSteps (on M3–M10)
|--------------------------------------|
|                                      |
|   ...screen content...               |
|                                      |
|--------------------------------------|
| Information only. Not legal, medical |
| or financial advice.                 |  <- footer, every screen
| What this check is not · Privacy     |  <- footer links, every screen
+--------------------------------------+
```

- **Footer links.** "What this check is not" and "Privacy" open the same text as on M1, in a dialog, without leaving the current screen. They sit in the same place on every screen (WCAG 3.2.6).

- **Skip link.** "Skip to content" is the first focus stop on every screen (AC7.8.4).
- **Step indicator.** Read aloud as "Step 2 of 3: Review, current step" (AC3.2.7).
- **"Discard everything"** is in the header from M2 onwards. It opens the Discard dialog (US7.3).
- **Footer.** The disclaimer is always present (FR13.1).
- **Desktop.** The header and footer span the full width; the content is a centred 640 px column, except on M6 (see there).

---

## M1. Start and what this does

*Screen 1 of 2 before upload. Delivers US1.1, US1.2, US1.3. Rough S1 + S2 notice.*

```
+--------------------------------------+
| Project RS                           |
|--------------------------------------|
|  Is your hospital bill right?        |  h1
|                                      |
|  We read your bill line by line and  |
|  show what may be worth asking the   |
|  hospital about, with the evidence.  |
|                                      |
|  ✓ Free. No sign-up.                 |
|  ✓ Your files are deleted after we   |
|    read them.                        |
|  ✓ A few minutes of your time, plus  |
|    a short wait while we read the    |
|    bill (usually under 15 minutes).  |
|                                      |
|  What this check is not              |  h2
|  • Not legal, medical or financial   |
|    advice. It is information only.   |
|  • It does not promise any outcome.  |
|  • It does not check what insurance  |
|    will pay.                         |
|  • You decide what to say to the     |
|    hospital.                         |
|                                      |
|  Your privacy, in short              |  h2
|  • What we collect: the documents    |
|    you upload, for this check only.  |
|  • Why: to read the bill and find    |
|    things worth asking about.        |
|  • How long: files are deleted once  |
|    read; everything else when you    |
|    leave, unless you choose to share |
|    at the end.                       |
|  • Who sees it: only the services we |
|    use to read your bill and run     |
|    this site, under contract. They   |
|    may not keep your files or use    |
|    them to train their systems. We   |
|    never send anything to the        |
|    hospital or an insurer.           |
|  • Your rights: you can stop and     |
|    delete everything at any time.    |
|  [ More detail ▾ ]                   |
|                                      |
|  [        Start my check        ]    |  primary, full width
|  [ Leave ]                           |  quiet
|--------------------------------------|
| Information only. Not legal, medical |
| or financial advice.                 |
+--------------------------------------+
```

- **Wording rules.**
  - The rough "Nothing about you is saved" is replaced with "Your files are deleted after we read them". It is accurate whatever the user later chooses about storage (FR1.12; the founder or counsel still approves the start-screen text, AC1.3.4).
  - "Who sees it" names the data processors in plain words: the bill-reading service and the hosting provider, under contract, with no keeping and no training (AC1.3.1, AC7.5.7, NFR19). This line is added to the wording the founder or counsel must approve.
  - "A few minutes of your time, plus a short wait" replaces the rough "About 10 minutes". The wait is tied to the report target of 15 minutes at the 90th percentile (NFR2).
  - The four disclaimer statements match AC1.2.1 word for word.
- **"More detail"** expands the full privacy notice in place; it does not navigate away (AC1.3.2).
- **"Leave"** takes the user to a plain "You can close this page. Nothing was uploaded." view (AC1.2.3).
- **Desktop.** The content sits in a single column. A three-step strip ("Upload your bill → Confirm a few details → Get your report") sits above the "What this check is not" section.
- **Focus order.** Skip link → h1 → "More detail" → "Start my check" → "Leave".

| State | What the user sees |
|---|---|
| Default | As drawn |
| "More detail" open | The full notice below the summary; the button reads "Less detail ▴" |
| Arrived by a deep link to a later screen | This screen, with no message (AC1.1.4) |
| Page failed to load fully | A `SystemMessage`: "Something went wrong loading this page. Please try again." with [ Try again ] |

## M2. Consent and whose bill

*Screen 2 of 2 before upload. Delivers US1.4, US1.5, US1.6, US1.7; beta variant US9.5.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
|--------------------------------------|
|  Before you upload                   |  h1
|                                      |
|  Whose bill is this?                 |  legend
|  ( ) Mine                            |
|  ( ) My spouse's                     |
|  ( ) My parent's                     |
|  ( ) My child's                      |
|  ( ) Another family member's         |
|  ( ) I am an authorised              |
|      representative                  |
|  ( ) I am a professional helping     |
|      the family                      |
|                                      |
|  Is the patient under 18?            |  appears for anyone
|  ( ) No    ( ) Yes                   |  other than "Mine"
|                                      |
|  [ ] I am allowed to share this      |
|      patient's documents.            |
|                                      |
|  [ ] I agree that you may read these |
|      documents for this check only.  |
|                                      |
|  [         Continue         ]        |
+--------------------------------------+
```

**Declarations.** These appear in the same screen, under "Is the patient under 18?", only when they apply:

| Answer | Declaration shown (checkbox) |
|---|---|
| Under 18: Yes | "I am the patient's parent or legal guardian." (US1.6) |
| Under 18: No, and not "Mine" | Three choices: "The patient has agreed to me checking this bill"; "The patient cannot decide right now, and I am acting for them"; "My [spouse / parent / child / family member] has passed away" (US1.7) |
| "Has passed away" chosen | A short line: "We're sorry for your loss. You only need to tick the box above; no documents are needed." No use of "deceased", "legal heir", "death certificate" or "proof" (AC1.7.2) |

- **Errors** (AC1.5.2). The user stays on the screen. A text message appears under each missing item, for example "Please choose whose bill this is" or "Please tick this box to continue". Focus moves to the first missing item, and the message is announced. "Continue" is never disabled; it explains instead.
- **Declining consent** (AC1.4.3). Leaving the consent box unticked and pressing "Continue" shows the message "We can only check the bill if you agree that we may read it". It is followed by [ Go back ] and [ Leave ], and nothing is uploaded.
- **Beta variant** (US9.5). An extra checkbox appears above "Continue": "I agree that my original bill is kept for testing this product. It is stored separately, and names and numbers are blacked out." Upload is blocked until it is ticked.
- **Desktop.** One column; the declarations appear directly under the question that triggers them.
- **Focus order.** h1 → relationship group → under-18 group (when shown) → declaration (when shown) → authorisation box → consent box → "Continue".

| State | What the user sees |
|---|---|
| Default | As drawn, with no answer selected |
| Missing answer | The text error next to each missing item (above) |
| Consent declined | The declined message with [ Go back ] [ Leave ] |
| Request sent without consent (no UI) | Refused on the server; the next screen load shows M2 again (AC1.4.2) |

## M3. Add your documents

*Delivers US2.1–US2.11 (stay question US2.11), US2.9 checklist, US7.5 deletion notice. Rough S3 + S4.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
| ●Upload ─ ○Review ─ ○Report          |
|--------------------------------------|
|  Add your documents                  |  h1
|                                      |
|  When are you checking?              |  legend, answered first
|  ( ) Still in hospital               |
|  ( ) At discharge                    |
|  ( ) Already paid                    |
|                                      |
|  [ Take a photo ] [ Choose files ]   |  disabled until answered,
|  Got the bill on WhatsApp? Save it   |  with text reason below
|  to your phone first, then choose it.|
|                                      |
|  Your documents                      |  h2
|  ✓ hospital-bill.pdf   3 pages  Done |
|  ↻ pharmacy-1.jpg      Uploading 62% |
|  ! page-4.jpg   Blurry — retake it   |
|      [thumbnail]  [ Retake ]         |
|                                      |
|  What we still need                  |  h2 (DocChecklist)
|  ✓ Hospital bill — added             |
|  ! Itemised bill — needed            |
|    This looks like a summary bill.   |
|    [ Get a request letter ]          |
|  ○ Pharmacy bills — optional         |
|  ○ Discharge summary — optional,     |
|    lets us check more                |
|  ○ Package letter or estimate        |
|  ○ Payment receipts                  |
|                                      |
|  Files are deleted once we've read   |
|  them.                               |
|                                      |
|  [        Check my bill        ]     |  sticky action bar
+--------------------------------------+
```

- **Stay question** (AC2.11.1). "Take a photo" and "Choose files" are disabled until it is answered. The line "Please answer 'When are you checking?' first" sits beside them and is announced when they are pressed.
- **Checklist by answer.**
  - "Still in hospital": interim bill only.
  - "At discharge": adds "Earlier interim bills, so we can spot amounts counted twice".
  - "Already paid": adds payment receipts (US2.9).
- **"Take a photo"** opens the `CameraTips` sheet first (AC2.2.1).

  ```
  +--------------------------------------+
  |  Before you take the photo           |
  |  • Lay the page flat                 |
  |  • Avoid glare from lights           |
  |  • Keep all four edges in the frame  |
  |  [ ] Don't show these tips again     |
  |  [        Open camera        ]       |
  +--------------------------------------+
  ```

- **Limits** (US2.3). Each message names the limit and the next step:
  - "That's more than 20 files. Remove a file to add another."
  - "This file is over 25 MB. Save it as a smaller PDF or a photo and try again."
  - "That would take you past 60 pages. Remove some pages to add this file."
- **Desktop.** The stay question and the buttons sit at the top. Below them, a drop area on the left reads "Drop files here, or [ Choose files ]" (the button is always present, so dragging is never required), and "Your documents" and "What we still need" are on the right. "Take a photo" is hidden on desktop.
- **Focus order.** h1 → stay question → "Take a photo" → "Choose files" → file list rows (each with its action) → checklist actions → "Check my bill".

| State | What the user sees |
|---|---|
| Empty | The stay question, the two buttons, and "Start with the hospital bill" in place of the file list |
| Uploading | Per-file progress in text ("Uploading 62%"), announced at start, finish and failure only (AC2.10.5) |
| Weak connection | "Weak connection — your upload is paused and will continue when you're back online." (AC2.10.3) |
| Page reloaded during upload | "These files were received: … Please choose the files that didn't finish: …" (AC2.10.4) |
| Unreadable page | "Page 4 of the pharmacy bill is blurry. Please retake it." with a small preview from the device and [ Retake ] (AC2.6.1) |
| Not a hospital document | "This doesn't look like a hospital document." with [ Remove ] [ Keep it ] (AC2.4.3) |
| Wrong type | Each file row has "Type: Hospital bill [ Change ]" (AC2.4.2) |
| Missing page | "Page 3 of 7 seems to be missing." with [ Add it ] (AC2.5.1) |
| Duplicate page | "You've added page 2 twice. We'll count it once." (AC2.5.3) |
| Password-protected PDF | A row with "This PDF is locked. Enter its password:" [ field ] [ Open ]. After the last allowed attempt: [ Skip this file ] (US2.8) |
| Unsupported format | "We can't read .docx files. Save it as a PDF or a photo and try again." (AC2.1.4) |
| Upload failed | `SystemMessage`: "That file didn't upload. Nothing you added is lost." [ Try again ] |

## M4. Reading your bill, and first look

*Delivers US3.9, US3.3, US3.2 progress. Rough S5.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
| ✓Upload ─ ●Review ─ ○Report          |
|--------------------------------------|
|  Reading your bill                   |  h1 (changes to "First
|                                      |   look" when ready)
|  ✓ Reading pages                     |
|  ↻ Running checks                    |
|  ○ Report ready                      |
|  About 3 minutes left. You can stay  |
|  on this page.                       |
|--------------------------------------|
|  First look — may change after you   |  h2
|  confirm a few details               |
|                                      |
|  Total billed            ₹2,48,600   |  real table
|  Advances paid           ₹1,50,000   |
|  Balance due               ₹98,600   |
|                                      |
|  Largest parts of the bill           |
|  Room charges              ₹62,000   |
|  Pharmacy                  ₹48,300   |
|  Consumables               ₹31,900   |
|                                      |
|  3 things worth a look:              |
|  • Room days                         |  topics only: no class,
|  • A scan that appears twice         |  no rupee amount
|  • A drug price                      |  (D-US14)
|                                      |
|  [    Confirm a few details    ]     |
+--------------------------------------+
```

- **Progress** uses three milestones, and only these are announced (AC3.2.6). The estimate is shown in minutes.
- **Desktop.** The first-look figures sit in a row of three tiles above the two lists.

| State | What the user sees |
|---|---|
| Reading, no first look yet | The milestones and estimate only |
| Longer than estimated | "Taking longer than usual. Still working." No stalled bar (AC3.2.5) |
| 15-minute cap reached | "This is taking too long. Your documents are still here." with [ Try again ] and [ Discard everything ]. From this moment the idle timer runs again. "Try again" starts a new job with its own 15-minute cap [Q1] |
| A step failed | "We couldn't finish reading your bill. What we've done so far is kept." with [ Try again ] (AC3.9.3) |
| Nothing worth a look | "Nothing stood out on a first look. Confirm a few details for the full check." |

## M5. Confirm a few details

*Delivers US3.1, US3.4, US3.5 (pre-report), US3.6, US3.7, US2.11 confirmation. Rough S6.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
| ✓Upload ─ ●Review ─ ○Report          |
|--------------------------------------|
|  Please check a few things           |  h1
|                                      |
|  About this stay                     |  h2
|  Hospital  [ City Care Hospital   ]  |
|  City      [ Pune                 ]  |
|  Admitted  [ 03/10/2026 ] 10:40 IST  |
|  Discharged[ 09/10/2026 ] 12:15 IST  |
|  Treatment ( ) Planned (•) Emergency |
|  Paying by ( ) Myself  ( ) Claiming  |
|            later  ( ) Cashless       |
|  Insurer   [ — Please fill in —   ]  |
|  When      (•) At discharge  [Change]|
|                                      |
|  Your answer and the documents       |  only if they disagree
|  differ: you said "Still in          |  (AC2.11.3)
|  hospital", but we found a final     |
|  bill with receipts. Which is right? |
|                                      |
|  We weren't sure about 2 lines       |  h2
|  Pharmacy bill, page 4, line 12      |
|  "Inj. ....rone 1g"                  |
|  Quantity [ 6 ]   Amount [ 1,840 ]   |
|  [ This is right ]                   |
|                                      |
|  Could not be read (1)               |  h2
|  Hospital bill, page 5, line 3       |
|  This line isn't in English, so we   |
|  left it out of the checks.          |
|  [ Type it in ]  [ Leave it out ]    |
|                                      |
|  [   Confirm and see my report   ]   |
+--------------------------------------+
```

- **Mismatch notes** (AC3.7.1, AC3.7.2). They appear under "About this stay" when documents disagree, for example: "The UHID on the discharge summary is different from the bill. Please check you uploaded the right papers."
- **All lines** (US3.5). A "See all lines" link under the unclear lines opens M6a, where any line can be checked or fixed before the report.
- **Low confidence** (AC3.4.1). The note "We weren't sure" is in text, not only a coloured border.
- **Desktop.** "About this stay" is on the left and the unclear lines are on the right. Each unclear line can show a bill-line image, but only one drawn from the file still on this device (AC4.9.8).
- **Focus order.** h1 → stay fields in order → each unclear line → each unread line → "Confirm and see my report".

| State | What the user sees |
|---|---|
| Nothing unclear | Only "About this stay" and the button |
| Field missing from documents | The field is empty and marked "Please fill in" (AC3.1.3) |
| Unclear line left unconfirmed | Pressing the button shows "1 line still needs a look. Findings that depend on it will be left out." with [ Look now ] [ Continue anyway ] (AC3.4.3) |

## M6. Your bill check (the report)

*One page [Q8]. Delivers US4.9–US4.14, US5.1, US5.2, US5.5, US6.1, US6.4, US6.5, US3.5 (after report), US6.7. Rough S7.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
| ✓Upload ─ ✓Review ─ ●Report          |
|--------------------------------------|
|  Your bill check                     |  h1
|  City Care Hospital · Bill 4471      |
|  ₹2,48,600 billed                    |
|                                      |
|  [ Save or share ]                   |  SaveBar, above the fold;
|  Save a copy now. We can't send it   |  "Download" on desktop (AC5.4.7)
|  to you later.                       |
|                                      |
|  Top 3 things to do now              |  h2
|  1 Room charged for an extra day     |
|    Ask the hospital        ₹4,000    |
|  2 The same CT scan listed twice     |
|    Ask the hospital        ₹2,600    |
|  3 A drug charged above its MRP      |
|    Ask the hospital          ₹820    |
|                                      |
|  Bill Breakdown                      |  h2, table + bar
|  ▲ Likely error   3 items  ₹7,420   |  amber
|  ? Worth asking   4 items ₹12,300   |  blue
|  ✓ Explained          ₹2,21,180     |  grey
|  ○ Could not be assessed   ₹7,700   |
|                                      |
|  Jump to: Findings · Billing desk    |  nav "On this page"
|  summary · Letters · Next steps      |
|                                      |
|  Findings                            |  h2
|  [All][Likely error][Worth asking]   |  FindingFilter
|  [Explained]                         |
|  ┃▲ Likely error · High confidence   |
|  ┃ Room charged for an extra day     |
|  ┃ ₹4,000 · Ask the hospital     >   |
|  ┃▲ Likely error · High confidence   |
|  ┃ The same CT scan listed twice     |
|  ┃ ₹2,600 · Ask the hospital     >   |
|  ┃? Worth asking · Medium confidence |
|  ┃ 120 pairs of gloves for 6 days    |
|  ┃ ₹3,100 · Ask the hospital     >   |
|  ┃? Worth asking · High confidence   |
|  ┃ The stent invoice is missing      |
|  ┃ ₹0 · You can sort this out    >   |
|  ...                                 |
|  [ Other things you may ask about    |  LowConfidenceGroup
|    (5) ▾ ]                           |
|                                      |
|  Checks we couldn't run              |  h3
|  Still being tested: Implant price   |
|  cap                                 |
|  Needs a document you can add:       |
|  Package price — needs the package   |
|  letter  [ Add a document ]          |
|                                      |
|  [ Fix a line ]  [ Add a document ]  |
|                                      |
|  ── Billing desk summary ──          |  anchor, see M8
|  ── Letters and next steps ──        |  anchor, see M9
|                                      |
|  What do you plan to do next?        |  h2 (FR12.2); options
|  ( ) Ask the billing desk today      |  vary by the stay answer
|  ( ) Ask later, after discharge      |
|  ( ) Pay and raise it afterwards     |
|  ( ) Not sure yet                    |
|                                      |
|  [        I'm done        ]          |  goes to M10
+--------------------------------------+
```

- **Ordering** (AC4.9.7). "Likely error" findings come first, then "Worth asking", then "Explained"; within a class, the larger amount comes first.
- **Names and IDs** (AC4.9.6). Findings use plain-language names. The check ID (for example LOS-01) appears only inside the finding detail and the report appendix.
- **Low confidence** (AC4.12.4, D-US20). Low-confidence findings never appear in the Top 3, and their amounts count under "Worth asking" in the Breakdown.
- **Route labels** (AC4.13.1). Cards and details use only the four labels, word for word: "Ask the hospital", "Explained — no billing concern", "Complex — consider professional advice", "You can sort this out". The missing stent invoice above is the "You can sort this out" example (AC4.13.4).
- **Save label** (AC5.4.7). The SaveBar reads "Save or share" on phones and "Download" on desktop.
- **Plan options by stay answer** (FR12.2):
  - "Still in hospital" or "At discharge": as drawn.
  - "Already paid": "Ask the hospital for a revised bill or refund", "Raise it with patient relations", "Leave it for now", "Not sure yet".
  - Domain Design may still adjust the categories.
- **Desktop** [Q9]:
  - The title, the SaveBar, the Top 3 and the Breakdown sit across the top, in a 1200 px container.
  - Below them, the findings list (400 px) is on the left, and the selected finding's detail is on the right (M7). The first finding is selected by default.
  - "Jump to" stays visible above the list.
- **Focus order.** h1 → "Save or share" → Top 3 items → Breakdown table → "Jump to" links → filter → finding cards → low-confidence group → checks not run → "Fix a line" → "Add a document" → summary → letters → plan question → "I'm done".

| State | What the user sees |
|---|---|
| Full check still running | The parts that are ready, with the rest marked "Still checking". When the report is ready, "Full report ready" is announced (AC5.2.6) |
| No findings | "We found nothing that looks wrong in what you gave us." It is followed by what we checked and the checks we couldn't run. This makes clear that nothing found does not mean fully checked (AC4.11.3) |
| Summary bill only | "We could only check part of your bill, because it's a summary bill." Followed by [ Get a request letter ] and the list of checks that need the itemised bill |
| Regenerating after "Fix a line" (M6a) or "Add a document" | The report stays on screen with "Updating your report…" at the top (polite announcement); findings that may change are marked "Updating". The user is not sent back to M4 |
| After regenerating | A `ChangeNotice` at the top, for example: "Your report was updated. 1 finding removed. Total potentially affected changed by ₹1,840." If a "Will ask" finding was removed, the notice names it. Focus moves to the notice. Marks and ratings are kept (AC3.5.4, AC3.5.5, AC6.7.4) |
| Partial failure | "Some checks didn't finish." Followed by the list of checks and [ Try again ] (AC4.14.4) |

## M6a. Your bill lines (Fix a line)

*Reached from "Fix a line" on M6 and from "See all lines" on M5. Delivers US3.5 (AC3.5.1–AC3.5.5).*

```
+--------------------------------------+
|  < Back to your report               |
|  Your bill lines                     |  h1
|  Find a line: [ search text      ]   |
|  Show: [All][Changed by you]         |
|                                      |
|  Hospital bill · page 2              |  h2 per document page
|  ┌──────────────────────────────────┐|
|  │ Line 4                            ||
|  │ As read:  "ROOM CHG - SEMI PVT"   ||
|  │ Means:    Room charge, semi-      ||
|  │           private room            ||
|  │ Qty 7 · Rate ₹4,000 · ₹28,000     ||
|  │ [ Fix this line ]                 ||
|  └──────────────────────────────────┘|
|  ┌──────────────────────────────────┐|
|  │ Line 5  · Changed by you          ||
|  │ As read:  "NURSING CHG"           ||
|  │ Means:    Nursing charge          ||
|  │ Qty 6 · Rate ₹1,000 · ₹6,000      ||
|  │ [ Fix this line ]  [ Undo ]       ||
|  └──────────────────────────────────┘|
+--------------------------------------+
```

**Edit form** (opens in place under the line when "Fix this line" is pressed):

```
|  Fix line 4                          |  h3
|  Quantity  [ 7      ]                |
|  Rate (₹)  [ 4,000  ]                |
|  Amount(₹) [ 28,000 ]                |
|  [ Save change ]  [ Cancel ]         |
```

- **What can be changed:** quantity, rate and amount. The original text and the standard description are shown but are not edited here; a wrong description is reported through the finding rating.
- **Validation:** numbers only, with Indian grouping accepted ("28,000"). An empty field or text shows "Please enter a number, for example 4,000" under the field, focus moves there, and it is announced.
- **Saving a change** returns the user to the report (M6), which shows "Updating your report…" and then the `ChangeNotice`. Focus moves to the notice. The corrected line shows "Changed by you" on M6a and in the report appendix (AC3.5.3).
- **Undo** restores the value as read and regenerates the report the same way.
- **Desktop.** The lines sit in a single centred column, with the edit form in place under the line, as on the phone. The page headings group the lines.
- **Focus order.** "Back to your report" → search → filter → each line's "Fix this line" in order. "Cancel" returns focus to that line's "Fix this line".

| State | What the user sees |
|---|---|
| Default | All lines, grouped by document and page |
| No matches for the search | "No lines match ‘…’." with [ Clear search ] |
| Invalid number | The text error under the field |
| Line could not be read (US3.6) | "This line isn't in English, so we left it out of the checks." with [ Type it in ] |
| Saving failed | `SystemMessage`: "Your change wasn't saved. Please try again." Nothing else changes |

## M7. Finding detail

*Phone: its own view. Desktop: the right-hand panel of M6. Delivers US4.9, US4.13, US6.1, US6.4. Rough S8.*

```
+--------------------------------------+
|  < Back to findings                  |
|  Room charged for an extra day       |  h1 (h2 in the desktop
|  ▲ Likely error · High confidence    |   panel)
|  Who to ask: Ask the hospital        |
|                                      |
|  What it is                          |  h2
|  The daily charge for the hospital   |
|  room.                               |
|                                      |
|  What we noticed                     |  h2
|  7 room days billed (Hospital bill,  |
|  page 2, line 4). The discharge      |
|  summary says admitted 03 Oct,       |
|  discharged 09 Oct: 6 days.          |
|  [ See the bill line ]               |  only if the file is still
|                                      |  on this device (AC4.9.8)
|  Rule or reference: room days are    |
|  counted by the hospital's day rule  |
|  (midday to midday).                 |
|  Why it may be wrong                 |  h2
|  One more day than the stay appears  |
|  to have been charged.               |
|  Amount potentially affected: ₹4,000 |
|                                      |
|  What to do                          |  h2
|  Ask the billing desk to check the   |
|  number of room days. You can ask    |
|  today or after paying.              |
|  What to attach: the discharge       |
|  summary.                            |
|  By when: before you pay if you can; |
|  otherwise within 7 days.            |
|                                      |
|  Will you raise this?                |
|  [ Will ask ]  [ Not pursuing ]      |  MarkToggle
|                                      |
|  Was this helpful?                   |
|  ( ) Helpful ( ) Not helpful         |
|  ( ) Wrong                           |
|  Reason (optional) [ Choose…     ▾ ] |  fixed reasons only
|                                      |
|  Details: check LOS-01 · rule v1.3 · |  Small text
|  reference data 2026-10              |
+--------------------------------------+
```

- **Fixed reasons** (User Stories Q25):
  - "The hospital already explained this";
  - "The amount is wrong";
  - "This isn't on my bill";
  - "I don't understand it".
- **"Complex" findings** show, under "Who to ask": "Complex — consider professional advice". "What to do" then reads: "This check can't judge this. You could ask the treating doctor or the hospital's patient relations team to explain it." No clinical opinion is given (AC4.13.5).
- **"Explained" findings** show "Explained — no billing concern", with only "What it is" and "What we noticed".
- **"You can sort this out" findings** (for example the missing stent invoice) say what document to get and where it is usually found. "What to do" reads: "Ask the hospital pharmacy or billing desk for the implant invoice or sticker, then add it with 'Add a document'." (AC4.13.4)
- **Every field of AC4.9.1 is shown:**
  - the rule or regulatory reference, as its own line;
  - "What to attach" and "By when", as their own lines in "What to do" (AC4.9.2);
  - the check ID, in the small "Details" line.

  The "By when" deadlines are illustrative until the reference data is loaded.
- **Desktop.** The panel sits beside the list. Choosing another finding replaces the panel, and focus moves to its heading. Esc or "Back to findings" returns focus to the list item (AC7.8.9).
- **Focus.** It moves to the h1 when the view opens and returns to the originating card on "Back".

## M8. Billing Desk Summary

*A section of M6 and a full-screen view. Delivers US5.3, US6.1. Rough S9.*

```
+--------------------------------------+
|  Billing Desk Summary                |  h2 in M6; h1 full screen
|  City Care Hospital, Pune            |
|  Bill no. 4471, dated 09 Oct 2026    |
|                                      |
|  Please correct                      |  h3
|  1 Page 2, line 4 — Room days: 7     |
|    billed, stay was 6 days. ₹4,000   |
|  2 Page 6, lines 9 and 11 — CT scan  |
|    listed twice.             ₹2,600  |
|  3 Page 8, line 2 — Price above the  |
|    printed MRP.                ₹820  |
|                                      |
|  Please explain                      |  h3
|  4 Page 5, line 7 — 120 pairs of     |
|    gloves for a 6-day stay.          |
|                                      |
|  Thank you for checking these.       |
|                                      |
|  [ Show full screen ] [ Save or share ]
+--------------------------------------+
```

- **Content rule** (D-US22):
  - It holds the findings marked "Will ask".
  - When nothing is marked, it holds the Top 3, with the line "You can choose which points to raise", linking back to the findings.
  - "Not pursuing" and low-confidence findings are never included.
  - In the downloaded PDF, the "Will ask" checklist section says "You didn't choose any points to raise." when nothing is marked (AC6.1.3).
- **Wording** (S1 stakeholder rules). No confidence labels, no class names and none of the banned words (AC5.3.3).
- **Overflow.** When the items don't fit on one A4 page, the PDF keeps them in ranking order and ends with "3 more points are in the full report" (AC5.3.4).
- **"Show full screen"** (AC5.3.5). A full-screen view with only the summary, in 24 px text, and a [ Close ] button. It still works if the connection drops (AC5.3.6).
- **Desktop.** Shown at A4 proportions, centred, ready to print.

## M9. Letters and next steps

*A section of M6. Delivers US5.6, US5.7, US5.8, US5.9, US6.2, US6.3. Rough S10.*

```
+--------------------------------------+
|  Letters you can edit and send       |  h2
|  (Already paid: this one is first)   |
|  ▸ Request a revised bill or refund  |
|  ▸ Ask for the itemised bill         |
|  ▸ List of billing questions         |
|  ▸ Ask for the implant invoice and   |
|    package inclusions                |
|  ▸ Grievance escalation letter       |
|  ▸ How to complain to the NPPA about |  only with a price finding
|    a drug or device price            |
|                                      |
|  ▾ List of billing questions         |  opened letter
|  [ Dear Billing Manager,          ]  |  LetterEditor (textarea)
|  [ Please could you check ...     ]  |
|  [                                ]  |
|  Information only. Not legal advice. |  disclaimer kept outside
|  [ Copy ]  [ Download ]              |  the editable area
|                                      |
|  If the billing desk can't help      |  h2
|  1 Billing desk — today              |
|  2 Patient relations — within 7 days |
|  3 Medical superintendent or         |
|    grievance officer — within 15 days|
|  4 State health authority, where     |
|    applicable                        |
|  5 NPPA, for drug and device prices  |
|  6 Consumer Commission               |
|  You can raise these even after the  |
|  bill is paid. You don't need to     |
|  delay payment or discharge.         |
|                                      |
|  We never contact the hospital or    |
|  anyone else for you.                |
|                                      |
|  [ ] Add a dispute pack to my        |
|      download (findings, evidence,   |
|      letters and this ladder)        |
|                                      |
|  What to do next time                |  h3 ("Tips for the family"
|                                      |   when the patient has died)
|  • Ask for an estimate in writing    |
|    before a planned admission.       |
|  • Keep all receipts together.       |
+--------------------------------------+
```

- **Deadlines** come from the hospital-side timeline reference data. The values shown are illustrative. All times are labelled IST (AC5.5.3).
- **Reassurance line.** It appears in every report, whether or not the bill is paid (AC5.8.3).
- **Bereavement wording** (AC1.7.4, AC5.9.3).
  - The heading is "What to do next time" (AC5.9.1).
  - When the patient has passed away, the heading becomes "Tips for the family", and the tips read as general advice for the family. They never mention the patient's future care.
- **Desktop.** Letters on the left, the ladder on the right.

## M10. Before you go

*Delivers US7.1, US7.2, US6.6, US7.3. Rough S11.*

```
+--------------------------------------+
| Project RS     [ Discard everything ]|
| ✓Upload ─ ✓Review ─ ●Report          |
|--------------------------------------|
|  Before you go                       |  h1
|                                      |
|  [     Save or share your report  ]  |  first focus target;
|                                      |  "Download your report"
|                                      |  on desktop
|  We can't send it to you later.      |
|  Nothing is saved to an account.     |
|                                      |
|  Help improve bill checks?           |  h2
|  We'd keep the bill lines with names,|
|  numbers and addresses removed, and  |
|  delete them after 3 months.         |
|  [ Yes, keep it ]  [ No thanks ]     |  equal size and weight
|                                      |
|  Want an insurance check later?      |  only for cashless or
|  [ I want the Insurance Check ]      |  claiming later
|  We won't ask for any contact        |
|  details.                            |
|                                      |
|  [            Finish            ]    |
+--------------------------------------+
```

- **Storage question** (US7.2). Not answering counts as "No thanks" (AC7.2.6). The wording never uses "anonymous" (AC7.2.7). The founder still approves the "before you go" wording before launch (AC1.3.4).
- **Open privacy item** (R-04). The risk that a rare procedure at a small hospital could point to one patient is carried to Domain Design [Q3]. This screen's wording may change once that is settled.
- **"Finish"** with no saved copy shows the reminder: "You haven't saved your report. We can't send it to you later." with [ Save or share ] (desktop: [ Download ]) and [ Finish without saving ] (AC7.1.1).
- **After "Finish"**: "Done. Your documents and this check have been deleted. If you saved the report, it's on your device." with [ Start a new check ] (AC7.1.5).
- **"Start a new check" order** (AC7.1.5). It is only offered after the old session has ended. From M10 that happens after the "Finish" reminder; from the "Session ended" view the session is already gone. In both cases there is nothing left to save, so no second reminder is shown.
- **Desktop.** Single centred column.

---

## Session-wide states

| State | Wording | Notes |
|---|---|---|
| Idle warning (at 25:00 idle) | "Are you still there? In 5 minutes your session will end and your documents will be deleted." [ Keep going ] | `IdleWarning` dialog; focus on "Keep going"; announced (AC7.4.7) |
| Timer paused | (nothing shown) | While a report is being prepared, for at most 15 minutes [Q1] |
| Session ended | "Your session ended and your documents were deleted. If you saved the report, it is still on your device." [ Start a new check ] | No error code (AC7.4.8) |
| Discard dialog, before a report exists | "Discard everything from this check? Your documents and choices will be deleted now. This can't be undone." [ Discard everything ] [ Cancel ] | Destructive button in red; focus starts on "Cancel" (US7.3) |
| Discard dialog, report exists and is not saved | "You haven't saved your report. Once you discard, it can't be retrieved. Your documents, findings and choices will be deleted now." [ Save or share first ] (desktop: [ Download first ]) [ Discard everything ] [ Cancel ] | AC7.1.1. "Save or share first" saves, then returns to the dialog so the user can still choose to discard; focus starts on "Save or share first" |
| Discard dialog, report saved | As "before a report exists", with "Your saved copy stays on your device." added | — |
| Leave without saving (reload or close) | The browser's own prompt | Its text is set by the browser (AC7.1.3) |
| Out-of-date saved copy | "Your report changed after you saved it. Save it again?" | After a regeneration (AC7.1.4) |
| Something went wrong | "Something went wrong. Your documents are still here." [ Try again ] | Never an error code |

## The downloaded report (PDF)

The PDF is built on the device (D-US9) in the seven parts of FR10.1.

- **Page 1:** the Billing Desk Summary, printable on its own.
- **Page 2:** summary, Top 3, Bill Breakdown and the limitations statement (AC5.2.2).
- **Then:** findings by category with evidence; the line-item appendix; checks performed and not possible; next steps and the escalation ladder; disclaimers, the "hospital billing only" statement and the rule-version stamp.
- **With the dispute pack:** the letters and an evidence index are added.
- **Checklist section:** lists the "Will ask" findings, or says "You didn't choose any points to raise." when nothing is marked (AC6.1.3).
- **Format:**
  - selectable text in reading order;
  - an embedded font that has the ₹ sign;
  - the file name `bill-check-YYYY-MM-DD.pdf`, with no names (AC5.4.6, AC5.4.8, AC5.4.10).

---

## Internal tools

Four screens are sketched, where a mistake matters most [Q11]. The rest are described. All are desktop-first and behind sign-in; the sign-in method is set in NFR Requirements (R-05, [Q3]).

### I1. Sign in

```
+--------------------------------------+
|  Project RS — internal tools         |
|  Sign in                             |  h1
|  Email   [                       ]   |
|  [ Continue with passkey or code ]   |  method per NFR Requirements
|                                      |
|  Every access to personal data is    |
|  logged.                             |
+--------------------------------------+
```

- **Error:** "We couldn't sign you in. Check your details and try again." It never says which part was wrong.
- **Partial method rejected:** "This account needs the second step to sign in." (AC10.2.4)

### I2. Approve a rule change

```
+------------------------------------------------------------------+
|  Approve a rule change                                    h1     |
|  RX-02 ceiling price · Paracetamol 650 mg tablet                 |
|  Changed by: builder · 07 Nov 2026 14:20 IST                     |
|  What:  ₹2.04 → ₹1.98 per tablet                                 |
|  Why:   NPPA notification of 01 Nov 2026                         |
|  Source: [link to notification]                                  |
|                                                                  |
|  Test bills re-run                                        h2     |
|  ✓ 412 synthetic bills re-run · 0 unexpected changes             |
|  Expected change: 2 bills now show a smaller excess (listed)     |
|                                                                  |
|  ( ) You made this change. Approving it yourself is logged as a  |
|      self-approval.                                              |
|  [ Approve ]   [ Send back ]                                     |
+------------------------------------------------------------------+
```

- **Safeguard** [Q2]. The synthetic test bills re-run automatically before "Approve" is available.
- **Unexpected change found:** "3 bills changed unexpectedly", with the list. "Approve" is disabled, with the line "This change can't take effect until the unexpected changes are explained or fixed."
- **Missing fields.** If who, what, why or source is missing, "Approve" is disabled, with the line naming the missing field (AC8.2.3).
- **Self-approval** is allowed and logged (User Stories Q24, AC8.2.4).

### I3. Promote a check to live

```
+------------------------------------------------------------------+
|  Promote a check to live                                  h1     |
|  LOS-01 Room days vs stay · class: Likely error · now: shadow    |
|                                                                  |
|  Latest measurement (07 Nov 2026)                         h2     |
|  Bills where the error is present    12  (needs 10)  ✓           |
|  Bills where it is not               34  (needs 30)  ✓           |
|  Precision                        91.7%  (needs 90%) ✓           |
|  1 bill since withdrawn (counts kept)                            |
|                                                                  |
|  [ Make live ]                                                   |
+------------------------------------------------------------------+
```

- **Not ready:** each failing row shows ✗ and its shortfall, for example "9 (needs 10)". "Make live" is disabled with the reason in text (AC10.1.3, AC10.1.4).

### I4. Withdraw a contributor's bill

```
+------------------------------------------------------------------+
|  Withdraw a bill from the test set                        h1     |
|  Bill TS-0142 · contributed 12 Sep 2026 · personal network       |
|                                                                  |
|  This will delete:                                               |
|  • the redacted original                                         |
|  • both labels and the reconciliation record                     |
|  It will be left out of every future accuracy run.               |
|  Derived copies (run outputs, caches, backups): to be settled    |
|  in Domain Design (R-07).                                        |
|                                                                  |
|  Reason given by contributor [                          ]        |
|  [ Withdraw and delete ]   [ Cancel ]                            |
+------------------------------------------------------------------+
```

- **After withdrawal:** "Bill TS-0142 was deleted. This is recorded in the access log." (AC9.3.4)

### Described, not sketched

- **Rule library list.** A table of checks with state (live, shadow, off), version, owner, review date and the date of the reference data. Each row opens the edit form, which requires source, effective dates, version, owner and review date (AC8.1.2). Editing covers data and settings only (D-US17).
- **Switch a check.** On a check's page, "Shadow" and "Off" switch directly. "Live" leads to I3 (US8.5).
- **Labelling queue.** Bills waiting for each person's label. The other person's labels stay hidden until both have submitted (AC9.2.1). There is no AI pre-fill (AC9.2.2).
- **Quality dashboard.** Reading accuracy, precision by check, missed-issue rate and user ratings, with "No test run yet" shown before the first run (US10.3).
- **Access log.** Who, what and when for every access to test-set data (AC10.2.3).

## Story coverage

Every user-facing story (Groups 1–7) maps to a screen. The internal stories (Groups 8–10) map to I1–I4 or the described tools. The full screen-by-story mapping is in `interaction-spec.md` §2.

## Assumptions & Open Questions

- All bill content, names, amounts and deadlines shown are illustrative. The real deadlines come from the hospital-side timeline reference data (US8.4).
- **R-04** (rare combinations in stored data) and **R-07** (derived copies on withdrawal) are carried to Domain Design. **R-05** (sign-in method) and **R-06** (exact "3 months") are carried to NFR Requirements [Q3].
- The founder or counsel must approve the M1 and M10 privacy wording before launch (AC1.3.4). That covers:
  - "Your files are deleted after we read them";
  - the M1 "Who sees it" line naming the bill-reading service and hosting provider;
  - the M10 storage wording.
- The wording for declarations about a minor or a patient who has died is not legally reviewed, because there is no legal review before launch.
- The plan-question options on M6 are a first draft. Domain Design may adjust them to fit the outcome categories it defines.
