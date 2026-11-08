# Design System Mapping — Hospital Bill Check (Project RS, Phase 1)

This document maps every element in `mockups.md` and `interaction-spec.md` to design tokens and to Tailwind and shadcn components [Q5]. It is the reference the build uses, so the screens, the tests and the clickable prototype (`prototype.html`) all look and behave the same.

## Sources

- [Q1]–[Q12]: answers in `refined-mockups-questions.md` in this folder.
- Rough mockups: `ideation/rough-mockups/wireframes.md` (design basis: plain, neutral, "Project RS" placeholder).
- User stories: `inception/user-stories/stories.md`, mainly US3.2 (script size), US4.9 (class shown in text), US7.8 (accessibility).
- Team practices: `inception/practices-discovery/team-practices.md` (300 KB first-load limit, Prettier and ESLint, strict TypeScript).
- Codebase: `aidlc/spaces/default/codekb/hospital-bill-audit-app/component-inventory.md` (the root layout currently loads the Geist fonts through `next/font`).

## 1. Foundations

| Decision | Value | Source |
|---|---|---|
| Styling | Tailwind CSS, with design tokens as CSS variables in the global stylesheet | [Q5] |
| Components | shadcn/ui components copied into the repository (`components/ui/`). They are built on accessible headless primitives and imported one component at a time | [Q5] |
| Icons | `lucide-react`, imported icon by icon (no icon font) | shadcn default |
| Fonts | The device's own system fonts; nothing is downloaded. The Geist fonts in `app/layout.tsx` are removed | [Q6] |
| Theme | Light only in Phase 1; the system dark-mode setting is not followed | Assumption, see §8 |
| Look | Plain and neutral, "Project RS" as a placeholder name | Rough Mockups Q1 |

## 2. Colour tokens

Every pair below was measured with the WCAG contrast formula. Body text needs at least 4.5:1, and borders, focus rings and icons at least 3:1 (stories AC7.8.7).

| Token | Hex | Used for | Measured contrast |
|---|---|---|---|
| `--background` | #ffffff | Card and dialog surfaces | — |
| `--page` | #f8fafc | Page background | — |
| `--foreground` | #1f2937 | Body text | 14.7:1 on white |
| `--muted-foreground` | #4b5563 | Secondary text, hints | 7.6:1 on white; 7.2:1 on page |
| `--primary` | #0f766e (deep teal) | Primary buttons, links, active step | White text on it 5.5:1; as a link on white 5.5:1 |
| `--primary-foreground` | #ffffff | Text on primary buttons | — |
| `--border-input` | #6b7280 | Input, checkbox and radio borders | 4.8:1 on white |
| `--ring` | #1e3a8a | Focus ring (2 px, 2 px offset) | 10.4:1 on white |
| `--likely` / `--likely-tint` / `--likely-border` | #92400e / #fffbeb / #b45309 (deep amber) | "Likely error" label, card edge | Text 6.8:1 on tint; edge 5.0:1 on white [Q7] |
| `--worth` / `--worth-tint` / `--worth-border` | #1e40af / #eff6ff / #2563eb (blue) | "Worth asking" label, card edge | Text 8.0:1 on tint; edge 5.2:1 on white [Q7] |
| `--explained` / `--explained-tint` / `--explained-border` | #374151 / #f3f4f6 / #6b7280 (grey) | "Explained" label, card edge | Text 9.4:1 on tint; edge 4.8:1 on white [Q7] |
| `--destructive` / `--destructive-tint` | #b91c1c / #fef2f2 (red) | System errors and "Discard everything" only | Text 5.9:1 on tint [Q7] |

Rules:

- **Red is never a finding colour** [Q7]. Q7 limits red to system errors.
  - Design decision DS-1 extends that to the "Discard everything" confirm button. Discarding is an irreversible, system-level action, and the standard destructive style warns the user before deleting a report they may not have saved.
  - The decision keeps Q7's intent that red never appears on anything about the bill itself. It is recorded here because Q7 named only system errors (review of this stage, R-08).
