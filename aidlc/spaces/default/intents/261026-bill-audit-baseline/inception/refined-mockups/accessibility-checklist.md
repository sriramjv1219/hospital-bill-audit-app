# Accessibility Checklist — Hospital Bill Check (Project RS, Phase 1)

The target is WCAG 2.2 level AA on phone and desktop browsers (NFR12; Requirements Analysis Q8). This checklist says how each relevant success criterion is met in `mockups.md`, `interaction-spec.md` and `design-system-mapping.md`, and how it is verified. Automated tools find only about a third of issues, so the manual checks are part of the gate, not extras (stories AC7.8.1, AC7.8.2).

## Sources

- [Q1]–[Q12]: answers in `refined-mockups-questions.md` in this folder.
- User stories: `inception/user-stories/stories.md`, US7.8 and the accessibility criteria in other stories (AC1.5.2, AC3.2.6, AC3.2.7, AC4.9.5, AC4.11.4, AC4.12.3, AC5.4.8, AC7.4.7).
- Requirements: `inception/requirements-analysis/requirements.md` (NFR12).
- Team practices: `inception/practices-discovery/team-practices.md` (Playwright end-to-end tests).
- Design knowledge: `.claude/knowledge/aidlc-design-agent/accessibility-wcag.md`.

## Verification methods

| Code | Method |
|---|---|
| A | Automated: axe-core in Playwright on every user screen and every state, on every merge |
| K | Keyboard-only walk-through of the whole journey |
| SR | Screen reader: TalkBack on Android, NVDA or VoiceOver on desktop (AC7.8.2) |
| Z | 200% text and 320 px width checks (Playwright viewport plus manual) |
| C | Contrast measured from the tokens (`design-system-mapping.md` §2) |
| R | Design or content review |

## Success criteria

| SC | Name | How it is met | Where | Check |
|---|---|---|---|---|
| 1.1.1 | Non-text content | Icons beside class labels are `aria-hidden`, and the text label carries the meaning. A bill-line image has alt text naming the document, page and line | M6, M7, M5 | A, SR |
| 1.3.1 | Info and relationships | Real headings (h1 per screen, h2 sections), `<fieldset>` and `<legend>` for every choice group, real tables for amounts, lists for checklists and the ladder | All | A, SR |
| 1.3.2 | Meaningful sequence | DOM order matches the visual order. On desktop, the findings list comes before the detail panel | M6 | SR |
| 1.3.4 | Orientation | Works in portrait and landscape; the full-screen summary rotates | M8 | Z |
| 1.3.5 | Identify input purpose | Internal sign-in email field uses `autocomplete="email"`. There are no personal fields in the family journey | I1 | A |
| 1.4.1 | Use of colour | Class, confidence, errors and checklist status are always in text; colour is extra | M3, M6, M7 | R, C |
| 1.4.3 | Contrast (minimum) | All text at least 4.5:1; the lowest measured is 5.5:1 (white on primary) | All | C, A |
| 1.4.4 | Resize text | 200% text with no loss of content or function | All | Z |
| 1.4.10 | Reflow | 320 CSS px width with no sideways scroll, except bill-line tables, which scroll inside their own area | All | Z |
| 1.4.11 | Non-text contrast | Input borders 4.8:1, class edges at least 4.8:1, focus ring 10.4:1 | All | C |
| 1.4.12 | Text spacing | Layout survives increased line, letter and word spacing; no fixed-height text boxes | All | Z |
| 1.4.13 | Content on hover or focus | No essential tooltips; nothing appears only on hover | All | R |
| 2.1.1 | Keyboard | Every action works from the keyboard, including drag-and-drop upload ("Choose files" is always present) | All | K |
| 2.1.2 | No keyboard trap | Dialogs trap focus only while open and release on close or Esc | Dialogs | K |
| 2.2.1 | Timing adjustable | The 30-minute idle limit warns at 25 minutes with "Keep going" (extend); the timer pauses while a report is prepared, for at most 15 minutes | Session-wide | K, SR |
| 2.2.2 | Pause, stop, hide | No auto-moving content; progress text changes only at milestones | M4 | R |
| 2.3.1 | Three flashes | No flashing content | All | R |
| 2.4.1 | Bypass blocks | "Skip to content" is the first focus stop | All | K |
| 2.4.2 | Page titled | Each screen has a unique title, for example "Add your documents — Project RS" | All | A |
| 2.4.3 | Focus order | Focus orders are listed per screen in `mockups.md` | All | K |
| 2.4.4 | Link purpose | Links and buttons name their target ("Retake page 4", "Jump to Letters") | All | SR |
| 2.4.6 | Headings and labels | Every heading and label describes its content | All | R |
| 2.4.7 | Focus visible | 2 px focus ring, 2 px offset, never removed | All | K |
| 2.4.11 | Focus not obscured (minimum) | The sticky action bar on M3 and M5 never covers the focused element; the page scrolls the focused item into view above the bar | M3, M5 | K |
| 2.5.3 | Label in name | The accessible name starts with the visible label | All | A |
| 2.5.7 | Dragging movements | Drag-and-drop upload always has the "Choose files" button | M3 | K |
| 2.5.8 | Target size (minimum) | All targets at least 44 × 44 CSS px (stricter than the 24 px minimum) with 8 px gaps | All | R, Z |
| 3.1.1 | Language of page | `lang="en"` on every page | All | A |
| 3.2.1 / 3.2.2 | On focus / On input | Choosing an option never navigates; only buttons move to the next screen | M2, M3 | K |
| 3.2.3 | Consistent navigation | Header, step indicator and footer are in the same place on every screen | All | R |
| 3.2.4 | Consistent identification | Fixed labels: "Keep going", "Save or share" (phone) and "Download" (desktop), "Discard everything" (header and dialog confirm), "Confirm a few details", "I want the Insurance Check" | All | R |
| 3.2.6 | Consistent help | The "What this check is not" and "Privacy" footer links are in the same place on every screen (`mockups.md`, Common to every screen) | All | R |
| 3.3.1 | Error identification | Errors are described in text next to the field and announced (AC1.5.2) | M2, M3, I1 | SR |
| 3.3.2 | Labels or instructions | Every input has a visible label; formats are shown ("DD/MM/YYYY") | M5, I1 | R |
| 3.3.3 | Error suggestion | Messages say what to do next ("Remove a file to add another") | M3 | R |
| 3.3.4 | Error prevention (legal, financial, data) | "Discard everything" asks for confirmation; storage consent is a deliberate choice of two equal buttons | Dialogs, M10 | K |
| 3.3.7 | Redundant entry | Case details are pre-filled from the documents; nothing typed earlier is asked again | M5 | R |
| 3.3.8 | Accessible authentication (minimum) | Internal sign-in uses no cognitive test (no puzzles or transcription), and passkeys or codes can be pasted; the final method is set in NFR Requirements | I1 | R |
| 4.1.2 | Name, role, value | shadcn primitives expose roles and states (`aria-expanded`, `aria-current`, `aria-invalid`) | All | A, SR |
| 4.1.3 | Status messages | Upload status, milestones, change notices and copy confirmations use `role="status"` | M3, M4, M6, M9 | SR |

