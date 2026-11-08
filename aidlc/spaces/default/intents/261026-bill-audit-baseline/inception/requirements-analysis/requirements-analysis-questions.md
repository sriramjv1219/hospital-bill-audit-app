# Requirements Analysis — Questions

The product document (PRD) already lists the Phase 1 requirements with stable IDs, and ideation settled the scope. These questions cover only what still needs a number or a choice before the requirements can be written with pass/fail criteria.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- PRD: `PRDs/app-baseline.md` v0.14, read through the document library (`aidlc/spaces/default/knowledge/documents/app-baseline.md`, document id 01a17987), §8.4, §12, §13, §14 and gaps G-01 to G-23.
- Ideation: `ideation/intent-capture/intent-statement.md`, `ideation/scope-definition/scope-document.md`, `ideation/feasibility/constraint-register.md`, `ideation/rough-mockups/wireframes.md`, `ideation/approval-handoff/initiative-brief.md`.
- Code knowledge base: `aidlc/spaces/default/codekb/hospital-bill-audit-app/`.
- Team practices: `inception/practices-discovery/team-practices.md`.

Already settled, so not asked again:

- **Launch geography, Phase 1 free, public-source reference data and the patient-side brand stance** (PRD D17, D25, D22).
- **Scope:** every Must Have and Should Have except urgent-situation and pay-under-protest guidance (Phase 1.5); Could Have items deferred; the 18 launch checks live, the 8 document checks as guidance, the rest going live as each passes (`scope-document.md`).
- **The eight agreed rough-mockup changes (R-01 to R-08)** are carried forward as inputs here and to Refined Mockups (`approval-handoff-questions.md`, Q1). They are written into the requirements as agreed.
- **PRD gaps already resolved** in scope definition: G-03, G-04, G-06, G-11, G-12, G-13, G-20, G-22.
- **Founder decisions due before the build:** data residency (G-14), hosting, and the bill-correction measure (G-02) (`approval-handoff-questions.md`, Q7).
- **Accuracy targets:** at least 95% of lines read correctly on digital PDFs and 90% on good photos; at least 90% precision for "Likely error" and 75% for "Worth asking" (PRD §8.4).

---

## Q1. How long can a session sit idle before it ends and its data is discarded?

The PRD leaves this open (gap G-10, requirement SD-08). Families at the billing desk may step away, and a session that ends too soon loses an unsaved report.

A. 30 minutes idle, with a warning and a "Keep going" button at 25 minutes
B. 60 minutes idle, with a warning at 50 minutes
C. 2 hours idle, with a warning 10 minutes before
X. Other (please specify)

[Answer]: A

## Q2. Above what amount should a finding be marked "Complex — consider professional advice"?

Findings above a value threshold, or in clinically sensitive categories, get this label (requirement AA-16; PRD open question Q-P2). The threshold can be changed later without a release.

A. Start at ₹25,000 per finding
B. Start at ₹50,000 per finding
C. Use clinically sensitive categories only at launch; set the value threshold from usage data later
X. Other (please specify)

[Answer]: C

## Q3. Which low-confidence findings should be folded away under "Other things you may ask about"?

Low-confidence findings below a value threshold are shown collapsed so families are not overwhelmed (PRD §13.1.2). The threshold can be changed later.

A. Low-confidence findings under ₹1,000
B. Low-confidence findings under ₹500
C. Every low-confidence finding, whatever the amount
X. Other (please specify)

[Answer]: C

## Q4. How many test bills must a check be measured on before it can go live?

A check goes live only when it meets its precision target on the labelled test bills, but some checks (implant caps, package rates) may have very few qualifying bills (gap G-16).

A. At least 20 bills where the error is present and 50 where it is not, with at least 5 of the 20 being real (not made-up) bills
B. At least 10 bills with the error and 30 without
C. At least 30 bills with the error and 100 without
X. Other (please specify)

[Answer]: B

## Q5. How do we stop abuse of the free check without identifying anyone?

Per-device limits could count as personal data and contradict "no device-linked profile" (gap G-09, requirements SD-02 and SD-09).

A. Short-lived limits by network address, kept in memory for at most 24 hours, plus a bot challenge before upload; no lasting device identifier
B. A bot challenge before upload only
C. No abuse controls at launch; add them if abuse appears
X. Other (please specify)

[Answer]: X. Other: "None at launch, add this as a requirement to Phase 1.5"

## Q6. Does the closed beta continue after launch?

The beta is described as pre-launch (LT-04), but the test set is later topped up from "network or beta" (LT-13), and how beta testers' data is handled is not specified (gap G-17).

A. Yes, it continues after launch under its own separate consent; tester contact details stay only with the beta programme and are never linked to product data
B. No, it ends at launch; later top-ups come from the personal network only
X. Other (please specify)

[Answer]: B

## Q7. How should "share a bill from WhatsApp" work (Should Have S1)?

On phone browsers this needs the site to be installed as a home-screen app, and then it works mainly on Android Chrome. The feasibility check flagged that this was never assessed (warning W1).

A. Make the site installable so it appears in the phone's share menu where supported, with normal upload as the fallback everywhere else
B. Drop share-in for this build; offer upload only, with guidance on saving a file from WhatsApp first
X. Other (please specify)

[Answer]: B

## Q8. Which accessibility standard must the screens meet?

The PRD asks for small-screen readability, large text and screen-reader support (NFR-13) but names no standard, so there is no pass/fail test.

A. WCAG 2.2 level AA
B. WCAG 2.1 level AA
X. Other (please specify)

[Answer]: A

## Q9. What phone and network should the app be built and tested for?

The PRD asks the app to work on low and mid-range Android phones on patchy hospital networks (NFR-12), with no figures.