- **Colour is never the only signal.** Every finding class also has its text label and an icon (stories AC4.9.5):
  - "Likely error": `AlertTriangle`;
  - "Worth asking": `HelpCircle`;
  - "Explained": `CheckCircle2`.
- **Primary teal is separate from the class colours,** so a button is never mistaken for a finding class.

## 3. Typography

System font stack: `ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans", "Helvetica Neue", Arial, sans-serif` [Q6]. Indian-script fallbacks come from the device. They are only needed for showing bill lines as read (US3.6); all product text is English.

| Step | Phone (< 1024 px) | Desktop (≥ 1024 px) | Line height | Used for |
|---|---|---|---|---|
| Display | 28 px / 600 | 32 px / 600 | 1.25 | Screen title (h1) |
| Heading | 22 px / 600 | 24 px / 600 | 1.3 | Section title (h2) |
| Subheading | 19 px / 600 | 19 px / 600 | 1.35 | Card title, finding name |
| Body | 18 px / 400 | 17 px / 400 | 1.6 | All running text, buttons, inputs |
| Small | 15 px / 400 | 15 px / 400 | 1.5 | Evidence lines, footnotes; never below 15 px |

Five sizes only [Q6]. Amounts use tabular figures (`font-variant-numeric: tabular-nums`), so columns of rupee amounts line up.

## 4. Spacing, shape and size

| Token | Value |
|---|---|
| Spacing scale | 4, 8, 12, 16, 24, 32, 48 px (Tailwind 1, 2, 3, 4, 6, 8, 12) |
| Page gutter | 16 px phone, 32 px desktop |
| Content width | 640 px max for single-column screens; 1200 px max for the desktop findings layout [Q9] |
| Radius | 8 px cards and inputs, 12 px dialogs, full radius for status pills |
| Tap target | At least 44 × 44 px for every control; primary buttons 48 px tall on phone (stories AC7.8.6) |
| Gap between targets | At least 8 px |
| Shadow | None on cards (borders only); one soft shadow on dialogs |

## 5. Breakpoints

Two layouts [Q10]:

| Range | Tailwind | Layout |
|---|---|---|
| Below 1024 px | default (no prefix) | Phone and narrow tablet: single column, full-width primary buttons, sticky action bar only on the upload and confirm screens |
| 1024 px and up | `lg:` | Desktop: single centred column (640 px) on every screen except the report, which uses a findings list (left, 400 px) and detail panel (right) [Q9] |

Widths in between are fluid. The `sm:` and `md:` prefixes are not used for layout, only for small spacing tweaks.

## 6. Component mapping

"Product component" is the name used in `interaction-spec.md`. "Built from" is the shadcn component or native element underneath.