## Beyond WCAG: product-specific checks

| Check | Requirement | Where | Check |
|---|---|---|---|
| Body text at least 18 px on phones | [Q6] | All | R |
| Small text never below 15 px | `design-system-mapping.md` §3 | All | R |
| Reduced motion | No animation beyond simple state changes when "reduce motion" is on (AC7.8.8) | All | Z |
| Step indicator read as text | "Step 2 of 3: Review, current step" (AC3.2.7) | All | SR |
| Bill Breakdown has a text table | AC4.11.4 | M6 | SR |
| PDF text is selectable and in reading order | AC5.4.8 | PDF | R |
| Full-screen summary readable at 24 px | AC5.3.5 | M8 | Z |
| Calm wording for a family that has lost a patient | AC1.7.2, AC1.7.4 | M2, M9 | R |
| Real phone pass before launch | One pass on an 8 GB Android phone with TalkBack (launch checklist) | All | SR |

## Per-screen checklist

| Screen | Headings | Keyboard entry point | Key screen-reader text | States to test |
|---|---|---|---|---|
| M1 | h1 "Is your hospital bill right?" | Skip link, then "More detail" | Disclaimer list read in full | Default, notice open |
| M2 | h1 "Before you upload" | First relationship option | Group legends, error text | Default, each declaration, missing answer, declined |
| M3 | h1 "Add your documents" | Stay question | Disabled reason, file status, checklist status | Empty, uploading, paused, blurry, locked, refused |
| M4 | h1 "Reading your bill" then "First look" | Status region | Milestones only | Reading, long wait, 15-min cap, failed |
| M5 | h1 "Please check a few things" | First field | "We weren't sure", "Could not be read" | None unclear, unclear left, mismatch |
| M6 | h1 "Your bill check" | "Save or share" | Card labels, Breakdown table, group state | Running, no findings, summary only, changed |
| M6a | h1 "Your bill lines", h2 per document page | "Back to your report" | "Changed by you", number errors | Default, no matches, invalid number, unread line, save failed |
| Discard dialog | h2 dialog title | "Cancel", or "Save or share first" when unsaved | Unsaved-report line | No report, report not saved, report saved |
| M7 | h1 finding name (h2 in the desktop panel) | "Back to findings" | Four part headings, mark state | Likely, Worth asking, Explained, Complex |
| M8 | h1 in full screen | "Show full screen" | List items with page and line | Marked, nothing marked, full screen, offline |
| M9 | h2 per group | First letter | "Letter copied" | Already paid order, NPPA shown or hidden |
| M10 | h1 "Before you go" | "Save or share your report" | Equal buttons, reminder | Not saved, saved, finished |
| I1–I4 | h1 per screen | First field or button | Disabled reasons in text | Pass and fail states |

## Assumptions & Open Questions

- The internal sign-in method (R-05) is set in NFR Requirements. The 3.3.8 check is re-run once it is chosen.
- Screen-reader versions for the manual pass are chosen in Build and Test.