A. A 2 GB RAM Android phone on a slow connection (about 400 kbps): the first screen usable within 5 seconds, at most 300 KB of scripts per page, and uploads that resume after a dropped connection without re-sending finished files
B. A 4 GB RAM Android phone on a typical 4G connection, with the same resume behaviour
X. Other (please specify)

[Answer]: X. Other: "8GB, typical 4G"

## Q10. How much can be uploaded in one session?

Bills can run to many pages, and photos of each page add up quickly (DU-01, DU-02).

A. Up to 20 files, 25 MB per file and 60 pages in total
B. Up to 10 files, 10 MB per file and 30 pages in total
X. Other (please specify)

[Answer]: A

## Q11. Should the three recommendations that have no requirement become requirements?

The PRD recommends them but gives them no ID (gap G-19): a watermark on reports saying they are a private, non-conclusive analysis; a public data charter ("we never share your data with insurers for underwriting"); and keeping the bill-reading service replaceable without rewriting the checks.

A. Yes, all three become requirements
B. Only the watermark and the replaceable bill reader
C. None; log them as decisions only
X. Other (please specify)

[Answer]: C

## Q12. What should families see for the two unnamed "Who to ask" routes?

The check list uses "User" (for example, a missing document only the family can supply, DOC-02) and "Expert" (later-phase checks), but neither is a label shown to families (gap G-18).

A. "You can sort this out" for the User route; the Expert route is not shown in Phase 1
B. "Check your own documents" for the User route; the Expert route is not shown in Phase 1
X. Other (please specify)

[Answer]: A

## Q13. How should the requirements handle the bill-correction measure until the founder decides it?

Whether billing desks actually correct bills cannot be tracked after the session (gap G-02). The founder decides how to measure it before the build starts. The PRD already lets the session ask about bills that are already paid (FB-02).

A. Write the in-session question for already-paid bills ("Was this bill revised?") as a requirement now; the founder may add or change the measure later
B. Leave it as an open item until the founder decides
X. Other (please specify)

[Answer]: X. Other: "No Need to handle bill correction measure."

---

## Follow-up questions

## Q14. Does the 8 GB phone on typical 4G replace the PRD's "low and mid-range Android phones" target, and what happens to the speed and data-use parts?

Your Q9 answer was "8GB, typical 4G". The PRD requirement for device and network (NFR-12) says the app works on low and mid-range Android phones on patchy hospital networks, with uploads that resume and low data use. An 8 GB phone is high-end in India, so the two need reconciling before the requirement can have a pass/fail test.

A. 8 GB phone on typical 4G is the only target; keep upload resume within the session and a page-weight limit (at most 300 KB of scripts per page)
B. 8 GB phone on typical 4G is the main target, plus a basic smoke check on a 4 GB phone; keep upload resume and the page-weight limit
C. 8 GB phone on typical 4G only; drop upload resume and the low-data-use requirement
X. Other (please specify)

[Answer]: A

## Q15. With no bill-correction measure, does the in-session question "Was this already-paid bill revised?" stay?

Your Q13 answer was "No Need to handle bill correction measure." The PRD requirement for in-session outcome questions (FB-02) asks what the family already knows, such as whether an already-paid bill was revised, and what they plan to do. It is part of Must Have M18.

A. Keep FB-02 as the PRD writes it; there is simply no bill-correction metric
B. Drop the "was the bill revised?" part; keep only "what do you plan to do"
C. Drop FB-02 entirely
X. Other (please specify)

[Answer]: B

## Q16. Which clinically sensitive categories get the "Complex — consider professional advice" label at launch?

Your Q2 answer uses categories only, with no value threshold. The PRD requirement for this label (AA-16) does not list the categories.

A. Implants and medical devices, ICU and critical care, surgery and operation-theatre charges, and cancer medicines
B. Implants and medical devices, and ICU and critical care only
C. Let Domain Design draft the list from the PRD's bill categories, to be confirmed there
X. Other (please specify)

[Answer]: C

---

## Consolidated Summary Confirmation

- Session timeout: a session ends after 30 minutes idle, with a warning and a "Keep going" button at 25 minutes (Q1).
- "Complex — consider professional advice" label: clinically sensitive categories only at launch, no value threshold; the value threshold is set from usage data later (Q2). The category list is drafted in Domain Design and confirmed there (Q16).
- Folded away under "Other things you may ask about": every low-confidence finding, whatever the amount (Q3).
- Minimum test bills before a check goes live: at least 10 bills with the error and 30 without (Q4).
- Abuse controls: none at launch; identity-free abuse controls (Should Have S9) move to Phase 1.5 as a requirement there (Q5).
- Closed beta: ends at launch; later Labelled Test Set top-ups come from the personal network only (Q6).
- Share-in from WhatsApp (Should Have S1): dropped from this build; upload only, with guidance on saving a file from WhatsApp first (Q7).
- Accessibility: WCAG 2.2 level AA (Q8).
- Device and network: an 8 GB RAM Android phone on typical 4G is the only target; uploads resume within the session after a dropped connection, and each page loads at most 300 KB of scripts (Q9, Q14).
- Upload limits: up to 20 files, 25 MB per file and 60 pages per session (Q10).
- Report watermark, public data charter and replaceable bill reader: logged as decisions only, not requirements (Q11).
- "Who to ask" labels: "You can sort this out" for the User route; the Expert route is not shown in Phase 1 (Q12).
- Bill-correction measure: not handled in this build (Q13); the in-session outcome question (FB-02) keeps only "what do you plan to do" and drops "was this already-paid bill revised?" (Q15).
- Scope changes against `scope-document.md`: S1 and S9 leave this build (S9 to Phase 1.5), and PRD gap G-02 is closed as not measured.

Does this all look correct before I generate the requirements artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
