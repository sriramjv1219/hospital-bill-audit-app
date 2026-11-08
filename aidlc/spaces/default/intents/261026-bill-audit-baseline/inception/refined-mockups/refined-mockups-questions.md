# Refined Mockups — Questions

This stage turns the rough text sketches (`ideation/rough-mockups/`) into refined mockups. It adds exact wording, every screen state, the interaction specification, the design-system mapping and an accessibility checklist, all built from the approved user stories (`inception/user-stories/stories.md`).

## Already settled, not asked again

- **Look:** no brand yet. Plain and neutral, with "Project RS" as a placeholder name (Rough Mockups Q1).
- **Screen sizes:** phone and desktop are designed equally. Each sketch shows the phone layout plus a written note on the desktop layout (Rough Mockups Q2; project rule).
- **Accessibility standard:** WCAG 2.2 level AA (Requirements Analysis Q8; replaces the 2.1 in the rough mockups). Stories US7.8 add 44 × 44 px tap targets, 320 px reflow and keyboard-only completion.
- **Screen list and order:** taken from PRD journey J1 and the rough mockups S1–S11. The changes come from the stories:
  - at most 2 screens before upload;
  - "When are you checking?" at the top of the upload screen;
  - a first look that shows topics only;
  - the Billing Desk Summary built from "Will ask" findings.
- **Fixed labels:** "Keep going", "Confirm a few details", "Save or share", "Discard everything" (stories).

## Carried forward from User Stories

The User Stories approval accepted seven review findings without changing the stories. Per the project rule, they are raised here first so they are not lost:

- **R-01:** nothing caps how long the idle timer stays paused while a report is prepared (Q1).
- **R-02:** with self-approval allowed, rule-change approval does not stop a wrong price reaching families (Q2).
- **R-03:** the story count is 85, not 45–60. You heard this at the gate and approved, so nothing more is needed.
- **R-04 to R-07** (Q3):
  - R-04: rare hospital-and-procedure combinations in stored data;
  - R-05: sign-in blocks the internal tools until NFR Requirements defines it;
  - R-06: the exact length of "3 months";
  - R-07: derived copies when a contributor withdraws a bill.

---

## Q1. Limit on the paused idle timer (User Stories review finding R-01)

The idle timer pauses while a report is being prepared (User Stories Q18). If processing stalls, the session and its bill data could stay open with no end. The requirements allow up to 15 minutes for a full report (NFR2).

A. Cap the pause at 15 minutes. After that, the job is treated as failed: the user sees "This is taking too long" with "Try again", and the idle timer runs again from that moment
B. Cap the pause at 30 minutes, with the same behaviour after the cap
C. Leave the cap to NFR Requirements; the mockups show the "taking too long" state without a number
X. Other (please specify)

[Answer]: A

## Q2. What rule-change approval protects against (User Stories review finding R-02)

You chose to allow self-approval of rule changes (User Stories Q24). The reviewer noted that the story still claims a mistake "cannot reach families unchecked", which self-approval does not deliver.

A. Add one cheap safeguard: before any approved change takes effect, the synthetic test bills are re-run automatically, and the change is blocked if any expected finding changes unexpectedly. The internal-tools sketch shows this
B. No new safeguard: record self-approval as an accepted risk, and describe the approval step as "a recorded, deliberate change with an audit trail"
C. Carry it to Domain Design to decide
X. Other (please specify)

[Answer]: A

## Q3. The other carried findings (R-04 to R-07)

These do not change screens much, but each needs a home.

A. Carry them as named items: R-04 (rare combinations) and R-07 (derived copies on withdrawal) to Domain Design; R-05 (sign-in) and R-06 (3-month length) to NFR Requirements. The storage-consent and internal-tool sketches note them as open
B. Settle them in this stage as design decisions where they affect a screen
X. Other (please specify)

[Answer]: A

## Q4. How detailed should the refined mockups be?

The rough mockups were quick text sketches. This stage adds exact copy and every state.

A. Detailed text mockups: exact wording, every state (empty, loading, error, partial, success) and annotations for focus order and screen-reader text, with phone layouts drawn and desktop described
B. The same, plus a clickable HTML prototype of the main journey that you can open in a browser and show the founder
X. Other (please specify)

[Answer]: B

## Q5. Components: build our own or use a library?

There is no design system yet. Every page must stay within 300 KB of compressed JavaScript on first load.

A. Our own small set of about 15 components (button, field, checkbox group, file list, progress steps, disclosure, dialog, toast, finding card and others) on design tokens, using native HTML elements first and no third-party UI library
B. Our own styling on top of an accessible headless library for the hard parts only (dialog, disclosure, focus handling), loaded per component
C. A full component library (for example Material UI), accepting the larger script size
X. Other (please specify)

