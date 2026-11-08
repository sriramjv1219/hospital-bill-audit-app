# Phase Check — Ideation to Inception

This check confirms that the ideation work hangs together before detailed planning starts. It traces the approved intent through the scope to the backlog, and checks every scope item against the feasibility findings.

**Result: Pass, with three warnings.** None of the warnings blocks Inception.

## Sources

- `ideation/intent-capture/intent-statement.md` (the product boundary).
- `ideation/scope-definition/scope-document.md` (Must Haves M1–M22 and Should Haves S1–S9) and `ideation/scope-definition/intent-backlog.md` (work packages WP-00 to WP-14).
- `ideation/feasibility/feasibility-assessment.md`, `ideation/feasibility/constraint-register.md` and `ideation/feasibility/raid-log.md`.
- `ideation/approval-handoff/initiative-brief.md` and `ideation/approval-handoff/decision-log.md`.

## 1. Intent to scope

| Intent boundary (`intent-statement.md`) | In the scope document | Status |
|---|---|---|
| Phase 1 Hospital Bill Check MVP | Every Must Have (M1–M22) and Should Have except S7 | OK |
| Internal modules and P0 tooling: rule library, Labelled Test Set, ops/admin | Section 2.3, in leanest form (M15, M19, M20) | OK |
| Phase 1.5 and later phases are out | Section 3 | OK |
| Insurer-side problems are out | Won't Have list, W1–W22 | OK |
| Bereaved families are a primary customer | Not a separate scope item. Covered by the consent declarations (M1) and the gentle wording noted in `wireframes.md` | OK |

## 2. Scope to backlog

Every in-scope feature maps to at least one work package.

| Scope items | Work package | Status |
|---|---|---|
| M1, M2, M16, M17 | WP-01 Entry, consent and session | OK |
| M3, M4 | WP-02 Upload and capture guidance | OK |
| M5, M6, M7, S5, implant detail capture | WP-08 Bill reading and normalisation | OK |
| M8, M9, M10 | WP-03 Launch checks engine | OK |
| M11, M12, S2, S4, S6 | WP-05 Recommendations and templates | OK |
| M13, M14 | WP-04 Report and Billing Desk Summary | OK |
| M15, Labelled Test Set coverage reporting and top-ups | WP-11 Labelled Test Set and quality measurement | OK |
| M18, S8 | WP-09 Feedback | OK |
| M19 | WP-07 Rule library and reference data | OK |
| M20 | WP-12 Ops and admin | OK |
| M21 | WP-06 Trust and safety | OK |
| M22, S9 | WP-13 Security, privacy and incident response | OK |
| S1 | WP-10 Share from messaging apps | OK |
| S3 | WP-14 Wave 2 and Wave 3 checks | OK |
| Feasibility condition F1 | WP-00 Bill-reading trial | OK |
| S7 urgent-situation guidance | No package; moved to Phase 1.5 | OK (out of scope) |

Reverse check: every work package traces back to a scope item or a feasibility condition. There are no orphan packages.

## 3. Scope to feasibility

| Work packages | Feasibility backing (`feasibility-assessment.md`) | Status |
|---|---|---|
| WP-00, WP-08 Bill reading | Unproven; the highest risk. Gated on condition F1 | OK, conditional |
| WP-03, WP-07, WP-14 Checks and rule library | Feasible, with domain risk. The typical-quantity source is open (G-03) | OK |
| WP-04, WP-05 Report, letters and Billing Desk Summary | Feasible | OK |
| WP-01, WP-06, WP-13 Consent, trust, security and privacy | Feasible but demanding; DPDP standard from day one. Storage is gated on F2 and F3 | OK, conditional |
| WP-02 Upload | Works on low-end phones and patchy networks (`constraint-register.md`, CR-T4) | OK |
| WP-11 Labelled Test Set | Feasible but slow to build; gated on F4 | OK, conditional |
| WP-12 Ops and admin | Feasible; needs its own strong sign-in (CR-T11) | OK |
| WP-09 Feedback | Covered by the standard web app base | OK |
| WP-10 Share from messaging apps | Not assessed on its own | Warning W1 |

## 4. Warnings

| ID | Warning | Suggested handling |
|---|---|---|
| W1 | Sharing a bill straight from a messaging app (S1, WP-10) was not assessed on its own. On mobile web it depends on browser support for receiving shared files, which varies by phone and browser **[assumption]** | Check it in Requirements Analysis. If support is weak, fall back to a plain upload |
| W2 | The rough-mockup changes R-01 to R-08 were approved without being applied. R-01 covers Must Haves DU-11 and RP-06 | Carried forward into Requirements Analysis and Refined Mockups (`decision-log.md`, DL-51) |
| W3 | The bill-correction measure (PRD G-02) is not in the metric tree yet | The founder decides it before the build starts (DL-57) |

## 5. Consistency

- The order of work in `scope-document.md` and `intent-backlog.md` matches the feasibility conditions: the bill-reading trial comes first, and storage waits for residency and hosting.
- The deferral of pay-under-protest guidance is consistent across the scope, the backlog and `constraint-register.md` (CR-R5). Rule C10 still applies to all wording.
- No contradictions were found between the intent, scope, feasibility and approval answers.

## Human approval

- [ ] Ideation to Inception check reviewed (approved through the Approval & Handoff sign-off)

## Assumptions & Open Questions

- W1 rests on an assumption about browser support that has not been checked.