| Product component | Built from | Notes |
|---|---|---|
| `Button` (primary, secondary, quiet, destructive) | shadcn `Button` | Destructive variant only inside the Discard dialog |
| `ProgressSteps` | Native `<ol>` with `aria-current="step"` | Upload → Review → Report (US3.2) |
| `ChoiceGroup` (radio) | shadcn `RadioGroup` + `Label` | "Whose bill is this?", "When are you checking?", "Is the patient under 18?" |
| `Checkbox` | shadcn `Checkbox` + `Label` | Authorisation, consent and declaration boxes |
| `FieldError` | Native `<p>` linked with `aria-describedby` | Text message next to the field (AC1.5.2) |
| `LayeredNotice` | shadcn `Collapsible` | "More detail" on the privacy notice (AC1.3.2) |
| `CameraTips` | shadcn `Sheet` (bottom on phone) | Tips shown before the camera opens (AC2.2.1) |
| `FileDrop` + `FileList` | Native `<input type="file">` + `<ul>`; drag-and-drop only as an extra on desktop | Each file row shows status in text (AC2.10.5) |
| `DocChecklist` | Native `<ul>` with status text | Present, missing or optional (US2.9) |
| `ProcessingStatus` | Native `<section>` + `aria-live="polite"` | Milestone announcements only (AC3.2.6) |
| `FirstLook` | shadcn `Card` + native `<table>` | Topics only, no class labels (AC3.3.5) |
| `CaseDetails` | shadcn `Input`, `RadioGroup`, `Label` | Pre-filled, editable (US3.1) |
| `UnclearLine` | shadcn `Card` + `Input` | Confirm, type in or skip (US3.4, US3.6) |
| `BillBreakdown` | Native `<table>` plus a simple bar drawn with CSS | Text table always present (AC4.11.4) |
| `TopThree` | Native `<ol>` | Plain-language names (AC4.9.6) |
| `JumpTo` | Native `<nav aria-label="On this page">` with anchor links | [Q8] |
| `FindingCard` | shadcn `Card` + `Badge` | Class label, icon, confidence in text, amount |
| `FindingFilter` | shadcn `ToggleGroup` (single) | All / Likely error / Worth asking / Explained |
| `LowConfidenceGroup` | shadcn `Collapsible` | "Other things you may ask about (n)" (AC4.12.3) |
| `FindingDetail` | Phone: its own view; desktop: a panel beside the list | [Q9] |
| `MarkToggle` | shadcn `ToggleGroup` | "Will ask" / "Not pursuing" (US6.1) |
| `Rating` | shadcn `RadioGroup` + fixed-reason `Select` | No free text [User Stories Q25] |
| `DeskSummary` | Native `<article>` | Full-screen view uses shadcn `Dialog` set to full screen (AC5.3.5) |
| `LetterEditor` | shadcn `Textarea` + `Button` (Copy, Download) | Disclaimer kept outside the editable area (AC5.6.4) |
| `EscalationLadder` | Native `<ol>` | Deadlines in IST (AC5.5.3) |
| `SaveBar` | shadcn `Button` | "Save or share" on phones, "Download" on desktop; "Save a copy now" (AC5.4.7) |
| `LineList` + `LineEditor` | shadcn `Card`, `Input`, `ToggleGroup` | M6a: lines as read with standard description; quantity, rate and amount editable (US3.5) |
| `StorageChoice` | Two shadcn `Button`s, equal size and weight | Neither pre-selected (AC7.2.7) |
| `IdleWarning` | shadcn `AlertDialog` | Focus to "Keep going" (AC7.4.7) |
| `DiscardDialog` | shadcn `AlertDialog` | Destructive confirm |
| `ChangeNotice` | Native `<div role="status">` | After regeneration (AC3.5.5); not a disappearing toast |
| `SystemMessage` | shadcn `Alert` | Errors: plain words and a next step, no codes |
| Internal: `DataTable` | shadcn `Table` | Rule library, checks list, test-set queue |
| Internal: `SignInForm` | shadcn `Input`, `Button` | Method set in NFR Requirements (R-05) |

Not used: shadcn `Toast` or `Sonner` for important messages (they disappear), `Carousel`, `HoverCard`, `Tooltip` for essential information, and `NavigationMenu` (the product has no menus).

## 7. Script-size notes (300 KB first load)

- shadcn components are source files in the repository, so only the components a route imports are shipped.
- The headless primitives under `Dialog`, `AlertDialog`, `Sheet` and `Collapsible` load only on the routes that use them.
- No web fonts and no icon font. Lucide icons are imported one by one.
- The on-device PDF tool is loaded only after "Download" or "Save or share" is tapped, within its own 200 KB limit (User Stories Q21).
- The automatic script-size check (stories AC3.2.4) is the authority. If a component pushes a route over the limit, replace it with the native element listed in §6.

## 8. Assumptions & Open Questions

- Light theme only in Phase 1. Following the system dark-mode setting can be added later through the same tokens.
- shadcn's current default primitives library and version are chosen at Code Generation, after reading the bundled Next.js 16.4.3 docs (`team-practices.md`).
- Tailwind's version and its setup with Next.js 16.4.3 are confirmed at Code Generation.
- "Project RS" stays a placeholder until there is a brand. Swapping the name and the primary colour is a token change only.