[Answer]: X. Other: use tailwind and shadcn

## Q6. Text style

The users include stressed families and older relatives reading on mid-range phones over patchy networks.

A. The phone's own system fonts (nothing to download), 18 px body text on phones and 17 px on desktop, a short type scale of 5 sizes, generous line spacing
B. One downloaded web font (for example Noto Sans), 16 px body text, for a more consistent look across devices
X. Other (please specify)

[Answer]: A

## Q7. How the finding classes look

Classes are always written in text with an icon (stories AC4.9.5). Colour is extra. The colour choice still sets the emotional tone for a family under stress.

A. Calm colours: deep amber for "Likely error", blue for "Worth asking", grey for "Explained"; red is used only for system errors
B. Conventional colours: red for "Likely error", amber for "Worth asking", green for "Explained"
C. No colour coding: text labels and icons only
X. Other (please specify)

[Answer]: A

## Q8. How the report screen is organised

The report has several parts: Top 3, Bill Breakdown, findings, Billing Desk Summary, letters and next steps.

A. One scrolling report page with the Top 3 and Bill Breakdown first, then a short "Jump to" list (Findings, Billing desk summary, Letters, Next steps); the Billing Desk Summary also has its own full-screen view
B. Separate tabs for each part
X. Other (please specify)

[Answer]: A

## Q9. Desktop layout for the findings

Meera works on a laptop and reviews many findings.

A. On desktop, findings show as a list on the left with the selected finding's detail on the right; every other screen is a single centred column
B. Desktop uses the same single centred column as the phone throughout, just wider
X. Other (please specify)

[Answer]: A

## Q10. Screen-width breakpoints

A. Two layouts: phone and narrow tablet below 1024 px, desktop from 1024 px, with fluid widths in between
B. Three layouts: phone below 768 px, tablet 768–1023 px, desktop from 1024 px
X. Other (please specify)

[Answer]: A

## Q11. Internal tools in the mockups

The rough mockups only described the internal tools (Rough Mockups Q4).

A. Sketch the four screens where mistakes matter most: promoting a check to live, approving a rule change, signing in, and withdrawing a contributor's bill; describe the rest
B. Keep describing the internal tools only; no sketches
C. Sketch every internal screen
X. Other (please specify)

[Answer]: A

## Q12. Follow-up to Q4: how the clickable prototype is shared

You asked for a clickable HTML prototype of the main journey to show the founder (Q4 B). It uses made-up bill data only.

A. A single HTML file in this stage's folder that you open in a browser and can send to the founder as a file
B. The same file, also published as a private web page with a link you can share with the founder (private by default; nothing is shared until you send the link)
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- The paused idle timer is capped at 15 minutes. After that the job counts as failed, the user sees "This is taking too long" with "Try again", and the idle timer runs again (User Stories review R-01) [Q1].
- Before an approved rule change takes effect, the synthetic test bills are re-run automatically, and the change is blocked if any expected finding changes unexpectedly (User Stories review R-02) [Q2].
- Carried as named items: R-04 (rare hospital-and-procedure combinations) and R-07 (derived copies on bill withdrawal) to Domain Design; R-05 (sign-in definition) and R-06 (exact "3 months") to NFR Requirements. The affected sketches note them as open [Q3].
- Detailed text mockups with exact wording, every state, and focus-order and screen-reader notes; phone layouts drawn and desktop layouts described [Q4].
- A clickable HTML prototype of the main journey, with made-up bill data only, saved as a single local HTML file in this stage's folder [Q4], [Q12].
- Components are built with Tailwind and shadcn [Q5].
- Text uses the phone's own system fonts, 18 px body text on phones and 17 px on desktop, a 5-size scale and generous line spacing [Q6].
- Finding classes use calm colours: deep amber for "Likely error", blue for "Worth asking", grey for "Explained"; red is used only for system errors; class is always also written in text with an icon [Q7].
- The report is one scrolling page: Top 3 and Bill Breakdown first, then a "Jump to" list (Findings, Billing desk summary, Letters, Next steps); the Billing Desk Summary also has its own full-screen view [Q8].
- On desktop, findings show as a list on the left with the selected finding's detail on the right; every other screen is a single centred column [Q9].
- Two layouts: phone and narrow tablet below 1024 px, desktop from 1024 px, with fluid widths in between [Q10].
- Internal tools: four sketched screens (promoting a check to live, approving a rule change, signing in, withdrawing a contributor's bill); the rest described [Q11].

Does this all look correct before I generate the refined mockups?

- Looks correct
- Request changes

[Answer]: Looks correct
