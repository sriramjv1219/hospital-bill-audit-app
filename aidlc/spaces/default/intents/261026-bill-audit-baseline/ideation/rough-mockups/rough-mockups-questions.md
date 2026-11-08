# Rough Mockups — Questions

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- The approved intent statement (`ideation/intent-capture/intent-statement.md`) and the scope document and intent backlog (`ideation/scope-definition/`).
- The PRD's journey J1 (§11.2), onboarding, upload, report and action requirements (§12.1–12.10), personas (§9) and accessibility requirement NFR-13 in `PRDs/app-baseline.md`.

Already settled, so not asked again:

- **The screens and their order.** The PRD's journey J1 sets them:
  - privacy notice and consent;
  - picking the moment ("In hospital", "At discharge", "Already paid");
  - guided upload with a checklist;
  - a quick summary;
  - the full audit with the Bill Breakdown;
  - acting at the billing desk with the Billing Desk Summary;
  - download and finish.
- **How findings are grouped.** Likely errors / worth asking / explained / not assessable (PRD §11.2).
- **The report's seven parts and the one-page Billing Desk Summary** (PRD RP-01, RP-02).
- **Mobile web and desktop web; no native app** (PRD D18). English only (PRD D9).
- **Plain, neutral, non-accusatory language** at about a school-leaver reading level (PRD RP-04, TS-02).
- **Pay-under-protest and urgent-situation guidance** are out of this build (Scope Definition).

---

## Q1. Is there a brand or visual style to follow?

The PRD has no brand guidelines, logo or design system, and the product is still called by its codename, "Project RS", until launch.

A. No brand yet: use a plain, neutral look with the codename "Project RS" as a placeholder
B. Yes, there are brand guidelines or a design system (please give a link or path under X)
C. Use a specific existing design system (please name it under X)
X. Other (please specify)

[Answer]: A

## Q2. Which screen size should the design start from?

The PRD supports both phone and desktop browsers. It says the product must work on low and mid-range Android phones over patchy hospital networks (NFR-12). The remote family manager often works from a laptop (persona "Meera").

A. Phone first: design for a phone at the billing desk, then widen for desktop
B. Phone and desktop designed equally from the start
C. Desktop first, then narrow for phones
X. Other (please specify)

[Answer]: B

## Q3. What accessibility level should the screens meet?

The PRD asks for screens that are readable on small screens, offer large text, are friendly to screen readers, and support users with low digital literacy (NFR-13). It does not name a formal standard.

A. WCAG 2.1 level AA, the common standard for public web products
B. WCAG 2.2 level AA, the newer version of the same standard
C. Only what NFR-13 lists, with no formal standard
X. Other (please specify)

[Answer]: A

## Q4. Should the rough mockups also cover the internal tools?

The internal tools (rule library, Labelled Test Set labelling, ops/admin) are in scope in their leanest form (Scope Definition, Q6). Families never see them.

A. User-facing screens only; describe each internal tool in a short paragraph
B. User-facing screens plus simple sketches of the internal tools
X. Other (please specify)

[Answer]: A

## Q5. What form should the rough mockups take?

A. Simple text sketches of each screen inside the documents (boxes and labels), which are quick to change
B. Text sketches, plus a clickable page you can open in a browser and share with the founder
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Brand (Q1): no brand yet; a plain, neutral look with the codename "Project RS" as a placeholder.
- Screen size (Q2): phone and desktop designed equally from the start.
- Accessibility (Q3): WCAG 2.1 level AA.
- Internal tools (Q4): user-facing screens only; each internal tool is described in a short paragraph.
- Form (Q5): simple text sketches of each screen inside the documents.
- Taken from the PRD without asking: the journey J1 screens and their order, the finding groups, the report's seven parts and one-page Billing Desk Summary, English only, plain non-accusatory language; pay-under-protest and urgent-situation guidance stay out.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
