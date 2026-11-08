# Product Requirements Document
## Indian Medical Bill Audit Platform — codename "Project RS" (until launch)

| Field | Value |
|---|---|
| Document type | Product Requirements Document — Discovery + Requirements |
| Version | 0.14 — "PRD-v2": editorial restructure of v0.13. **No founder decision, requirement priority or target has been changed**, except where stale text contradicted a confirmed decision (listed in Change Summary CS.4). |
| Date | 4 October 2026 |
| Status | Draft for stakeholder review. Open questions (§20) and the review findings (CS.5) must be resolved before v1.0. |
| Audience | CEO and executives; Product; Engineering; Design; Business and Growth; domain experts and counsel (for items marked [VALIDATE]); investors |
| Not covered here | UI and visual design, website implementation, technical architecture, implementation logic of audit rules |

**Conventions**
- **[VALIDATE]** marks a regulatory or market fact from public sources or domain knowledge. A domain expert or counsel must confirm it before it is used in product copy, the rule library or investor material.
- **[HYPOTHESIS]** marks a product bet that the MVP will test.
- **D#** refers to a founder decision (Appendix A). **Q-…** refers to an open question (§20). **G-#** refers to a review finding (CS.5).
- **Phase labels:** **P0** Foundations (before the build), **P1** Hospital Bill Check (the MVP), **P1.5** Expert Review, **P2** Insurance Check, **P3** Scale and B2B2C, **P4** Ecosystem. "MVP" always means Phase 1.

---

## Change Summary (v0.13 → v0.14 / PRD-v2)

### CS.1 Sections reorganised

The document now follows a standard PRD order: **context → strategy → product definition → requirements → delivery and governance → reference.** Requirements are grouped by phase, so Phase 1 reads cleanly and later phases stay fully specified.

| v0.13 location | v2 location | Change |
|---|---|---|
| Front matter: Decisions D1–D30 | Appendix A — Founder Decision Log | Moved out of the front matter. A "Resolves" column was added, and resolved open questions are now recorded there. |
| Front matter: Change log | Appendix D — Version History | Condensed |
| §1 Executive Summary | §1 | Tightened. The Phase 1.5 item no longer sits in the Phase 1 feature list. |
| §2 Critical Assessment | §6 Strategic Considerations | Moved after the market and competition sections, which it builds on. Each item now ends with its current status or the decision that settled it. |
| §3 Problem Statement / §4 Current Process / §5 Business Context | §2 / §3 / §4 | Renumbered. §2.1 gains an "addressed in phase" column. |
| §6 User Personas | §9 Users | Now follows Goals |
| §7 Vision + §10.3 Product Principles | §7 Product Vision and Principles | Merged |
| §8 Competitive Analysis | §5 Competitive Landscape | §8.4, §8.5 and §8.6 merged into one differentiation table (§5.4) |
| §9 Customer Journeys | §11 | The Case/Session definition is now §11.1 |
| §10 Goals + §11 Success Metrics | §8 Goals and Success Metrics | Merged |
| §12 Scope + §20 Future Enhancements + §22.1 MVP definition + §22.3 Roadmap | §10 Scope and Phasing | Merged into one phase model with scope and exit criteria |
| §13 Functional Requirements | §12 | Grouped by phase: Phase 1 modules (§12.1–12.14), Phase 1.5 (§12.15), Phase 2 (§12.16), Won't requirements kept for traceability (§12.17). The Session & Storage module (SD) now sits next to Consent (ON). |
| §14 Audit Intelligence Framework | §13 | Insurance dispute areas and non-payable lists (§14.5–14.6) grouped as a Phase 2 reference (§13.9). Rollout waves now appear in the catalogue's Phase column. |
| §15 NFRs | §14 | — |
| §22.2 MoSCoW | §15 MVP Prioritisation | "Won't Have" merged into the Phase 1 out-of-scope table (§10.3) |
| §16 Assumptions + §22.4 Validation plan | §16 Assumptions and Validation Plan | Merged. Each assumption is now paired with its test. |
| §17 Constraints / §18 Risks / §19 Compliance | §17 / §18 / §19 | Rows put back in numeric order (v0.13 had A18 before A17, C11–C12 before C10, and RK30/RK29/RK20 out of order) |
| §21 Open Questions | §20 | Only open items remain; resolved items are in Appendix A |
| Appendix A Glossary / B Sources | Appendix B / C | About 25 missing terms added to the glossary |

### CS.2 Duplicates removed

| Duplicated content | Where it appeared in v0.13 | Kept in v2 |
|---|---|---|
| Out-of-scope list (≈20 items stated twice) | §12.4 and §22.2 Won't Have (W1–W22) | §10.3, one table that keeps the W-IDs |
| Phase 1.5 / 2 / 3 / 4 scope | §12.2, §12.3, §20, §22.3, §9.4, §1 | §10 (journeys in §11.5 only list journey names) |
| MVP definition and launch shape | §1, §12.1, §22.1, §22.3 | §10.2 (summary in §1) |
| Launch and Wave 2–3 check lists | §14.8, §22.2 M8, §22.2 S3 | Phase column of the catalogue (§13.6) plus the counts in §13.8 |
| Check counts (115 / 59 / 38) | §1, §14.8 | §13.8 |
| Bill-only MVP differentiation (five points) | §2.9 pt 1, §8.4, §8.6, RK18 | §5.4 |
| Bill-only MVP risks | §2.9 and RK18–RK20 | Risk register; §6.9 keeps the trade-off and points to the register |
| Finding-type logic and "Who to Ask" | §2.3, §2.4, §14.1 | §13.1; §6.3–6.4 keep only the rationale |
| Legal-services constraints | §2.8, §19.2, §12.4 | §19.2 |
| Validation targets | §11.2, §16, §22.4 (e.g., action intent ≥ 40%, NPS ≥ 40, download ≥ 70%) | Targets live in §8.4; §16 points to them |
| Confirmed decisions restated as assumptions | A1–A3 duplicated D1, D2, D5 | Appendix A |
| English-only assumption | A10 and A17 | Merged as A10 / A17 |
| "English only" requirement | ON-09, RP-05, DP-08, NFR-14, §12.1, §12.4 | ON-09 (DP-08 keeps only its behaviour) |
| "No sign-up, mobile number or verification" | About 12 places | ON-01 and SD-01/SD-02; elsewhere by reference |
| "No legal review before launch (D27)" | NFR-18, C12, RK28, RK3, RK5, §19.6, §21.3, D27 | D27, C12, RK28 and the §19 intro. Removed from NFR-18, RK3 and RK5, where it was not a quality attribute or a mitigation. |
| Resolved risk RK25 | §18 | Removed (resolved by D10; noted in Appendix A) |
| Resolved open questions (18 items) | §21 | Appendix A "Resolves" column |
| Per-heading version tags ("added in v0.12") | Many headings | Appendix D |
| A broken table row ("Insurer-specific claim document checklists") | Below a paragraph in §14.9 | Moved back into the reference-data table (§13.10) |

### CS.3 Major consolidations

1. **One phase model (§10).** Scope, future enhancements, MVP definition and roadmap were spread across five sections. They are now one table of phase, focus and exit criteria, followed by scope per phase.
2. **Requirements grouped by phase (§12).** Phase 1 modules contain only M/S/C requirements for Phase 1. All P1.5, P2 and W requirements are kept with their original IDs and wording in dedicated subsections, so no traceability is lost.
3. **Assumptions and validation plan merged (§16).** Each assumption now sits on the same row as its hypothesis, test and success signal (H1–H9 kept).
4. **Differentiation merged (§5.4).** One table shows each differentiator and the phase in which it is available.
5. **Goals and metrics merged (§8)** so each goal sits next to the metric that measures it.
6. **Catalogue rollout made single-source (§13.6, §13.8).** Each check's rollout wave (Launch, W2, W3, Guidance) is stated once, in the catalogue.
7. **Terminology standardised.**
   - Phases: P0–P4 (see Conventions).
   - Hospital Bill Check / Insurance Check / Expert Review are always capitalised.
   - "Audit Report" replaces "Full Audit Report", "Full Report" and "report".
   - "Labelled Test Set" replaces "Phase 0 labelled set" and "labelled bill set".
   - The mapping between finding classes and their user-facing labels ("Likely error", "Worth asking") is stated once (§13.1.1).
   - A legend was added for the "Who to ask" column.

### CS.4 Text corrected to match confirmed decisions

These statements in v0.13 contradicted later founder decisions. They have been aligned with the decision. **Please confirm none of them was intentional.**

| # | v0.13 location | Stale text | v2 text | Basis |
|---|---|---|---|---|
| 1 | §12.1 In scope | "Payments for paid tiers" | Removed | D25 (Phase 1 free) |
| 2 | §9 intro | "A user may own several cases and share a case with family members" | One case per session; no sharing | D6, D7 |
| 3 | RE-08 | "…and remind the user before deadlines" | Deadlines shown in the report; no reminders | D6, AD-03 |
| 4 | §22.2 M7 | "human ops fallback for low-confidence extraction" | User confirmation of low-confidence fields (DP-22) | D11 |
| 5 | §22.2 M19 | "expert-only switches" | "on/off and shadow-mode switches" | D11 |
| 6 | KB-01 | "Domain experts can maintain the check catalogue…" | "The rule owners (the product team in Phase 1)…" | D22 |
| 7 | OP-03 | Phase 1 support "handles payment refunds" | Refunds by payment reference from P1.5 | D25 |
| 8 | RK4 mitigation | "legal opinion before launch" | Legal opinion before P1.5 (§19.6) | D27 |
| 9 | RK7 mitigation | "human review threshold" | User confirmation plus a "Complex" label above the value threshold | D11 |
| 10 | RK8, RK10, RK11, RK16 mitigations | Relied on insurer documents, expert letters, a "domain owner" and DOC-09 | Each now tagged with the phase in which it becomes available | D5, D10, D22 |
| 11 | RK18 mitigation | "keep the Free Scan competitive" | "keep the free bill check competitive" (Phase 1 has no Free Scan tier) | D25; see G-01 |
| 12 | §8.3 feature gap | Our product: "Escalation ladder + deadline tracking: Yes"; "Outcome tracking: Yes" | Ladder in report, no tracking; in-session outcomes only | D7, D12 |
| 13 | §10.3 Principle 7 | "easy deletion" | In-session discard (ON-07) | D6, D8 |
| 14 | §19.4 minors row | "open issue (Q-P10)" | Allowed on declaration (D15); accepted risk RK24 | D15 |
| 15 | §14.6 note | "[VALIDATE … Q-M6]" as open | Founder decision D16; legal review pending under D27 | D16 |
| 16 | Glossary, Verified Patient Value | "(our North Star)" | North Star once outcomes can be tracked | D13 |
| 17 | §3.2 | 10–30% out-of-pocket "to be measured from MVP data" | To be measured from P2 data (the MVP does not read insurer deductions) | D5 |
| 18 | DP-08 | Whole requirement tagged W | Split: listing unread lines = M; reading them = W | D9; the behaviour is assumed by the §8.4 metric and RK23 |
| 19 | NFR-08 | "strong authentication" | Clarified as authentication of internal staff (users have no login) | D6 |
| 20 | §22.2 MoSCoW refs | M13 cited RP-06–RP-08, which includes RP-07 (a P1.5 requirement). Several Must/Should requirements added in v0.11–v0.13 (SD-10, LT-01–LT-13, DP-14, OP-05) were not referenced at all. | RP-07 removed from M13. SD-10 added to M17, LT-01–LT-11 to M15, and DP-14, LT-12, LT-13 (Should) and OP-05 (Could) added. | Requirement priorities in §12 |

### CS.5 Gaps, inconsistencies and open questions found in review

These are new. They are not yet in §20 because each needs a founder or owner decision first.

| ID | Priority | Issue | Where | Why it matters | Suggested resolution |
|---|---|---|---|---|---|
| G-01 | High | **What is free and what is paid after Phase 1 is unclear.** PY-01 ("pay for Full Audit Report") and PY-03 ("Free Scan") suggest the bill check becomes partly paid in P1.5, but D25 only covers Phase 1, and §4.4 lists no Free Scan tier. | PY-01, PY-03, §4.4, RK18 | Pricing, positioning against free checkers, North Star continuity | Decide whether the Hospital Bill Check stays free in P1.5 and only Expert Review is paid. Then update PY-01/PY-03. |
| G-02 | High | **The core value assumption cannot be tested in Phase 1.** A7 (hospitals revise bills when shown evidence) is "validated by outcome tracking", but Phase 1 has no outcome tracking (D12). No metric measures whether the billing desk accepts the summary, although every Phase 1 outcome depends on it (§9.3). | A7, §8.4, §9.3 | PMF could be declared on findings that never turn into corrections | Add a measure: e.g., an in-session question for already-paid bills; a beta-tester follow-up outside the product (LT-04); or a small structured study with billing desks. |
| G-03 | High | **There is no source for "typical quantity" data in Phase 1.** Plausibility checks (DUP-02, LOS-04, PF-01, RX-04, CON-03) need typical ranges, which §13.10 sources from an "expert panel + case data". Phase 1 has no experts (D11, D22), and case data is kept only 3 months (D24). | §13.10, Wave 2–3 checks | Wave 3 may be blocked | Name a public source, or derive ranges from the Labelled Test Set (which has no 3-month limit, D30), or move these checks to P1.5. |
| G-04 | High | **Must Haves depend on Wave 3 checks.** M8 lists AA-03 (unusual charges) and AA-04 (excessive consumables) as Must, but the checks that deliver them (CLN-01, CON-03) are Wave 3, after launch. | §15 M8, §13.6 | Unclear launch acceptance criteria | Clarify that "Must" for AA-0x means the engine capability is built, not that the check is visible at launch, or move those AA items to Should. |
| G-05 | High | **The benchmark moat conflicts with 3-month retention.** BG3 and the "data moat" (§1, §5.4) assume a growing dataset of bills, deductions and outcomes. D24 deletes stored data after 3 months, and Phase 1 holds no deductions or outcomes. RK29 rates this risk at only 6. | BG3, §5.4, RK29, Q-P12 | Investor narrative vs actual data strategy | Decide Q-P12 earlier than Phase 2, or restate BG3 for Phase 1. Consider raising RK29. |
| G-06 | Medium | **"Join" implies contact details.** H7's success signal is "≥ 30% of insured users join", which implies a waitlist, but no contact details may be collected (D6). | §16 (A15/H7), S8 | Metric cannot be measured as written | Restate as "≥ 30% of insured users click 'I want the Insurance Check'" (an anonymous count). |
| G-07 | Medium | **Phase 2 and later features need an identity model.** Family Plan (annual subscription), Policy Health Check as a "reason to return", deadline reminders and phone numbers (P2) all need users to come back, which D6/D7 rule out. | §4.4, §6.1, §10.5 | Phase 2 business case rests on retention | Add a Phase 2 question: identity and return model. |
| G-08 | Medium | **The Phase 2 target date is deferred, but a mitigation depends on it.** §6.9 recommends setting it *before* the MVP launch, and RK20's mitigation relies on it, yet Q-B10 is deferred. | Q-B10, §6.9, RK20 | Roadmap stall | Set a target date before launch, or change the RK20 mitigation. |
| G-09 | Medium | **Device-based abuse limits may conflict with "no profile".** Per-device limits (SD-09) may be personal data under DPDP, and may contradict "no device-linked profile" (SD-02). | SD-02, SD-09 | Privacy promise | Define abuse controls that keep no persistent device identifier, or state an exception. |
| G-10 | Medium | **Session timeout is not defined.** SD-08 says "a defined period of inactivity". Families at the billing desk may step away, and RK21 (lost reports) depends on this value. | SD-08, RK21 | Lost reports; UX | Set a value, and decide whether a warning is shown before expiry. |
| G-11 | Medium | **Phase 1 escalation deadlines have no reference data.** RE-08 shows hospital-side ladder deadlines (e.g., Consumer Commission limitation period), but §13.10 lists grievance timelines as Phase 2 only. | RE-08, §13.10 | Possible wrong deadlines in reports | Add hospital-side escalation timelines to the Phase 1 reference data. |
| G-12 | Medium | **"Pay under protest" guidance has no requirement.** RK15 (impact 5) and C10 rely on "pay under protest" scripts, and TS-03 (urgent guidance) is only a Should. | RK15, C10, TS-03 | Patient-welfare risk | Add a requirement for pay-under-protest guidance; consider raising TS-03 to Must. |
| G-13 | Medium | **Owners named in the register do not exist in Phase 1.** Risk owners "Domain", "Legal", "Data" and §13.10 owners ("Insurance domain lead", "Healthcare domain lead", "Data science") are not in the Phase 1 team (D22). DPO appointment "early" (§19.4) also has no owner. | §18, §13.10, §19.4 | Accountability | Map each to a named person in the product team for Phase 1. |
| G-14 | Medium | **Data residency is a recommendation, not a decision.** NFR-10 is marked [recommendation], and AI-vendor processing (NFR-20, hidden risk 4) may take place outside India. | NFR-10, NFR-20 | DPDP and vendor choice | Decide before vendor selection. |
| G-15 | Medium | **The self-pay market is not sized.** §4.3 sizes the SAM from insured claims only, while the primary Phase 1 segment is self-pay patients (Q-M2). | §4.3 | Market case for the MVP | Add a self-pay hospitalisation sizing layer (NSSO/NHA). |
| G-16 | Medium | **The minimum test-set size per check is not defined.** The ≥ 90% precision gate is measured over 300–500 bills, but some checks (e.g., IMP-02 implants, PKG-01 packages) may have few qualifying bills. | LT-01, §8.4, §13.8 | A check could "pass" on too few examples | Set a minimum number of positive and negative examples per check before promotion. |
| G-17 | Low | **Does the closed beta continue after launch?** LT-04 calls it "pre-launch", but LT-13 tops up the set from "network or beta" after launch. Beta participants' personal data handling is not specified. | LT-04, LT-13 | D8 exception scope | Confirm whether the beta continues and how tester data is governed. |
| G-18 | Low | **Two catalogue routes are not defined as Who-to-Ask labels.** "User" (DOC-02) and "Expert" (P2 checks) are used in the catalogue but are not labels in §13.1.3. | §13.6 | Consistent report wording | Confirm the wording shown to users. A legend has been added for now. |
| G-19 | Low | **Some recommendations have no requirement.** The report watermark (hidden risk 5), the public data charter (hidden risk 3) and model/vendor portability (hidden risk 4) are recommended but have no requirement IDs. | §18.1 | May be lost in the backlog | Add requirements or log them as decisions. |
| G-20 | Low | **Interim carry-forward check depends on having both bills.** DUP-07 is in the launch set but works only if the user uploads the interim and final bills in the same session (D7). | DUP-07 | Expectation management | Say this in capture guidance (DU-10). |
| G-21 | Low | **Requirements have no acceptance criteria.** Several thresholds are undefined: the value threshold for "Complex" (Q-P2), the collapse threshold for low-confidence findings (§13.1.2) and the session timeout (G-10). | §12 | Engineering readiness | Add acceptance criteria during backlog refinement. |
| G-22 | Low | **IRDAI list mapping appears in two places.** DP-13 (M) maps consumables to the "IRDAI non-payable lists", while insurance attributes are DP-18b (P2). This overlaps with DP-18 (unbundling tags, M). | DP-13, DP-18, DP-18b | Possible scope creep into insurance | Confirm DP-13 is limited to the Lists II–IV unbundling reference (D16). |
| G-23 | Low | **The ID "C1/C2" means two different things.** It is used both for Constraints (§17) and for MoSCoW Could-Have items (§15.3). This was inherited from v0.13. | §15.3, §17 | Ambiguous references in backlogs | Rename the Could-Have items (e.g., CH1, CH2) at v1.0. They are left unchanged for now to keep traceability. |

### CS.6 What did not change
- All founder decisions (D1–D30), requirement IDs, check IDs, risk IDs, assumption IDs, metric targets and [VALIDATE]/[HYPOTHESIS] tags are kept.
- No requirement priority was changed. The only exception is the DP-08 split (CS.4 #18).
- All domain content is kept: regulatory facts, escalation ladders, IRDAI lists, the error and overcharging taxonomies, risk indicators, the 115-check catalogue, personas, competitor analysis and sources.

---

## Table of Contents

**Part I — Context and Strategy**
1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Current Process and Pain Points](#3-current-process-and-pain-points)
4. [Market and Business Context](#4-market-and-business-context)
5. [Competitive Landscape](#5-competitive-landscape)
6. [Strategic Considerations](#6-strategic-considerations)

**Part II — Product Definition**

7. [Product Vision and Principles](#7-product-vision-and-principles)
8. [Goals and Success Metrics](#8-goals-and-success-metrics)
9. [Users](#9-users)
10. [Scope and Phasing](#10-scope-and-phasing)
11. [Customer Journeys](#11-customer-journeys)

**Part III — Requirements**

12. [Functional Requirements](#12-functional-requirements)
13. [Audit Intelligence Framework](#13-audit-intelligence-framework)
14. [Non-Functional Requirements](#14-non-functional-requirements)

**Part IV — Delivery and Governance**

15. [MVP Prioritisation (MoSCoW)](#15-mvp-prioritisation-moscow)
16. [Assumptions and Validation Plan](#16-assumptions-and-validation-plan)
17. [Constraints](#17-constraints)
18. [Risks](#18-risks)
19. [Compliance and Regulatory Considerations](#19-compliance-and-regulatory-considerations)
20. [Open Questions](#20-open-questions)

**Appendices**

- [Appendix A — Founder Decision Log](#appendix-a--founder-decision-log)
- [Appendix B — Glossary](#appendix-b--glossary)
- [Appendix C — Sources](#appendix-c--sources)
- [Appendix D — Version History](#appendix-d--version-history)

---

# Part I — Context and Strategy

## 1. Executive Summary

**The problem.** Indian hospital bills are long and written in hospital-specific jargon. They arrive at the worst moment: at discharge, when the family is exhausted and under pressure to pay and leave. Patients rarely know whether the hospital billed them correctly, and they have no one to check. For the hundreds of millions of Indians with health insurance **[VALIDATE coverage figure]**, a second opaque step follows. The insurer or TPA applies room-rent caps, proportionate deductions, co-pays, sub-limits and "non-medical" exclusions, then hands the patient a residual amount to pay, often with only a one-line reason.

**Evidence of scale**
- Indian general and standalone health insurers settled ~32.6 million health claims in FY25, paying out ~₹94,248 crore; ~8% of claims were repudiated outright (IRDAI annual report, as reported). **[VALIDATE]**
- The Government told Parliament that health claims worth ~₹26,037 crore were rejected or disallowed in FY24. **[VALIDATE]**
- In a LocalCircles survey of 39,000+ respondents, only 25% said their claims in the past three years were fully approved; 33% said they were only partly approved "with invalid reasons". (Self-reported; directional only.)
- The Insurance Ombudsman resolved only 41% of health complaints in the complainant's favour or by mutual settlement in FY25, down from 55% in FY24. Success depends heavily on how well a complaint is framed and documented. **[VALIDATE]**
- Consumer commissions have ordered hospitals to refund unjustified charges and pay compensation where bills lacked transparency — but only for patients who knew to fight.

**The product**

*Phase 1 (MVP): Hospital Bill Check.* The hospital hands the patient a bill (interim or final). The patient or a family member uploads it, optionally with the discharge summary, pharmacy bills, implant invoices and the cost estimate, and the platform checks whether they are being overcharged. It:
1. extracts and structures every line item;
2. runs a library of hospital-billing checks: arithmetic, duplicates, room days vs length of stay, quantity plausibility, unbundling, package violations, charges not supported by the discharge summary, and breaches of printed MRP and NPPA price caps;
3. produces a plain-language **Audit Report** with evidence and confidence levels, including a **Bill Breakdown** (likely errors / worth asking / explained / not assessable), a one-page **Billing Desk Summary** and ready-to-use questions for the hospital.

The bill check works for every patient, insured or not. It matters most to **self-paying patients**, who bear 100% of any overcharge with no insurer scrutinising the bill, and to **reimbursement claimants**, who pay upfront. For insured patients, overcharging still uses up the sum insured and often becomes deductions the patient pays.

*Phase 1.5: Expert Review.* Optional paid review by a human billing expert, delivered by email.

*Phase 2: Insurance Check.* Adds the policy and the insurer's settlement letter: policy-application checks, settlement reconciliation, reimbursement readiness and insurer-side escalation. This completes the **three-way Bill ↔ Policy ↔ Settlement reconciliation**, which tells the user, for every rupee, whether to challenge the hospital, the insurer, or nobody.

**Why we can win.** Today's options fall into three groups:
- free web checkers (e.g., eVaakil) that give generic flags;
- grievance firms (e.g., Insurance Samadhan) that step in only *after* a claim rejection and take a small fraction of cases;
- broker and employer claim desks (Ditto, Policybazaar, Plum, Loop) that serve only their own customers and do not audit the hospital.

In Phase 1 we win on **depth and trust**: line-level extraction, objective findings a billing desk cannot easily dismiss, consistency with the discharge summary, and follow-through (§5.4). Phase 2 adds the three-way reconciliation nobody offers. The long-term moat is an anonymised **price-and-outcome benchmark dataset** (see G-05).

**MVP recommendation.**
- Launch the Hospital Bill Check **free, all-India, on mobile and desktop web, in English**, about 3–4 months after the team is in place.
- Use is **anonymous and single-session**.
- It works during the stay (interim bills), at discharge and after payment.
- **18 objective checks go live at launch.** More follow as each passes testing against the Labelled Test Set (§13.8).
- Details: §10.2 (scope) and §15 (prioritisation).

**Top risks for leadership**
1. B2C unit economics for an infrequent event (§6.1, RK1).
2. A bill-only, expert-free MVP can look like free bill checkers; it must win on depth and evidence (§6.9, RK18).
3. There is no legally binding "standard rate" for private hospitals, so pricing findings must be framed as *questions*, not accusations (§6.3, RK3).
4. Regulatory perimeter. In the MVP this is mainly DPDP consent for third-party and minors' health data. IRDAI intermediary questions become active in Phase 2 (§19).
5. The discharge window is short. The product must be fast and ideally used *during* the stay (§6.2).

---

## 2. Problem Statement

### 2.1 What problem does the product solve?
Patients and their families cannot tell whether the amount they are asked to pay for a hospitalisation is **correct, justified and consistent with their insurance policy**. They also lack the knowledge, time and evidence to challenge it effectively.

| # | Sub-problem | Description | Addressed in |
|---|---|---|---|
| P1 | **Bill opacity** | Bills run to dozens of pages, use internal codes and abbreviations, split one service into many lines, and mix room, pharmacy, consumables, professional fees and packages. | Phase 1 |
| P2 | **Policy opacity** | Policy wordings contain room-rent caps, proportionate deduction clauses, co-pays, disease-wise sub-limits, waiting periods and non-payable lists that most policyholders have never read. | Phase 2 |
| P3 | **Settlement opacity** | Settlement letters show "amount claimed", "amount deducted" and "amount approved" with terse reasons ("non-medical", "as per policy", "excess of reasonable charges") and no line-by-line mapping. | Phase 2 |
| P4 | **Action gap** | Even when users suspect a problem, they do not know whom to approach (hospital or insurer), what to say, what evidence to attach, what the deadlines are, or how to escalate. | Phase 1 (hospital); Phase 2 (insurer) |

### 2.2 Why it matters
- **Financial.** Out-of-pocket spending is a large share of health expenditure in India; National Health Accounts estimates put it at roughly 40% of total health expenditure in recent years **[VALIDATE]**. A single hospitalisation can wipe out months of household savings. Even insured families routinely pay 10–30% of the bill themselves through deductions and non-payables **[HYPOTHESIS — to be measured from Phase 2 data; the bill-only MVP does not read insurer deductions]**.
- **Systemic.** Opaque billing erodes trust in private healthcare and health insurance, which reduces insurance uptake and renewal.
- **Regulatory momentum.** IRDAI's 2024 Master Circular, the Supreme Court's attention to hospital rate standardisation, NHCX and the DPDP Act all push towards transparency. Together they open a window for a consumer-side transparency product (§4.2).

### 2.3 Who experiences the problem
- Insured patients using **cashless** treatment, who pay non-payables and deductions at discharge.
- Insured patients filing **reimbursement** claims, who pay the full bill upfront and then fight for reimbursement.
- **Uninsured / self-paying** patients, who bear 100% of the bill with no insurer scrutinising it.
- **Family members** who manage the process for the patient, often remotely (children of elderly parents, NRIs).
- **Corporate employees** whose group policies have caps and co-pays they do not know about.
- **Professional claim consultants** who do this work manually.

### 2.4 Financial and emotional impact

| Dimension | Impact |
|---|---|
| Direct financial loss | Paying for duplicated, unbundled, inflated or wrongly deducted items; losing reimbursement through missing documents or missed deadlines |
| Opportunity cost | Days spent chasing hospitals, TPAs and insurers; leave from work; travel |
| Liquidity stress | Arranging cash at discharge at short notice; borrowing at high interest; delayed reimbursements |
| Emotional burden | Decisions made while grieving, sleep-deprived or caring for a sick relative; fear that questioning the bill will affect treatment; guilt and helplessness |
| Power asymmetry | Hospitals and insurers have billing and claims experts; the family has nobody |
| Trust erosion | Belief that "the system is rigged", leading to under-insurance or avoiding care |

---

## 3. Current Process and Pain Points

### 3.1 How patients receive hospital bills
1. **At admission.** A deposit or advance is requested, sometimes with a cost estimate (often verbal or a single number). The room category is chosen under time pressure, frequently without reference to the policy's room-rent limit.
2. **During the stay.** Interim bills are rarely shared proactively, and families may be asked to "top up" the deposit.
3. **At discharge.** A final bill is generated, often a summary bill (category totals) rather than an itemised one. Pharmacy and consumables may be on separate bills. Implant invoices and stickers are often not given unless requested.
4. **Cashless patients.** The hospital sends the final bill to the TPA/insurer, which returns a final approval. The hospital then asks the family to pay the difference ("patient payable"). The family usually sees only the final number.

**Pain points:** no proactive itemisation; summary bills; payment demanded before explanation; no time to review; fear of delaying discharge.

### 3.2 How patients interact with insurers
- **Cashless.** The hospital requests pre-authorisation, the insurer/TPA approves an initial amount, enhancements follow during the stay, and final approval comes at discharge. The patient is largely a bystander; communication runs between the hospital's TPA desk and the TPA/insurer.
- **Reimbursement.** The patient pays in full and intimates the insurer within policy-specific deadlines, often 24–48 hours of an emergency admission **[VALIDATE per policy]**. They then submit the claim form and documents within a policy-specific window, the insurer raises queries, and the claim is settled or rejected.
- Interaction happens across call centres, emails, apps and TPA portals. One case often involves the hospital, a TPA and an insurer, each blaming the others.

**Pain points:** fragmented communication; repeated document requests; jargon; slow query cycles; no single view of status.

### 3.3 How patients review claim settlements
- The settlement letter ("claim settlement advice") shows claimed, deducted and approved amounts and a short list of reasons.
- There is usually no mapping from each deduction to a bill line and a policy clause.
- Patients rarely re-read their policy wording, so they cannot tell a correct deduction from an incorrect one.

**Pain points:** no line-level reconciliation; vague reasons; no comparison with the policy; complex proportionate-deduction maths.

### 3.4 How patients identify billing issues today
Intuition ("₹4,000 for gloves seems too much"); asking friends or relatives in healthcare; online forums and social media; hiring a claim consultant or lawyer (rare, mostly for large amounts); free web tools that give generic checks without policy context.

**Pain points:** no benchmark; no structured method; no evidence; issues discovered too late.

### 3.5 How patients raise disputes today

This table is also the basis for the escalation ladders in the product (RE-08).

| Stage | Hospital-side dispute | Insurer-side dispute |
|---|---|---|
| 1. First contact | Billing desk / patient relations / TPA desk | Insurer customer care / TPA; formal complaint to the insurer's Grievance Redressal Officer (GRO) |
| 2. Internal escalation | Medical Superintendent / hospital grievance officer | Insurer GRO must respond within IRDAI-prescribed timelines (commonly cited as 14 days) **[VALIDATE]** |
| 3. Regulator / administrative | State health department / Clinical Establishments authority (where the Act is adopted or State law applies); NPPA for drug or device prices above MRP or ceiling price | IRDAI's **Bima Bharosa** grievance portal |
| 4. Quasi-judicial | — | **Insurance Ombudsman**: free; claims up to ₹50 lakh; generally after the insurer rejects the complaint or does not respond within one month, and within one year of the insurer's reply **[VALIDATE]** |
| 5. Judicial | Consumer Disputes Redressal Commission (District / State / National by value) | Consumer Commission; civil courts |

**Pain points:** users do not know which ladder to climb, miss deadlines, write emotional rather than evidence-based complaints, fail to cite policy clauses or regulations, and give up.

### 3.6 Pain-point map

| # | Pain point | Felt most by | Severity | Addressed by | Phase |
|---|---|---|---|---|---|
| PP1 | No itemised bill / can't read it | All | High | Document checklist, extraction, plain-language explanations | P1 |
| PP2 | No time at discharge | Cashless patients, families | High | Fast Hospital Bill Check; interim-bill checks | P1 |
| PP3 | Don't know policy limits | Policyholders, employees | High | Policy Understanding, room-rent advisor | P2 |
| PP4 | Settlement deductions unexplained | Policyholders | High | Settlement reconciliation | P2 |
| PP5 | Don't know if a price is reasonable | All (esp. self-pay) | Medium | Benchmarks | Phased |
| PP6 | Reimbursement rejected for paperwork | Reimbursement claimants | High | Reimbursement Readiness | P2 |
| PP7 | Don't know whom to complain to or how | All | High | Who to Ask, templates, escalation ladder | P1 (hospital); P2 (insurer) |
| PP8 | Fear and emotional stress | Families, seniors | High | Calm, neutral, step-by-step guidance; human expert | P1; expert P1.5 |
| PP9 | Managing remotely | NRIs, working children | Medium | Downloadable report the user can forward (no shared case access, D7) | P1 |
| PP10 | Expert help is expensive or selective | Middle-income users | Medium | Free AI audit; paid expert tier | P1; P1.5 |

---

## 4. Market and Business Context

### 4.1 Ecosystem participants

| Participant | Role | Relationship to our product |
|---|---|---|
| Patients and families | Pay, claim, dispute | Primary users (B2C) |
| Hospitals (private, trust, corporate chains, nursing homes) | Bill the patient and/or insurer | Subject of billing checks; potential friction; long term, could adopt "audit-ready billing" |
| Insurers (general and standalone health) | Underwrite, settle claims | Subject of policy and settlement checks (P2) |
| TPAs | Process claims for insurers | Subject of settlement checks (P2) |
| Brokers, corporate agents, web aggregators | Sell policies, offer claim support | P3 distribution partners |
| Employers / HR | Buy group policies | P3 B2B2C customers |
| Claim consultants, patient advocates | Paid help to patients | P3 professional users |
| Regulators: IRDAI, NPPA, MoHFW / State health departments, NHA (ABDM/NHCX/PM-JAY), Data Protection Board | Set rules | Source of rules; compliance perimeter |
| Grievance bodies: insurer GROs, Bima Bharosa, Insurance Ombudsman, Consumer Commissions | Resolve disputes | Destinations of our escalation guidance |

### 4.2 Why now: regulatory and market forces
- **IRDAI Master Circular on Health Insurance (May 2024):**
  - cashless pre-authorisation decision within 1 hour, and final discharge authorisation within 3 hours;
  - the insurer bears extra hospital charges caused by delay;
  - insurers/TPAs are expected to collect documents from hospitals rather than burden policyholders. **[VALIDATE exact clauses]**
- **IRDAI standardisation of non-payables (2019–2020 guidelines)** — four lists of items (§13.9) that give a **public, structured rule-set** the audit can apply.
- **Proportionate deduction rules** — IRDAI guidelines state that proportionate deduction cannot be applied to pharmacy, implants/medical devices and diagnostics, and (per guidelines) not to ICU charges **[VALIDATE current applicability]**. This is a frequent, checkable source of wrong deductions.
- **Supreme Court attention on hospital rate standardisation (2024–2025)** — public discussion of fair pricing.
- **NPPA price controls** — ceiling prices on scheduled formulations (DPCO 2013, NLEM) and caps on coronary stents and knee implants.
- **NHCX (National Health Claims Exchange)** — digitising claims; a long-term route to structured claim data with consent.
- **DPDP Act 2023 and DPDP Rules (notified 13 Nov 2025)** — substantive obligations are enforceable from 13 May 2027, and consent managers can register from 13 Nov 2026 **[VALIDATE]**. This raises the bar for handling health data but also legitimises consented data flows.
- **Generative AI and document understanding** — makes extracting line items from photographed, multi-page, inconsistent bills affordable at consumer price points.

### 4.3 Market sizing approach (to be completed with validated data)
We deliberately do not publish a TAM figure in this draft; the inputs below must be validated first. A self-pay sizing layer is still missing (G-15).

| Layer | Definition | Inputs needed | Indicative anchor |
|---|---|---|---|
| TAM (events) | All in-patient hospitalisations in private facilities per year (insured + self-pay) | NSSO health survey, NHA data | — |
| SAM (events) | Privately insured hospitalisation claims (retail + group), urban and semi-urban | IRDAI annual report claim counts by segment | ~32.6M health claims settled in FY25 (all segments, incl. some government schemes handled by insurers) **[VALIDATE segment split]** |
| SOM (Years 1–2) | Users reached in launch markets through digital channels | Funnel conversion assumptions | To be modelled after the pilot |
| Value pool | Patient out-of-pocket on insured hospitalisations (deductions + non-payables + co-pays) plus self-pay hospitalisation spend | Claim deduction ratios, survey data | ₹26,037 crore of claims rejected or disallowed in FY24 **[VALIDATE]** |

### 4.4 Business model and pricing hypotheses
B2C first; B2B in Phase 3 (D1). **[HYPOTHESIS — pricing to be tested]**

| Tier | What the user gets | Indicative price point to test | Phase |
|---|---|---|---|
| Hospital Bill Check | Full audit: extraction, all live checks, Bill Breakdown, Billing Desk Summary, templates, escalation guidance | **Free** (D25) | P1 |
| Expert Review | A human billing expert reviews the AI findings, adds commentary, prioritises actions and drafts a tailored letter, within a set turnaround | Low four-figure ₹ (e.g., ₹1,499–₹4,999) by bill size / urgency | P1.5 |
| Insurance Check | Adds policy and settlement analysis to the bill check | Higher tier or add-on (Q-B9) | P2 |
| Family Plan | Policy Health Check for family policies + discounted audits for a year | Annual subscription (needs an identity model, G-07) | P2+ |
| B2B lines | Employer benefit (per employee per year), broker white-label, claim-consultant SaaS seats | To be set | P3 |

---

## 5. Competitive Landscape

> Competitor information comes from public sources as of October 2026 and should be refreshed before investor use. Several players do not publish pricing.

### 5.1 Landscape

| Category | Examples | Primary customer | Core offering |
|---|---|---|---|
| A. Free online bill checkers | eVaakil Indian Medical Bill Checker | Consumers (lead generation for legal services) | Upload a bill; flags IRDAI non-payables, unbundling, room-rent cap impact; query templates |
| B. Claim grievance / dispute services | Insurance Samadhan, Bima Samadhan, local claim consultants, legal-services marketplaces | Consumers after a rejection | Draft complaints, follow up with insurers, Ombudsman/legal escalation; selective intake |
| C. In-hospital claim facilitation | ClaimBuddy (250+ partner hospitals; $5M Series A, Apr 2024) | Hospitals + patients | Desk staff at hospitals to file claims correctly and track cashless claims |
| D. Broker / advisory claim support | Ditto, Beshak, Policybazaar, Turtlemint, Jio Insurance Brokers | Their own policy buyers | Claim assistance and advocacy for customers who bought through them |
| E. Employer health-benefit platforms | Plum, Loop, Onsurity; TPA apps (e.g., Medi Assist) | Employers / employees | Group policy servicing, claim tracking, claim support |
| F. Hospital revenue-cycle / B2B claims software | Care.fi and others | Hospitals | Faster claim submission, fewer denials, billing optimisation |
| G. Payer-side FWA analytics | Insurer/TPA internal teams and vendors | Insurers | Detect hospital over-billing and fraud (payer-aligned) |
| H. Government grievance and infrastructure | Bima Bharosa, Insurance Ombudsman, NHCX, PM-JAY grievance | Public | Grievance registration and adjudication; claims exchange infrastructure |
| I. International analogues | Goodbill (US; 20% contingency fee capped at $1,000; billing coders review bills against medical records), Resolve (US; reports average savings of 60–65%) | US patients | Bill error detection plus negotiation for patients |

### 5.2 Strengths and weaknesses

| Competitor / category | Strengths | Weaknesses |
|---|---|---|
| eVaakil bill checker | Free; clear consumer framing; references IRDAI non-payables and room-rent capping; query templates; legal-brand trust | Analyses the bill largely in isolation; no visible settlement reconciliation; no human review path in the tool; positioned as lead generation for legal services; no evident outcome tracking |
| Insurance Samadhan / grievance firms | Deep dispute experience; Ombudsman and legal know-how; success-based alignment | Post-rejection only; highly selective intake (reported ~3–4 of 100 cases); slow; not for small amounts; no billing audit of the hospital |
| ClaimBuddy (in-hospital) | Physically present at the point of care; hospital relationships; operational scale | Hospital-aligned business model (hospitals are clients), so little incentive to challenge hospital billing; limited to partner hospitals |
| Brokers (Ditto, Policybazaar, etc.) | Trust with their customers; insurer relationships; claim advocacy | Only for their own policyholders; focus on the insurer side, not hospital billing; inconsistent quality reported by some users |
| Employer platforms (Plum, Loop, etc.) | Embedded in employee benefits; high engagement | Employer-paid and dependent on insurer relationships; focus on servicing, not auditing; only for group policies they manage |
| Hospital RCM (Care.fi etc.) | Data and workflow inside hospitals | Hospital-side; goal is maximising hospital collections |
| Payer FWA | Large data sets; sophisticated rules | Insurer-aligned; outputs not available to patients |
| Ombudsman / Bima Bharosa | Free; authoritative; binding awards (Ombudsman) | Slow; user must frame the case; no help preparing evidence; insurer side only |
| Goodbill / Resolve (US) | Proven audit + negotiation model; outcome-based pricing | US-specific (CPT/ICD coding, chargemaster pricing); not in India; the contingency model may not translate legally |

### 5.3 Feature gap analysis

| Capability | A. Free checkers | B. Grievance firms | C. In-hospital | D. Brokers | E. Employer | **Our product (target)** |
|---|---|---|---|---|---|---|
| Line-item extraction from the bill | Partial | Manual | Manual | Manual | Partial | **P1** |
| Hospital billing error checks | Partial | No | No | No | No | **P1** |
| Policy-aware checks (room cap, PD, co-pay, sub-limits) | Partial | Manual | Partial | Manual | Partial | **P2** |
| Settlement letter reconciliation | No | Manual | Partial | Manual | Partial | **P2** |
| "Who to Ask" classification | No | Partial | No | Partial | No | **P1 (hospital side); P2 (insurer side)** |
| Use during the stay | No | No | Partial | No | Partial | **P1: interim-bill checks, one session each** |
| Reimbursement document readiness | No | No | Yes | Partial | Partial | **P2** |
| Evidence-backed templates with citations | Partial | Yes | No | Partial | No | **P1** |
| Escalation ladder | No | Yes | No | Partial | No | **P1: ladder in the report (no tracking, D7)** |
| Human expert on demand | No | Yes | Yes | Yes (own customers) | Yes (own customers) | **P1.5 (paid)** |
| Open to any policy / any hospital | Yes | Yes (selective) | No | No | No | **Yes** |
| Price benchmarks | No | No | No | No | No | **Phased (data moat)** |
| Outcome tracking / learning loop | No | Internal | Internal | Internal | Internal | **P1: in-session only (D12); full loop once outcomes can be tracked** |

### 5.4 Differentiation and positioning

| Differentiator | What it means | Available |
|---|---|---|
| **Depth** | Every line extracted and normalised, not just category totals or generic flags | P1 |
| **Objective findings** | Arithmetic, duplicates, MRP and NPPA cap breaches, room days vs length of stay: findings a billing desk cannot easily dismiss | P1 |
| **Evidence and citations** | Every finding links to the bill line and the record (discharge summary, implant sticker, package terms) with a confidence level; policy clauses and regulatory references are added in P2 | P1; P2 for policy |
| **Follow-through** | A Billing Desk Summary designed for billing staff (§9.3), templates and a hospital escalation ladder | P1 |
| **Point-of-care speed** | Useful *before* money changes hands, including on interim bills | P1 |
| **"Who to Ask"** | Tells the user whether to challenge the hospital, the insurer, or accept the charge | P1 hospital side; P2 insurer side |
| **Accessibility** | No sign-up, WhatsApp-friendly upload, a downloadable report families can forward; regional languages later | P1; languages P3 |
| **Patient-side neutrality** | Not paid by hospitals or insurers | All phases |
| **Hybrid AI + expert** | An affordable default with a human safety net for high-stakes cases | P1.5 |
| **Three-way reconciliation** (Bill ↔ Policy ↔ Settlement) | Explains every rupee of patient payable | P2 |
| **Outcome data loop → benchmark moat** | Anonymised, consented data on prices, deductions and dispute outcomes by hospital, city, procedure and insurer. It becomes defensible over time and supports future B2B products. | Phased (limited by 3-month retention, G-05) |

**Phase 1 focus.** Until the Insurance Check ships, the MVP competes mainly with free bill checkers. It must be clearly better on the first five rows, with Expert Review from P1.5 as backup.

**Positioning.** "**The patient's billing expert in your pocket**": not a legal service, not an insurance seller, not a hospital partner.
- Lead with **clarity and evidence** rather than outrage ("we explain your bill and tell you what to ask"), so front-line staff at hospitals and insurers take users' questions seriously.
- Avoid "catch the hospital cheating" marketing, which invites backlash and legal risk (§6.3, RK3).

---

## 6. Strategic Considerations

These are assumptions in the original brief, and in the reference product, that were challenged before investment. Each item ends with its current status. Where a founder decision has settled the matter, the decision is cited.

### 6.1 "B2C first" collides with low usage frequency
Most households are hospitalised infrequently, often once every few years. A pure B2C funnel therefore has **very low natural repeat usage**, so paid acquisition is expensive relative to lifetime value.

**Recommendation.** Keep B2C as the product's core and brand, and plan for **event-triggered acquisition**:
- search intent at the moment of pain;
- WhatsApp sharing within family groups;
- word of mouth in hospital waiting rooms;
- consumer-rights content.

Also schedule B2B2C distribution (employers, brokers, claim consultants) for Phase 3, straight after the Insurance Check, and build a non-hospitalisation hook such as the **Policy Health Check** to give people a reason to return **[HYPOTHESIS]**.

**Status:** B2B in P3 (D1). Policy Health Check in P2 (J5); it needs a return mechanism (G-07). Risk RK1.

### 6.2 "Audit at discharge" is often too late
For cashless claims, the insurer must give final authorisation within 3 hours of the hospital's discharge request (IRDAI Master Circular, May 2024) **[VALIDATE]**. In practice the family sees the final bill and the deduction minutes before they are asked to pay. Leverage is highest *before* the final bill is generated.

**Recommendation.** A "Stay Mode": interim bills uploaded during the stay, with early alerts (consumables building up, duplicate investigations, package scope creep).

**Status:** Cumulative Stay Mode is not in the MVP (D7). Each interim bill can still be checked in its own session (§11.2). Constraint C5.

### 6.3 "Overcharging" has no single legal benchmark for private patients
- The Clinical Establishments (Central Government) Rules, 2012 require hospitals to charge within a government-notified range of rates. However, the Supreme Court noted in 2024 that the ranges had not been notified, and in 2025 that most States/UTs had not submitted proposals **[VALIDATE]**.
- CGHS and PM-JAY rates apply to their own beneficiaries. They are **not** legal caps for a private, self-paying or privately insured patient.
- Hard caps exist only in narrow areas: NPPA ceiling prices for scheduled drugs and certain devices (e.g., coronary stents, knee implants), and MRP on packaged goods.

**Response.** The product distinguishes three kinds of finding (§13.1):
- **objective errors**: arithmetic, duplicates, MRP or ceiling-price breaches, items billed but not supported by the record;
- **policy misapplication**: deductions not supported by policy wording or IRDAI guidelines (P2);
- **price outliers**: a *question*, not an accusation.

Reports, product language and marketing never call a hospital fraudulent, and never say definitively that a user was "overcharged" unless the finding is objective. This protects users' credibility and protects the company from defamation and hospital backlash (RK3).

### 6.4 "Non-payable" does not mean "recoverable"
Many items an insurer deducts as non-payable are, under the policy, legitimately the patient's to pay. A tool that simply lists "₹X of non-payables found" creates false hope.

**Response.** Every rupee is classified as **Challenge the hospital**, **Challenge the insurer** or **Legitimately payable**. This "Who to Ask" classification is the core differentiator (§13.1.3). The hospital side is in P1; the insurer side arrives in P2.

### 6.5 Getting an itemised bill is itself a hurdle
Many hospitals give patients a summary bill. The detailed itemised bill, pharmacy breakup and implant invoices or stickers are often shared only with the TPA. The audit can only be as good as its inputs.

**Response.** The product (a) detects summary bills (DOC-01, DU-08), (b) tells the user exactly which documents to request and how (DU-08, RE-06a), and (c) produces a partial audit with clearly stated limits (DP-21). Risk RK8; assumption A4.

### 6.6 Medical-necessity questions require clinicians, not AI
Questions such as "was this ICU stay, investigation or length of stay necessary?" are clinical judgements that carry liability. The AI raises only *consistency* observations (e.g., "ICU charged for 5 days; discharge summary mentions ICU for 2 days") through the CLN checks. Clinical-necessity opinions, if offered at all, come from a credentialed doctor on the expert panel (P1.5, EX-06) and are clearly labelled.

### 6.7 The insurer is both adversary and potential customer
Our reports may challenge insurer deductions, yet insurers and TPAs are natural future B2B buyers because they want to catch hospital over-billing. Selling the same engine to both sides creates a perceived conflict of interest.

**Recommendation.** Adopt the brand stance **"patient-side, always"** now:
- B2B buyers (P3) are patient-aligned: employers, brokers, consultants, patient-advocacy groups.
- Any insurer or TPA partnership is data-only (anonymised benchmarks) or deferred, and is disclosed.

**Status:** open question Q-B3. Risk RK14.

### 6.8 Success fees and legal services carry regulatory traps
A "% of money recovered" model is attractive, but:
- Bar Council rules prohibit advocates in India from charging contingency fees;
- entities that solicit or settle claims for a fee may draw IRDAI scrutiny **[VALIDATE]**;
- the Advocates Act restricts legal advice by non-advocates.

**Response.** Phase 1 is free (D25). Phase 1.5 earns revenue from fixed-fee Expert Review. The product provides self-help templates rather than representation. A legal opinion is required before any success-fee or representation service (§19.2, Q-B4).

### 6.9 A bill-only MVP: trade-offs
Founder decision D5 narrows the MVP to the Hospital Bill Check.

**What it gains**
- A simpler, faster build: no policy-wording library, no settlement-letter parsing, no insurer-specific rules.
- Value for every patient, insured or not.
- Lower regulatory exposure at launch: no insurer-facing activity, so most IRDAI questions arise only in P2.
- A clear first message: *"Check if your hospital bill is fair."*

**What it gives up**
1. *Differentiation against free tools.* The MVP must win on depth and trust (§5.4). Risk RK18.
2. *Less value for cashless insured patients.* Much of their payable comes from insurer deductions, which the MVP does not explain. They can still use the bill check, but self-pay and reimbursement patients are targeted first (D20). Risk RK19.
3. *Possibly lower willingness to pay* for a bill-only report. Phase 1 is therefore free (D25), and Expert Review is the first paid product (P1.5).
4. *The main long-term differentiator is deferred* (three-way reconciliation). Plan P2 as a fast follow:
   - complete the domain validation of insurance rules (Q-D1–Q-D4, Q-D6) during the MVP;
   - build the P2 labelled dataset through a separate, consented research cohort rather than routine MVP uploads (data minimisation). Risk RK20.

**Recommendation (accepted with D5).**
- (a) Make self-pay patients a primary segment.
- (b) Position the MVP as *the most thorough hospital bill check in India*, not as a free utility.
- (c) Set a target date for the Insurance Check before launch. This is not yet done (Q-B10, G-08).

---

# Part II — Product Definition

## 7. Product Vision and Principles

### 7.1 Vision
> **"No Indian family should pay a hospital bill they don't understand."**
>
> Within five years, become the trusted, patient-side standard for understanding and challenging hospital bills and insurance settlements in India. Every patient should be able to see, in minutes, what they were charged, what their policy covers and what they should pay, and know exactly what to do if something looks wrong.

### 7.2 Value proposition

**Phase 1 — Hospital Bill Check**

**For** patients, insured or self-paying, and the families who manage their care,
**who** are handed hospital bills they cannot read or verify,
**our platform** is a patient-side hospital bill auditor
**that** reads every line of the bill and checks it against its own arithmetic, the discharge summary, printed MRPs and government price caps. It explains what is wrong, what is worth asking about and what is fine.
**Unlike** free bill checkers, which give generic flags without evidence or follow-through,
**we** deliver an evidence-backed report in minutes and a summary to hand to the billing desk, with an expert available from Phase 1.5.

**Phase 2 — adds the Insurance Check**

The same bill is reconciled with the user's policy and the insurer's settlement. Every rupee the patient pays is explained and classified: challenge the hospital, challenge the insurer, or legitimately payable.

### 7.3 Key benefits

| Benefit | What it means for the user | How we deliver it | Phase |
|---|---|---|---|
| Detect potential overcharging | Spot duplicates, arithmetic errors, unbundled items, quantity anomalies, MRP and price-cap breaches | Hospital-bill checks (§13) | P1 |
| Increase billing transparency | Understand every line in plain language | Extraction, categorisation, glossary | P1 |
| Challenge questionable charges | Know what to ask the hospital, what evidence to attach and how to escalate | Billing Desk Summary, templates, hospital escalation ladder | P1 |
| Reduce stress and time | One place, step by step, no jargon | Guided journey; expert help from P1.5 | P1 |
| Avoid surprises at discharge | Spot issues on interim bills during the stay | Interim-bill checks, one session each | P1 |
| Improve claim understanding | Know exactly how the insurer arrived at the payout | Policy + settlement reconciliation | P2 |
| Reduce insurance losses | Avoid wrong deductions and reimbursement rejections | Insurer-side Who to Ask; Reimbursement Readiness | P2 |
| Prevent future losses | Learn policy limits before the next admission | Policy Health Check | P2 |

### 7.4 Product principles
1. **Evidence over opinion.** Every finding cites a bill line, record, policy clause or regulatory reference.
2. **Question, don't accuse.** Use neutral, respectful language that users can safely repeat to hospital staff.
3. **Honest about uncertainty.** Show confidence levels, and say "we can't tell" when inputs are insufficient.
4. **Speed at the moment of need.** The first useful insight arrives in minutes, not hours.
5. **Patient-side, always.** No hidden incentives from hospitals or insurers.
6. **Human in the loop for high stakes.** High-value or clinical findings are labelled "Complex — consider professional advice" in Phase 1, and receive Expert Review from P1.5.
7. **Privacy by default.** Collect the minimum data, ask for explicit consent, let users discard data in the session, and store nothing identifying.

### 7.5 What we will not promise
- We will not guarantee refunds or claim approvals.
- We will not declare that a hospital or insurer acted fraudulently.
- We will not give medical advice or clinical opinions through AI.
- We will not give legal advice or represent users (representation is not planned before P4, and only subject to a legal opinion).

---

## 8. Goals and Success Metrics

### 8.1 User goals

| ID | Goal | Phase |
|---|---|---|
| UG1 | Understand every line of a hospital bill in plain language. | P1 |
| UG2 | Know, before paying, which charges are questionable and whom to ask. | P1 |
| UG3 | Understand exactly how the insurer computed the payout and which deductions may be wrong. | P2 |
| UG4 | File reimbursement claims that are complete and on time. | P2 |
| UG5 | Raise disputes with the right party, with the right evidence, within the right deadlines. | P1 (hospital); P2 (insurer) |

### 8.2 Business goals (first 12–18 months)

| ID | Goal |
|---|---|
| BG1 | Prove B2C product-market fit: users find the audit accurate, useful and worth paying for (willingness to pay tested from P1.5). |
| BG2 | Show that audits find real, actionable issues. Measured by the North Star (§8.3) and the value of objective errors flagged (D13). Verified savings are measured once outcomes can be tracked. |
| BG3 | Build a consented, anonymised dataset of bills, deductions and outcomes that improves audit quality and creates a benchmark moat (constrained by D24; see G-05). |
| BG4 | Establish trust and regulatory safety: zero material data incidents, no regulatory action, low complaint rate. |
| BG5 | Validate at least one B2B2C channel (employer, broker or claim consultant) as a pilot within 18 months (entry to P3). |

### 8.3 North Star metric (Phase 1)
**Actionable Audit Rate**: the share of completed audits with at least one "Likely error" or "Worth asking" finding (D13).

Supporting measures:
- **Objective value flagged**: the median ₹ value of "Likely error" findings per audit. Always described as money *flagged*, never money *saved*.
- **Action intent**: the share of sessions in which the user marked findings "Will ask" or downloaded a template.

**Verified Patient Value** (rupees actually avoided or recovered) becomes the North Star once outcomes can be tracked (P1.5 or later).

### 8.4 Metric tree (Phase 1)

> Targets are **initial hypotheses** for the first months after launch and are recalibrated after the first 500 completed audits. Phase 1 is anonymous, single-session, free and has no experts (D6, D7, D11, D25). Only metrics that can be observed within a session, or against the Labelled Test Set, are used.

| Area | Metric | Definition | Initial target |
|---|---|---|---|
| Usage | Audits started per week | Sessions with ≥ 1 document uploaded | Growth trend |
| Activation | Audit completion rate | Sessions reaching the full report / sessions started | ≥ 60% |
| Activation | Report download rate | Sessions that download the report / sessions reaching it | ≥ 70% **[HYPOTHESIS]** |
| Speed | Time to first insight | Upload complete → summary shown | ≤ 5 minutes (P90) for typed/PDF bills |
| Speed | Time to full report | Upload complete → full report | ≤ 15 minutes (P90) |
| North Star | Actionable Audit Rate | Audits with ≥ 1 "Likely error" or "Worth asking" finding / completed audits | Baseline in the first month |
| Value | Objective value flagged | Median ₹ of "Likely error" findings per audit | Baseline in the first month |
| Engagement | Action intent | Sessions with findings marked "Will ask" or a template downloaded / sessions with findings | ≥ 40% |
| Quality | Line-item extraction accuracy | Against the Labelled Test Set (D11) | ≥ 95% on digital PDFs; ≥ 90% on good-quality photos |
| Quality | Finding precision ("Likely error") | Against the Labelled Test Set; a check goes live only when it meets the target | ≥ 90% |
| Quality | Finding precision ("Worth asking") | Against the Labelled Test Set | ≥ 75% |
| Quality | Finding usefulness | Share of findings rated "helpful" by users in the session | Track |
| Coverage | Unreadable-line rate | Share of bill lines not read, incl. Hindi or regional script (D9) | Track by State; trigger for the language roadmap |
| Satisfaction | Report usefulness (CSAT) | "This report helped me" ≥ 4/5 | ≥ 80% |
| Satisfaction | NPS | At the end of the session | ≥ 40 |
| Growth | Referral | Self-reported "how did you hear about us" | ≥ 20% |
| Trust | Data incidents | Personal-data breaches | 0 |

**Not measured in Phase 1:** money actually saved; success rate of actions; expert metrics (P1.5); conversion and revenue (Phase 1 is free). See G-02.

### 8.5 Guardrail metrics
- Rate of "Likely error" findings later shown to be wrong, from user ratings or Labelled Test Set retests. Target: near zero, with each instance root-caused.
- Share of findings phrased as accusations. Target: 0, enforced by content QA.
- Stored records older than 3 months. Target: 0 (D24).

---

## 9. Users

The personas reflect the Phase 1 scope (D26): a hospital bill check that is anonymous, single-session, English-only, expert-free and available all-India. Insurance-centred personas are kept for later phases. The hospital billing desk is described as a key **stakeholder**, because every Phase 1 outcome depends on it.

### 9.1 Primary personas (Phase 1)

#### Persona 1 — Self-paying patient: "Ramesh, 47, shopkeeper, Jaipur"
- **Context:** No health insurance. His father is admitted for a knee replacement at a private hospital, paid from savings and a loan from relatives.
- **Goals:** Pay a fair price; make sure the implant and the package are billed as agreed.
- **Frustrations:** The final bill is ₹1.2 lakh above the verbal estimate; there are long lists of "consumables" and "miscellaneous"; no insurer is checking this bill, and nobody else will.
- **Key needs:** A quick, trustworthy check against the package quote, implant price caps and printed MRPs; polite, specific questions for the billing desk.
- **Expected outcomes:** Errors corrected or extras justified before he pays; confidence that he was not overcharged.

#### Persona 2 — Family member at discharge: "Priya, 41, homemaker, Hyderabad"
- **Context:** Her husband was admitted as an emergency for a cardiac procedure. She handles everything while caring for two children, on a mid-range Android phone.
- **Goals:** Get through discharge without being overcharged; avoid confrontation; protect the family's savings.
- **Frustrations:** Doesn't understand medical terms; is pressured to pay quickly; fears that questioning the bill will affect her husband's care.
- **Key needs:** Step-by-step guidance in simple English; polite wording she can repeat; one page she can show the billing desk; no sign-up while standing at the counter.
- **Expected outcomes:** Knows exactly 2–3 things to ask; the bill is corrected or explained before she pays.

#### Persona 3 — Remote family manager: "Meera, 38, software engineer, Singapore (NRI)"
- **Context:** Her parents live in Lucknow, and her father is in hospital. She manages everything by phone and WhatsApp across time zones, often from her laptop.
- **Goals:** Make sure her parents are not overcharged, without travelling; act quickly.
- **Frustrations:** Relatives send blurry photos of bills; she can't call during Indian business hours; her parents can't argue with the billing desk.
- **Key needs:** Run the check herself on desktop from forwarded photos; a crisp PDF to send to her parents and the hospital; clear notes on which pages are missing or unreadable (including lines in Hindi).
- **Expected outcomes:** Confidence and control from abroad.
- **Phase notes:** No shared case access (D7); expert help from P1.5.

#### Persona 4 — Patient paying upfront: "Rohit, 34, IT professional, Pune"
- **Context:** Planned appendectomy at a hospital outside his insurer's network. He pays ₹1.6 lakh upfront and will claim reimbursement later.
- **Goals:** Pay only what is fair; have a clean, correct bill for his later claim.
- **Frustrations:** A 40-page bill with internal codes; ₹22,000 under "consumables" with no detail; no time to read it before paying.
- **Key needs:** Line-by-line explanation; objective errors flagged before he pays; requests for the itemised bill and implant/package documents.
- **Expected outcomes:** Errors removed before payment; a bill he understands.
- **Phase notes:** Reimbursement help (deadlines, documents, deductions) arrives with the Insurance Check (P2).

#### Persona 5 — Bereaved family: "Anil, 44, bank officer, Kolkata"
- **Context:** His mother died in the ICU after 12 days. The final bill is ₹6.8 lakh, and the family is grieving and arranging the funeral.
- **Goals:** Settle the bill fairly without being exploited; get closure.
- **Frustrations:** Pressure to clear dues quickly; ICU, ventilator and drug charges he cannot judge; guilt about questioning the bill at all.
- **Key needs:** A gentle, respectful tone; objective errors first, with no aggressive language; reassurance that issues can be raised after payment; simple consent by declaration for a deceased patient's data (ON-06).
- **Expected outcomes:** Obvious errors removed; a feeling of having been treated fairly.
- **Design implications:** Tone and wording suited to bereavement; no upselling; a clear way to stop at any point.

### 9.2 Supported persona (Phase 1)

#### Persona 6 — Senior citizen: "Kamala, 68, retired teacher, Chennai"
- **Context:** Several admissions a year; limited comfort with smartphones; prefers Tamil; her son usually helps.
- **Goals:** Avoid being overcharged across repeated admissions; avoid repeated trips for paperwork.
- **Frustrations:** Long bills in small print that are hard to read on a phone.
- **Key needs:** Very simple English and large text; her son can run the check for her on desktop and walk her through the PDF.
- **Expected outcomes:** Confidence that each bill is correct.
- **Phase notes:** Tamil and other regional languages arrive in P3; co-pay and room-rent cap help in P2.

### 9.3 Key stakeholder — Hospital billing desk staff: "Sunita, 32, billing executive, 300-bed private hospital, Pune"
Not a user, but the person who decides whether a Phase 1 finding becomes a corrected bill.
- **Context:** Handles 40–60 discharges a day under pressure from patients, doctors and finance. She can correct obvious errors herself, but must escalate pricing or package questions to the billing manager or medical superintendent.
- **What makes her act:** Specific line and page references; the bill number and date; a short list with objective errors first; neutral, courteous wording; questions she can answer from the system.
- **What makes her dismiss a query:** Accusations ("you're cheating us"), vague claims, internet printouts, demands to waive legitimate charges, long documents.
- **What she can fix on the spot:** Arithmetic errors, duplicates, room days, MRP excess, uncredited advances.
- **Product implications:**
  - The Billing Desk Summary (RP-02) must be one page and list objective errors first.
  - It must quote the bill line and page for each item.
  - It must separate "please correct" items from "please explain" questions.
  - Templates must avoid accusatory language (TS-02).

### 9.4 Later-phase personas

#### Persona 7 — Retail policyholder: "Suresh, 52, small business owner, Indore" — P2
- **Context:** Family floater policy. His first major claim (reimbursement) was partly settled, with deductions labelled "non-medical" and "proportionate".
- **Key needs:** Line-by-line reconciliation of the settlement against the policy; a clear view of what can be contested; a complaint draft for the insurer and the Ombudsman.
- **Expected outcomes:** Recovers wrongly deducted amounts, or understands why they are valid.

#### Persona 8 — Corporate employee under group insurance: "Ananya, 29, analyst, Bengaluru" — P2 (policy checks), P3 (employer channel)
- **Context:** Group mediclaim that also covers her parents. Room rent is capped at a percentage of sum insured, and there is a maternity sub-limit. She discovers the caps only at discharge.
- **Key needs:** Explanation of group policy terms; checks on room and maternity sub-limits; evidence to escalate to HR and the broker.
- **Expected outcomes:** No avoidable deductions.

#### Persona 9 — Medical claim consultant: "Farhan, 45, independent consultant, Mumbai" — P3
- **Context:** Handles 20–40 cases a month manually.
- **Key needs:** Bulk upload, structured extraction, a check catalogue with citations, branded reports, case tracking.
- **Expected outcomes:** 3–5× throughput; professional reports for clients.

### 9.5 Persona priority

| Priority | Persona | Rationale |
|---|---|---|
| Primary (P1) | Ramesh (self-pay), Priya (family at discharge), Meera (remote NRI), Rohit (paying upfront), Anil (bereaved family) | They pay the hospital directly and get the most value from an anonymous bill check |
| Supported (P1) | Kamala (senior citizen) | Usually through a family member; English only in P1 |
| Key stakeholder (P1) | Sunita (billing desk staff) | Every P1 outcome depends on her accepting the Billing Desk Summary |
| P2 | Suresh (retail policyholder), Ananya (corporate employee) | Their main pain is insurance deductions and policy limits |
| P3 | Farhan (claim consultant) | Professional tooling; B2B |

---

## 10. Scope and Phasing

### 10.1 Phase overview

| Phase | Focus | Exit criteria |
|---|---|---|
| **P0 — Foundations (before the build)** | The product team builds rule library v0 from public sources (D22) and the Labelled Test Set (§12.14): network and synthetic bills first, then a closed pre-launch beta, with two-person labelling of real bills. No legal review (D27). | 300–500 bills in the set, with coverage reported; launch-set checks meet precision targets |
| **P1 — Hospital Bill Check (MVP)** | Must Haves plus selected Should Haves (§15); launch ~3–4 months after the team is in place with the 18-check launch set; Waves 2–3 after launch | North Star and supporting measures baselined (§8); precision targets held; completion and download rates on target; Insurance Check interest sized |
| **P1.5 — Expert Review** | Paid Expert Review by email; payments; doctor experts; legal and tax review of P1 choices | Experts deliver within committed times; users pay for review; legal review complete |
| **P2 — Insurance Check** | Policy Understanding; POL/SET/TAT/RMB checks; J2, J3, J5; insurer templates and escalation; Patient Payable Breakdown; phone numbers; benchmarks v1; deeper bill checks; State-aware hospital escalation | Precision targets met for insurance checks; Verified Patient Value measurable; upgrade rate of P1 users |
| **P3 — Scale and B2B2C** | Pre-admission review, regional languages and voice, WhatsApp-first, claim-consultant workspace, employer pilot, broker white-label, government schemes | A repeatable acquisition channel; one B2B2C pilot live with renewal intent |
| **P4 — Ecosystem** | ABDM/NHCX consented data, compliant legal partner network, outcome-based pricing (if viable), hospital "audit-ready" programme | Data network effects visible; multi-channel revenue |

### 10.2 Phase 1 — Hospital Bill Check (MVP)

**Objective.** Prove that a free, AI-led hospital bill check is **accurate, understood and acted on** by Indian families, starting with self-pay and reimbursement patients.

**Launch shape**
- Available **all-India** from day one (D17).
- **English only** for screens, reports and bill reading (D9).
- **Mobile web and desktop web**; no native app (D18).
- **Free**; no payments (D25).
- **Anonymous, single-session use**: no sign-up and no user profile; the report is downloaded as a PDF (D6, D7).
- Launch ~3–4 months after the team is in place, with the 18-check launch set live (D21).

**In scope**
- Journey J1 (Hospital Bill Check), used at three moments:
  - during the stay (interim bills, each checked separately);
  - at discharge (final bill);
  - after payment.
- In-patient and day-care bills from private and trust hospitals in India.
- All patients (self-pay, reimbursement and cashless), **for hospital-billing checks only**. Cashless patients are served but not targeted (D20).
- Documents:
  - hospital bills (interim/final, summary/itemised);
  - pharmacy bills;
  - consumables and implant invoices and stickers;
  - discharge summary (optional; enables consistency checks);
  - cost estimate or package letter (optional);
  - payment receipts.
- Hospital-bill check categories DOC, CAL, DUP, LOS, RM, PF, RX, CON, IMP, DX, OT, PKG, CLN, ADM, rolled out in waves (§13.8).
- An Audit Report with Bill Breakdown, evidence, confidence levels and a one-page Billing Desk Summary.
- A self-help, hospital-side action toolkit inside the downloaded report:
  - itemised-bill and document requests;
  - query lists;
  - revised-bill and refund requests;
  - hospital escalation ladder;
  - NPPA complaint pointer.
- No experts of any kind (D10, D11). Quality is measured against the Labelled Test Set.
- Storage of de-identified structured bill data only:
  - consent is asked after the report (D19);
  - data is deleted after 3 months (D24);
  - original files are not kept (D8, §12.2).
- In-session feedback on findings.

### 10.3 Phase 1 — Out of scope
"W#" refers to the Won't Have IDs used in prioritisation. Requirement IDs are listed in §12.17 and §12.16.

| W# | Item | Reason | Planned phase |
|---|---|---|---|
| W1 | **All insurance checks**: policy understanding, policy application, settlement reconciliation, reimbursement readiness, insurer templates, insurer escalation and deadlines (DU-01b, ON-10, CM-05, PU-01–PU-06, DP-05, DP-06, DP-18b, AA-07, AA-09, AA-10, AA-15b, AD-04; checks POL, SET, TAT, RMB, LOS-05, RM-03, RX-06, CON-01) | D5 | P2 |
| W2 | Pre-admission estimate review (J4) | Deferred; it reuses the bill check, so it could be pulled forward | P3 |
| W3 | Representation before hospitals, insurers, the Ombudsman or courts; sending communications on users' behalf (AD-06) | Regulatory perimeter (§19) | P4, subject to a legal opinion |
| W4 | Success-fee / contingency pricing | Bar Council and IRDAI constraints; legal opinion pending | P4, if viable |
| W5 | AI clinical opinions (medical necessity, appropriateness of treatment) | Liability | Never by AI; doctor experts from P1.5 |
| W6 | User-visible price benchmarks against peer hospitals | Insufficient data; legal risk | P2 |
| W7 | Public hospital ratings or rankings | Defamation risk; data validity | P4 transparency index only, with robust data and legal review |
| W8 | Integrations with hospitals, insurers, TPAs, NHCX, ABDM | — | P4 |
| W9 | B2B dashboards, consultant workspace, bulk upload (DU-12) | — | P3 |
| W10 | Government schemes (PM-JAY, CGHS, ECHS, State schemes) | Different rules and grievance processes | P3 |
| W11 | Hindi and regional languages: screens, reports and reading bills (DP-08, RP-05) | D9 | P3 |
| W12 | Selling or recommending insurance; lending / bill financing | IRDAI intermediary registration; separate regulated activity; neutrality | Not planned (lending: possible P4 partnership) |
| W13 | OPD, dental, maternity-only and diagnostics-only bills | Lower value per case | P4 |
| W14 | User accounts, login, profiles, saved cases, family sharing (ON-10, CM-04) | D6, D7; the user forwards the PDF instead | — |
| W15 | Notifications and reminders by SMS, email or WhatsApp (NT-01–NT-03, AD-03) | No contact details collected (D6) | — |
| W16 | Stay Mode across sessions (cumulative alerts during the stay) | D7 | Needs a return mechanism (P2+) |
| W17 | Storage of original uploaded files | D8 (only exception: closed beta, D28) | — |
| W18 | Long-term outcome tracking and follow-up surveys in the product (AD-02, CM-06, CM-07, FB-03) | D7, D12 | — |
| W19 | Expert review (user-facing) and internal expert QA | D10, D11 | P1.5 |
| W20 | Payments, promo codes, corporate access codes | D25 | P1.5 |
| W21 | Collecting email addresses or phone numbers | D6 | Email (for Expert Review only) P1.5; phone P2 |
| W22 | Native mobile app | D18 | — |
| — | International / overseas treatment bills | Different regimes | Not planned |
| — | Motor, life and personal-accident claims | Different domain | Not planned |
| — | Fraud determination against hospitals or insurers | Never in product language; internal risk indicators only (§13.5) | Not planned |

### 10.4 Phase 1.5 — Expert Review
- Optional, paid review of a Phase 1 audit by a human billing expert (§12.15).
- The user gives an **email address only**, and only if they want a review. Phone numbers are not collected.
- The expert replies **manually by email with a PDF**, and may correspond with the user manually by email.
- Payments (PY) arrive with Expert Review. The free/paid boundary is still open (G-01).
- Doctor experts handle clinical-consistency questions.
- Legal and tax review of Phase 1 choices is recommended here (§19.6).
- To decide in P1.5: what is kept while a review is open (original files, email address) and when it is deleted (Q-P11).

### 10.5 Phase 2 — Insurance Check
- Upload of insurance documents (DU-01b): policy schedule, e-card, wording or CIS; pre-authorisation and final approval letters; settlement letters; query letters.
- Policy Understanding (PU) and the Policy Key Terms card.
- Insurance checks: POL, SET, TAT and RMB, plus LOS-05, RM-03, RX-06 and CON-01 (§13.7).
- Journeys J2 (Reimbursement Readiness), J3 (Settlement Deduction Review) and J5 (Policy Health Check, a retention and acquisition hook).
- Insurer-side Who-to-Ask labels, the *Policy Impact* finding class and the Patient Payable Breakdown (AA-15b). Together these complete the three-way reconciliation.
- Insurer-side templates and escalation ladder (insurer clarification, GRO complaint, Bima Bharosa, Ombudsman preparation pack) and insurance deadline reminders.
- Multiple policies per case (CM-05; no saved profiles, D6).
- Phone numbers (deferred from P1.5), which enable later channels such as WhatsApp.
- Policy-aware interim-bill alerts. A connected Stay Mode would need a return mechanism (D7, G-07).
- Alongside: deeper bill checks tagged P2 (§13.6), benchmarks v1 (city/hospital-tier price distributions), and State-aware hospital escalation.

### 10.6 Phase 3 — Scale and B2B2C
| Enhancement | Value |
|---|---|
| **Pre-Admission Estimate Review (J4)**: estimate or package quote vs benchmarks and policy; room-choice advice | Prevention; earlier engagement |
| **Regional languages** (full Hindi, Tamil, Telugu, Marathi, Bengali, Kannada) and voice-led flows | Senior citizens; Tier 2/3 reach |
| **WhatsApp-first experience** | A low-friction channel familiar to families |
| **Claim consultant workspace (J6)**: bulk upload, branded reports, case pipeline | B2B revenue; expert supply |
| **Employer benefit (J7)**: access codes, anonymised HR insights on policy design gaps | B2B2C distribution |
| **Broker / aggregator white-label** | Distribution |
| **Government schemes** (CGHS, ECHS, PM-JAY, State schemes) | Large underserved base |

### 10.7 Phase 4 — Ecosystem
| Enhancement | Value |
|---|---|
| **ABDM/NHCX consented data access**: claim status and settlement details, with consent | Less manual upload; richer data |
| **Legal partner network** (compliant structure) for Ombudsman and consumer-commission support | A full resolution journey |
| **Outcome-based pricing** (if legally viable) | Aligned with users |
| **Hospital transparency index** (only with robust data and legal review) | Market-level transparency |
| **Bill financing / EMI partnerships** (regulated partners) | Liquidity at discharge |
| **"Audit-ready billing" certification for hospitals** | A positive-sum relationship with hospitals |
| **OPD, diagnostics, dental and maternity bill audits** | Higher-frequency use cases |

---

## 11. Customer Journeys

### 11.1 Core concepts: Case and Session
- A **Case** is one hospitalisation episode for one patient. It holds the documents, findings and the actions the user marks.
- In Phase 1, each **Session** creates exactly one Case:
  - the session is anonymous (D6);
  - the case cannot be reopened or shared after the session ends (D7);
  - everything the user needs leaves with them in the downloaded Audit Report;
  - only de-identified structured bill data is kept, and only with consent (D8, D19, §12.2).
- The MVP has one journey, J1, used at three moments. J2 and J3 are insurance journeys for P2 and are already specified below.

### 11.2 Journey J1 — Hospital Bill Check (P1)
Any patient; interim bill, final bill, or already paid.

| Stage | User situation | Product behaviour | Moment of value |
|---|---|---|---|
| 1. Trigger | The hospital hands over an interim or final bill; the family is unsure whether it is fair | Fast entry with privacy notice and consent only (no sign-up). The user picks the moment: "In hospital", "At discharge" or "Already paid" | Under 2 minutes to start |
| 2. Capture | Photos or PDFs of the bill; optionally the discharge summary, pharmacy bills, implant invoices/stickers, cost estimate or package letter | Guided capture with a checklist; detects summary vs itemised bills; requests missing pages; all documents are added within the same session | The user knows which documents to ask the hospital for |
| 3. Quick read | Waiting at the billing desk | Instant summary: total, category split, largest items, top flags, balance due after advances | Within minutes |
| 4. Full audit | Wants detail | The live check catalogue runs. The report shows a **Bill Breakdown** (likely errors / worth asking / explained / not assessable) and a per-item explanation. The user confirms any low-confidence lines. | Can ask 2–3 precise, polite questions |
| 5. Act | At the billing desk | **Billing Desk Summary** on screen and in the PDF; query list; document-request templates; the user marks findings to raise | The hospital revises the bill or explains |
| 6. Complex findings | High amount, or unsure | No expert in P1 (D10, D11). Complex or clinical findings are labelled "Complex — consider professional advice". Paid Expert Review by email arrives in P1.5. | Honest about limits |
| 7. Already paid | The bill is paid; the user suspects errors | Templates to request a revised bill, refund or credit note; the hospital escalation ladder is included in the report (no reminders) | A path to recovery after payment |
| 8. Finish | Before leaving | Clear prompt to download the report; optional rating of findings; in-session outcome question where relevant (D12); consent choice for storing de-identified bill data, asked after the report (D19) | The user keeps the report; nothing identifying the user is retained |

**Interim bills.** During admission, the family can check each interim bill as it arrives, each in its own session. Nothing carries over between sessions (D7), so the MVP cannot build cumulative alerts across the stay (W16).

**Insured users.** Insured patients can run J1 on the same bill. The MVP checks only the hospital's billing; it does not predict or explain what the insurer will pay. The report says so clearly and notes that the Insurance Check is coming. Interest is counted anonymously (S8).

### 11.3 Journey J2 — Reimbursement Readiness (P2)
After discharge, before filing.

| Stage | User situation | Product behaviour | Moment of value |
|---|---|---|---|
| 1. Trigger | Paid the full bill; needs to claim from the insurer | Select insurer and policy type; enter admission and discharge dates | Deadline countdown shown |
| 2. Policy capture | Has the policy PDF / e-card | Extract key terms: sum insured, room-rent cap, co-pay, sub-limits, waiting periods, non-payables cover/add-ons, claim intimation and submission deadlines | The user understands what the claim can realistically pay |
| 3. Document checklist | Collecting papers | Insurer-agnostic plus insurer-specific checklist; detects missing, illegible or mismatched documents (name, dates, policy number) | Fewer query cycles and rejections |
| 4. Pre-submission audit | Wants to know the expected payout | Billing checks plus policy checks; **estimated payable range** and likely deductions with reasons | Realistic expectations; a chance to get hospital corrections first |
| 5. Package for submission | Ready to file | An organised, indexed claim pack (documents in the recommended order) plus a cover note | Clean submission |
| 6. Track | Waiting for the insurer | Deadline reminders; query-response helper | Fewer missed deadlines |
| 7. Settlement arrives | The insurer settles | Moves straight into J3 | — |

### 11.4 Journey J3 — Settlement Deduction Review (P2)
After cashless final approval or reimbursement settlement.

| Stage | User situation | Product behaviour | Moment of value |
|---|---|---|---|
| 1. Trigger | Received a settlement letter or saw deductions | Upload the settlement letter (plus bill and policy if not already in the case) | — |
| 2. Reconcile | Wants to know why | Map each deduction to bill lines and policy clauses; recompute room-rent proportionate deduction, co-pay and sub-limit application; flag deductions without stated reasons | Line-by-line understanding |
| 3. Classify | Wants to know what to contest | Each deducted rupee classified: **Likely contestable with insurer / Ask hospital / Legitimately payable / Needs expert** | Focus on what is worth fighting |
| 4. Decide | Effort vs reward | Estimated contestable amount; recommended route; deadlines | Informed decision |
| 5. Act | Raising a dispute | Templates: insurer GRO complaint, Bima Bharosa complaint text, Ombudsman preparation guide; evidence list | Evidence-based complaint |
| 6. Expert (optional) | High value or complex | An expert reviews and personalises the complaint | Higher-quality submission |
| 7. Track and outcome | Waiting | Escalation ladder with dates; outcome capture | Feeds the benchmark of what works |

### 11.5 Future journeys
| Journey | Phase | Summary |
|---|---|---|
| J1 + Expert Review | P1.5 | Optional paid Expert Review on J1 (§10.4) |
| J1 + insurance layer | P2 | Patient Payable Breakdown; policy-aware alerts if a return mechanism is introduced |
| J5 — Policy Health Check | P2 | Explains an existing policy's limits before any hospitalisation |
| J4 — Pre-Admission Estimate Review | P3 | Compares an estimate or package quote with benchmarks and, with P2, policy limits |
| J6 — Professional Workspace | P3 | For claim consultants |
| J7 — Employer Benefit | P3 | Employees access audits through HR; anonymised insights go to HR |

---

# Part III — Requirements

## 12. Functional Requirements

**Priority key**

| Code | Meaning |
|---|---|
| M | Must (MVP) |
| S | Should (MVP if capacity allows) |
| C | Could (MVP stretch) |
| W | Won't (this release) |
| P1.5 | Deferred to Phase 1.5 (Expert Review) |
| P2 | Deferred to Phase 2 (Insurance Check) |

Requirement IDs are stable for traceability into engineering backlogs. §12.1–12.14 hold Phase 1 requirements. P1.5, P2 and W requirements are grouped in §12.15–12.17 with their original IDs.

### 12.1 Onboarding, identity and consent (ON)

| ID | Requirement | Priority |
|---|---|---|
| ON-01 | Users can use the product without registering, giving a mobile number or email, or verifying their identity (D6). | M |
| ON-02 | Users go straight from the privacy notice and consent to uploading; there is no sign-up step. | M |
| ON-03 | Before any upload, the user sees a clear, layered privacy notice in plain language (what is collected, why, retention, sharing, rights). They give explicit, granular consent for: (a) processing for the audit; (b) human expert access (P1.5, only if Expert Review is chosen); (c) storage of de-identified structured bill data for quality improvement and future benchmarks. Consent (c) is optional and separate, and is asked after the report is shown (D19, §12.2). | M |
| ON-04 | The user declares their relationship to the patient (self, spouse, parent, child, other family, authorised representative, professional) and confirms they are authorised to share the patient's data. | M |
| ON-05 | If the patient is a minor, the uploader declares that they are the parent or lawful guardian, and the check proceeds on that declaration (D15). **[VALIDATE — the DPDP Rules require verifiable parental consent, and a declaration may not meet this. Launching without legal review is an accepted risk (D27, RK24).]** | M |
| ON-06 | If the patient is an adult other than the user, the user declares within the session that they have the patient's consent, or authority where the patient is incapacitated or deceased, as per legal advice. | M |
| ON-07 | Users can withdraw consent and discard all their data at any point during the session. After the session ends, stored data is de-identified and not linked to the user, so it cannot be traced back for individual deletion. **[VALIDATE with counsel]** | M |
| ON-08 | Users see an onboarding explainer of what the platform does and does not do (no guarantee, no legal or medical advice). | M |
| ON-09 | Screens, reports and bill reading are in English only for the MVP (D9). Hindi and other languages arrive in P3. | M |

### 12.2 Session and data storage (SD)

| ID | Requirement | Priority |
|---|---|---|
| SD-01 | The product works in a single session: the user uploads, reviews and downloads the report in one sitting, with no account or login (D6, D7). | M |
| SD-02 | No user profile information is collected or stored: no name, mobile number, email, login or device-linked profile of the user. | M |
| SD-03 | Uploaded files are processed only to extract their content. Original files are not stored; they are deleted once extraction is complete, and in any case when the session ends (D8). The only exception is the closed pre-launch beta, where testers explicitly agree that their bills are kept for testing (D28, §12.14). | M |
| SD-04 | Extracted content is stored as structured data only if the user consents, and consent is asked after the report is shown (D19). Stored data covers line items, categories, quantities, rates, amounts, dates, hospital, city, findings and the user's ratings. | M |
| SD-05 | Before storage, remove the patient's name, UHID/IP number, phone, address and policy number. Keep the age band (e.g., 60–69), gender, hospital, city, admission month, length of stay and all line items (D14). Counsel to confirm whether this counts as de-identified under DPDP. | M |
| SD-06 | If the user does not consent to storage, nothing from the session is retained after it ends. | M |
| SD-07 | Before the session ends, the user is clearly prompted to download the report and told that it cannot be retrieved later. | M |
| SD-08 | Sessions expire after a defined period of inactivity, and in-progress data is then discarded. (Period to be set — G-10.) | M |
| SD-09 | Abuse controls that need no identity (e.g., limits on uploads per device or network) protect the free tier. (See G-09.) | S |
| SD-10 | Stored structured data is deleted automatically **3 months** after the session (D24). | M |

### 12.3 Case management (CM)

| ID | Requirement | Priority |
|---|---|---|
| CM-01 | Each session creates one Case representing one hospitalisation. The case records hospital name and city, admission and discharge dates, treatment type (planned/emergency), payment mode (cashless/reimbursement/self-pay) and insurer/TPA (if any). The patient is not named in the case. | M |
| CM-02 | The system infers case details from the uploaded documents and asks the user to confirm them, rather than requiring manual entry. | M |
| CM-03 | The session shows progress (upload → review → report) and the next recommended step. A case cannot be reopened after the session ends (D7). | M |

### 12.4 Document upload (DU)

| ID | Requirement | Priority |
|---|---|---|
| DU-01 | Users can upload hospital documents: bills (final, interim, summary, itemised), pharmacy bills, consumables and implant invoices and stickers, cost estimate or package letter (optional), discharge summary (optional; enables consistency checks), investigation reports (optional) and payment receipts. | M |
| DU-02 | Supported inputs: PDFs (digital and scanned), phone photos (single and multi-page) and screenshots, with multiple files per document. | M |
| DU-03 | Users can upload from the phone camera, with capture guidance (flatness, glare, cut-offs), and from files. | M |
| DU-04 | Users can share documents into the platform from messaging apps (e.g., files forwarded on WhatsApp). | S |
| DU-05 | The system auto-classifies each uploaded file by document type and lets the user correct it. | M |
| DU-06 | The system detects page order, duplicates and missing pages (e.g., "Page 3 of 7 missing", page totals not matching the grand total) and prompts the user. | M |
| DU-07 | The system checks image legibility and requests a re-upload of unreadable pages, identifying which page. | M |
| DU-08 | The system detects when the bill is a **summary** bill and explains how to request the **itemised** bill, pharmacy breakup and implant invoices, with a request template. | M |
| DU-09 | Password-protected PDFs (common for insurer documents) can be unlocked with a password the user provides, which is not stored. | S |
| DU-10 | The system shows a per-case document checklist (what is present, missing or optional) tailored to the journey. | M |
| DU-11 | Users can add documents at any point during the session (e.g., the discharge summary after the bill). Documents cannot be added after the session ends. | M |

### 12.5 Document processing (DP)

#### 12.5.1 Data extraction
| ID | Requirement | Priority |
|---|---|---|
| DP-01 | Extract header data: patient name, age/gender, UHID/IP number, hospital name/address/registration, GSTIN (if any), admission/discharge date and time, ward/room category, treating doctor(s), bill number/date, payer (cashless/TPA/insurer) and policy/claim number. | M |
| DP-02 | Extract every line item with: date of service, description, hospital code (if any), quantity, unit, unit rate, gross amount, discount, tax, net amount, and the section/category it appears under on the bill. | M |
| DP-03 | Extract bill-level totals: section subtotals, gross total, discounts, taxes, advances/deposits, insurer-approved amount, patient payable and amount paid. | M |
| DP-04 | Extract from the discharge summary: diagnosis, procedure(s) performed, procedure dates, ICU/ward stay periods, implants used, condition at discharge and discharge medicines. | M |
| DP-07 | Handle common characteristics of Indian bills: mixed fonts, tables spanning pages, merged cells, handwritten notes, stamps, rupee formats (lakh/crore, Indian digit grouping) and abbreviations. | M |
| DP-08 | Lines partly in Hindi or regional scripts are listed as "could not be read" and excluded from checks. Reading them is not supported in the MVP (D9; P3). | M (list unread lines); W (read them) |
| DP-09 | Every extracted field carries a confidence indicator, and low-confidence fields are highlighted for the user to confirm. | M |
| DP-10 | Users can view and correct extracted line items. Corrections are recorded and re-run the audit. | M |

#### 12.5.2 Line-item identification and normalisation
| ID | Requirement | Priority |
|---|---|---|
| DP-11 | Normalise line-item descriptions to a standard catalogue (e.g., "Inj. Pantop 40" → pantoprazole 40mg injection; "Glov Sterile 7.5" → sterile surgical gloves) while keeping the original text. | M |
| DP-12 | Identify drugs by generic name, strength, dosage form and, where possible, brand. | M |
| DP-13 | Identify consumables and disposables and map them to the IRDAI lists where applicable (see G-22). | M |
| DP-14 | Identify implants and devices and capture make/model/batch where present. | S |
| DP-15 | Identify professional fees by type (surgeon, assistant surgeon, anaesthetist, consultant visits, specialist referrals) and by doctor. | M |
| DP-16 | Identify packages and their stated inclusions and exclusions, where shown on the bill or provided by the user. | M |

#### 12.5.3 Categorisation
| ID | Requirement | Priority |
|---|---|---|
| DP-17 | Assign each line to a standard category: Room & Boarding; ICU/HDU; Nursing; Professional Fees; OT & Procedure; Anaesthesia; Pharmacy (drugs); Medical Consumables; Implants & Devices; Laboratory; Radiology & Imaging; Other Diagnostics; Blood & Blood Products; Physiotherapy & Allied; Diet & Nutrition; Package; Administrative & Registration; Non-medical/Personal; Taxes; Discounts/Adjustments; Other. | M |
| DP-18 | Tag each line with unbundling attributes: whether it is commonly considered part of room, procedure or treatment charges (IRDAI Lists II–IV used as an industry reference, D16). | M |

#### 12.5.4 Validation
| ID | Requirement | Priority |
|---|---|---|
| DP-19 | Validate document consistency: patient name, UHID, dates and hospital match across bill pages, pharmacy bills and the discharge summary, and mismatches are flagged. P2 extends this to policy and settlement documents, where mismatches cause claim queries or rejection. | M |
| DP-20 | Validate internal arithmetic (qty × rate, subtotals, totals, taxes, discounts) before the audit, separating extraction errors from billing errors. | M |
| DP-21 | Validate completeness: the audit states which checks could not run because of missing documents or data. | M |
| DP-22 | When extraction confidence is low, the user is asked to confirm or correct the uncertain fields during the session. Checks that depend on unconfirmed fields are not shown. Quality is measured against the Labelled Test Set (D11); there is no human review of users' audits in Phase 1. | M |

### 12.6 Audit analysis (AA)
The check catalogue is in §13. These requirements cover the audit engine as a whole.

| ID | Requirement | Priority |
|---|---|---|
| AA-01 | Run all applicable live checks for the case's journey and available documents. | M |
| AA-02 | Identify **duplicate charges**: same item, same date/time, same amount; the same investigation repeated without a matching clinical note; the same professional visit billed twice. | M |
| AA-03 | Identify **unusual charges**: items inconsistent with the diagnosis, procedure or patient profile (e.g., items for a different gender or age, a procedure not in the discharge summary). (See G-04.) | M |
| AA-04 | Identify **excessive consumables**: quantities per day of stay or per procedure far above typical (e.g., gloves, syringes, cannulas, IV sets). (See G-03, G-04.) | M |
| AA-05 | Identify **potential package violations**: items billed separately that are typically included in the stated package; stay beyond package days without explanation; package and itemised billing both present. | M |
| AA-06 | Identify **billing anomalies**: arithmetic errors, dates outside the stay, charges after discharge, different rates for the same item within the bill, round-figure miscellaneous charges, unexplained "others". | M |
| AA-08 | Identify **missing billing details**: no itemisation, no dates, no quantities, no batch/MRP for drugs, no implant invoice/sticker, no doctor names for professional fees. | M |
| AA-11 | Compare prices with available reference points: MRP printed on the item; NPPA ceiling prices; government schedule rates such as CGHS (context only); internal anonymised benchmarks when statistically valid. | S (MRP/NPPA: M) |
| AA-12 | Each finding includes: check ID, category, finding class (§13.1.1), confidence (High/Medium/Low), amount potentially affected (₹), evidence (bill line, page, policy clause, regulatory reference), Who-to-Ask classification and a plain-language explanation. | M |
| AA-13 | Findings are de-duplicated and grouped (e.g., 12 glove lines become one finding with 12 evidence lines). | M |
| AA-14 | The audit never counts the same rupee twice across findings in totals; it shows a reconciled "potentially affected" amount. | M |
| AA-15 | The audit produces a **Bill Breakdown** of the bill total: (a) likely errors to correct; (b) charges worth questioning; (c) explained, with no billing concern; (d) could not be assessed. | M |
| AA-16 | Findings above a configurable value threshold (Q-P2), or in clinically sensitive categories, are marked "Complex — consider professional advice" in Phase 1, and "Recommended for expert review" from P1.5. | M |
| AA-17 | The rule library is versioned, and each report records the rule and reference-data versions used. | M |

### 12.7 Recommendations and templates (RE)

| ID | Requirement | Priority |
|---|---|---|
| RE-01 | For each finding, provide an **Explanation**: what the item is, in plain language. | M |
| RE-02 | For each finding, provide an **Observation**: what we noticed, with evidence. | M |
| RE-03 | For each finding, state the **Potential Concern**: why it might be wrong, and how sure we are. | M |
| RE-04 | For each finding, suggest **Follow-up Actions**: what to ask, whom to ask, what evidence to attach, and by when. Whom to ask in P1: hospital billing desk, patient relations, medical superintendent. Added in P2: TPA desk, insurer, HR/broker. | M |
| RE-05 | Prioritise actions by expected value × likelihood of success × effort, and present a short "Top 3 things to do now" list. | M |
| RE-06 | Generate ready-to-use communications that the user can edit. **Hospital side (P1):** (a) itemised bill request; (b) billing query list; (c) request for implant invoice/sticker and package inclusion list; (d) request for a revised bill, written explanation, or refund/credit note; (e) hospital grievance escalation letter; (f) NPPA complaint pointer for MRP or ceiling-price breaches. **Insurer side (P2):** insurer/TPA clarification request; insurer GRO complaint; Bima Bharosa complaint text; Insurance Ombudsman preparation pack. | M (a–d); S (e–f); P2 (insurer side) |
| RE-07 | Communications use a respectful, factual tone and cite evidence and references. They never allege fraud. | M |
| RE-08 | Show the **escalation ladder** for the case, with relevant deadlines, in the report (no reminders, AD-03; reference data per G-11). P1 hospital-side ladder: billing desk → patient relations → medical superintendent / grievance officer → State authority where applicable → NPPA for drug/device prices → Consumer Commission. P2 insurer-side ladder: GRO → Bima Bharosa → Ombudsman → Consumer Commission. | M |
| RE-09 | Where findings are not enough to act on, say so and suggest which additional document would resolve the uncertainty. | M |
| RE-10 | Provide "What to do next time" learnings (e.g., choose a room within the cap; request the itemised bill daily; consider a consumables add-on at renewal). These are informational, not product recommendations. | S |

### 12.8 Reporting (RP)

| ID | Requirement | Priority |
|---|---|---|
| RP-01 | Generate an **Audit Report** per case with seven parts: (1) Summary (total billed, advances/payments, balance due, number and value of findings by class, top 3 actions); (2) Bill Breakdown; (3) Findings by category, with evidence; (4) Line-item appendix with explanations; (5) Checks performed and checks not possible, with reasons; (6) Next steps and hospital escalation ladder; (7) Disclaimers, the scope statement ("this checks hospital billing only, not insurance coverage") and the rule-version stamp. P2 adds Policy Key Terms and how they were applied, settlement reconciliation and the Patient Payable Breakdown. | M |
| RP-02 | A **one-page Billing Desk Summary** to show or forward to hospital staff. It uses a neutral tone and specific line references, with no internal confidence jargon (design rules in §9.3). | M |
| RP-03 | The report can be downloaded as a PDF during the session and forwarded by the user to family or the hospital. There is no in-app link, because nothing is stored to link to. | M |
| RP-04 | Reports use plain language at roughly a school-leaver reading level. Medical and insurance terms have inline explanations. | M |
| RP-06 | The report regenerates within the session when documents or corrections are added. | M |
| RP-08 | The report includes a clearly visible statement of limitations: based on the documents provided; not legal or medical advice; no guarantee; insurance coverage not assessed in the MVP. | M |

### 12.9 Action support (AD)

| ID | Requirement | Priority |
|---|---|---|
| AD-01 | Users can mark each finding "Will ask" or "Not pursuing". The marked findings form a checklist in the downloaded report. | M |
| AD-05 | The downloaded report can include a dispute pack: findings, evidence index, templates and escalation ladder. | S |
| AD-06 | The platform does not send communications to hospitals or insurers on the user's behalf in the MVP; the user sends them. | M (constraint) |

### 12.10 Feedback and outcome capture (FB)

| ID | Requirement | Priority |
|---|---|---|
| FB-01 | Users can rate each finding (helpful / not helpful / wrong), with an optional reason. | M |
| FB-02 | Within the session, capture what the user already knows (e.g., whether an already-paid bill was revised) and what they plan to do. Longer-term outcomes cannot be captured in the product (D12). | M |
| FB-04 | With consent, stored structured data and in-session feedback are aggregated by hospital, city, procedure and finding type, for internal quality and future benchmarks. | M |

### 12.11 Trust, safety and disclaimers (TS)

| ID | Requirement | Priority |
|---|---|---|
| TS-01 | Clear disclaimers at onboarding, on reports and on templates: informational; not legal, medical or financial advice; no guarantee of outcome; the user is responsible for their communications. | M |
| TS-02 | Language guardrails: findings and templates avoid accusatory terms ("fraud", "scam", "cheating") and state uncertainty appropriately (§13.1.5). | M |
| TS-03 | Escalate safety-sensitive content to guidance on immediate rights and helplines, not just billing advice. Example: the user indicates the patient is being denied discharge or treatment because of a billing dispute. (See G-12.) | S |
| TS-04 | No advertisements or referral fees from hospitals, insurers or lenders appear in reports. | M |

### 12.12 Knowledge base and rule library (KB, internal)

| ID | Requirement | Priority |
|---|---|---|
| KB-01 | Rule owners (the product team in Phase 1, D22; domain experts later) can maintain the check catalogue and reference lists without engineering releases. Reference lists: IRDAI non-payable lists, NPPA ceiling prices, device caps, insurer product wordings, grievance timelines. | M |
| KB-02 | Every rule and reference item has a source, effective dates, version, owner and review date. | M |
| KB-03 | A defined process monitors regulatory change (IRDAI circulars, NPPA notifications, Ombudsman rules) and updates rules within a target window. | M |
| KB-04 | Individual checks can be switched on or off, and kept in "shadow mode" (run but not shown to users) until they pass testing against the Labelled Test Set. | M |
| KB-05 | A standard catalogue for line-item normalisation (drugs, consumables, procedures, investigations) is maintained and expanded from cases. | M |
| KB-06 | Public-facing knowledge content (e.g., "What is proportionate deduction?") is consistent with the rule library. | S |

### 12.13 Operations and admin (OP, internal)

| ID | Requirement | Priority |
|---|---|---|
| OP-01 | Operations views for quality measurement against the Labelled Test Set and for check promotion. Expert queues and SLA tracking are added from P1.5. | M |
| OP-02 | Role-based access for ops, experts, domain admins and support; all access to personal data is logged. | M |
| OP-03 | Customer support tools explain data practices. Payment refunds by payment reference are added from P1.5. There is no case lookup, because cases are not linked to users. | M |
| OP-04 | A quality dashboard: extraction accuracy, finding precision by check, missed-issue rate, user ratings; SLA adherence from P1.5. | M |
| OP-05 | Content moderation for user-generated text (e.g., abusive language in templates users have edited); guidance only. | C |

### 12.14 Labelled Test Set (LT, P0 and pre-launch; internal)

The Labelled Test Set is the only measure of accuracy in Phase 1 (D11). Every check must pass against it before going live. Every model, prompt or rule change is retested against it (NFR-17).

| ID | Requirement | Priority |
|---|---|---|
| LT-01 | Target size of **300–500 bills**, covering a mix of: hospital sizes (corporate, mid-size, nursing home); States; procedure types; bill formats (itemised, summary, interim, final); capture quality (PDF, photo). Include discharge summaries where available. (See G-16.) | M |
| LT-02 | **Source 1 — personal network:** the team, friends and family share past hospital bills with written consent (D28). | M |
| LT-03 | **Source 2 — synthetic bills:** the team creates realistic bills modelled on real formats, with known errors planted and recorded as the answer key (D28). Planted errors include duplicates, MRP excess, wrong room days, arithmetic errors, unbundled items and implant prices above the cap. | M |
| LT-04 | **Source 3 — closed pre-launch beta:** invited testers use the product before launch and explicitly agree, in a separate consent, that their original bills are kept for testing (D28). Testers are recruited and contacted outside the product. Any tester contact details are held only by the beta programme and never linked to product data. (See G-17.) | M |
| LT-05 | Bills from public posts (forums, social media, court judgments) are **not** used without the patient's consent. | M |
| LT-06 | **Identifiers are blacked out on receipt** on every real bill: patient name, UHID/IP number, phone, address and policy number (D30). | M |
| LT-07 | **Labelling of real bills:** two team members label each bill independently, reading every line item correctly and marking every real error with its check ID. They reconcile differences against public sources (printed MRP, NPPA lists, IRDAI lists, discharge summary) and record the reason (D29). | M |
| LT-08 | **Synthetic bills** use their planted answer key and need no separate labelling (D29). | M |
| LT-09 | Labels are never produced by, or pre-filled from, the AI being tested (D29). | M |
| LT-10 | Redacted originals and labels are **kept for the life of the test set** (e.g., through P2). This is separate from the 3-month product retention rule (D24, D30). | M |
| LT-11 | Testers and network contributors can **withdraw their bills at any time**. Withdrawn bills are deleted from the set and from future test runs (D30). | M |
| LT-12 | Coverage of the set is reported by State, hospital type, procedure, format and capture quality, so gaps are visible before a check is promoted. | S |
| LT-13 | The set is topped up periodically with newly consented bills (network or beta) to promote Wave 2–3 checks. | S |

### 12.15 Phase 1.5 requirements — Expert Review, payments and expert-labelled reports

> No experts in Phase 1, including for internal quality review (D10, D11). Phase 1 is completely free (D25).

**Expert Review (EX)**

| ID | Requirement | Priority |
|---|---|---|
| EX-01 | Users can request a paid Expert Review of their audit by giving an **email address only** (no phone number, no account). The email is used only for this review. | P1.5 |
| EX-02 | Experts see the extracted data and AI findings (and original files, if kept for open reviews — Q-P11). They can confirm, edit, reject or add findings, and add commentary. | P1.5 |
| EX-03 | Experts can correspond with the user manually by email, e.g., to request more documents. | P1.5 |
| EX-04 | The expert deliverable is a revised report plus a personalised action plan, sent **manually by email as a PDF**. | P1.5 |
| EX-05 | Expert turnaround commitments are shown before payment, with refund or credit rules if missed. | P1.5 |
| EX-06 | Expert panel roles: medical billing specialists; ex-TPA/insurer claims professionals; doctors for clinical-consistency questions; advocates for legal-route guidance, only through a compliant arrangement (§19). | P1.5 |
| EX-07 | Expert access to data is limited to assigned cases, logged and time-bound. | P1.5 |
| EX-08 | Expert QA: experts review a sample of AI-only audits to measure quality, using stored structured data (original files are not kept). | P1.5 |
| EX-09 | Expert corrections feed the backlog for improving the rule library and reference data. | P1.5 |
| EX-10 | Expert onboarding requires credential verification, a conflict-of-interest declaration (e.g., current employment at a hospital or insurer in the case) and confidentiality undertakings. | P1.5 |
| RP-07 | Expert-reviewed reports are clearly labelled with the reviewer's credentials (role, not necessarily name) and the date. | P1.5 |

**Payments and pricing (PY)**

| ID | Requirement | Priority |
|---|---|---|
| PY-01 | Users can pay for the Audit Report and Expert Review with common Indian payment methods (UPI, cards, net banking). (Whether the Audit Report becomes paid is open — G-01.) | P1.5 |
| PY-02 | Pricing can be configured by tier, bill-value band and urgency. | P1.5 |
| PY-03 | The Free Scan shows enough value to be useful on its own (summary, top flags, document checklist) without dark patterns. (See G-01.) | P1.5 |
| PY-04 | Users receive GST-compliant invoices. | P1.5 |
| PY-05 | Refund and credit policy for failed processing, handled by payment reference (no user account). | P1.5 |
| PY-06 | Promo codes. Referral credits are not offered, because they need a user identity. | P1.5 |
| PY-07 | Corporate-sponsored access codes (to test B2B2C). | P1.5 |

### 12.16 Phase 2 requirements — Insurance Check (specified ahead)

> Deferred to P2 by D5. They are kept here so P2 is already specified.

**Policy Understanding (PU)**

| ID | Requirement | Priority |
|---|---|---|
| PU-01 | Produce a plain-language **Policy Key Terms** card for each policy in the case: sum insured available, room-rent eligibility, co-pay, sub-limits relevant to the diagnosis, waiting periods, add-ons and deadlines. | P2 |
| PU-02 | Where the user cannot provide the policy wording, fall back to the insurer's publicly filed wording for the identified product and version. Label it clearly: "standard wording — your policy may differ". | P2 |
| PU-03 | Determine the eligible room category/amount for the case and compare it with the room actually used, per day, including changes during the stay. | P2 |
| PU-04 | Identify which policy clauses are likely to affect this claim (e.g., cataract sub-limit, PED waiting period, modern-treatment cap). | P2 |
| PU-05 | For group policies where the employee does not have the wording, guide the user to get it from HR or the broker, and support manual entry of key terms. | P2 |
| PU-06 | Flag waiting-period or exclusion risks. This is informational, not a decision on whether the claim is admissible. | P2 |

**Phase 2 extensions to Phase 1 modules**

| ID | Module | Requirement | Priority |
|---|---|---|---|
| DU-01b | Upload | Users can upload insurance documents: policy schedule / e-card / policy wording / CIS; pre-authorisation and final authorisation letters; settlement letters / claim settlement advice; query and deficiency letters. | P2 |
| CM-05 | Case | A case can include multiple policies (e.g., group + retail + top-up), and the system records the order of claim. | P2 |
| DP-05 | Processing | Extract from policy documents: insurer, product, policy type (individual/floater/group), sum insured and balances (incl. bonuses/restore), room-rent limit (absolute or % of SI; ICU limit), co-pay (general/age/zone/network), deductible, disease and procedure sub-limits, waiting periods (initial, PED, specific), modern-treatment limits, add-ons (e.g., consumables/non-medical cover), pre/post-hospitalisation periods, ambulance and day-care cover, and claim intimation and document submission timelines. | P2 |
| DP-06 | Processing | Extract from pre-authorisation/final approval letters and settlement letters: claimed, approved and deducted amounts; each deduction with its amount and stated reason; co-pay; policy deductions; date of approval/settlement; TPA/insurer reference. | P2 |
| DP-18b | Processing | Tag each line with insurance attributes: room-linked (for proportionate deduction), IRDAI List I membership, and typically payable / typically non-payable / depends on policy. | P2 |
| AA-07 | Audit | Identify **potentially non-payable items** under IRDAI lists and the user's policy, and indicate who should bear them. | P2 |
| AA-09 | Audit | Apply **policy rules**: room-rent cap and proportionate deduction (incl. exclusion of pharmacy, implants, diagnostics and ICU per IRDAI guidelines), co-pay, deductibles, sub-limits, add-on covers. | P2 |
| AA-10 | Audit | **Reconcile the settlement**: map each insurer deduction to bill lines and policy clauses; flag deductions without reasons, with reasons inconsistent with the policy, or with incorrect arithmetic. | P2 |
| AA-15b | Audit | Produce a **Patient Payable Breakdown** of the amount the patient paid or is asked to pay: (a) policy-legitimate (co-pay, deductible, non-payables per policy); (b) potentially contestable with the insurer; (c) potentially contestable with the hospital; (d) unexplained. | P2 |
| AD-04 | Action | Users can draft responses to insurer queries and deficiency letters, with document suggestions. | P2 |

### 12.17 Won't requirements (kept for traceability)

| ID | Requirement as specified | Reason | Future |
|---|---|---|---|
| ON-10 | Save insurer and policy details for reuse | No user profiles (D6) | — |
| CM-04 | Share a case with other people inside the product | D7; the user forwards the PDF | — |
| CM-06 | Activity log of interactions (calls, emails, visits) with date, party and notes | D7 | — |
| CM-07 | Cases close automatically after inactivity with a reminder, and stay retrievable until the retention period ends | D6, D7 | — |
| DU-12 | Bulk upload for multiple cases | Professional tier | P3 |
| RP-05 | Report output in Hindi | D9 | P3 |
| AD-02 | Track disputes over time (the report includes the full hospital escalation ladder instead) | D7 | — |
| AD-03 | Reminders (P2 insurance deadlines are shown in the report instead) | No contact details (D6) | — |
| NT-01 | Notify the user when processing completes, when documents are missing or unreadable, when an expert responds, and before deadlines | No contact details (D6); all information is shown in the session and the report | — |
| NT-02 | Notification channels: SMS and email (M), WhatsApp (S), with user opt-in | As NT-01 | — |
| NT-03 | Notifications never include detailed health information in the message body; they link to the authenticated case | As NT-01 | — |
| FB-03 | Periodic follow-ups (e.g., 15 / 45 / 90 days) for disputes still in progress | D7, D12 | — |

---

## 13. Audit Intelligence Framework

This framework sets the business requirements: **what** the platform checks and **how findings are expressed**. It deliberately contains no implementation logic, thresholds or algorithms. Engineering and the rule owners own those, and validate them against the Labelled Test Set.

### 13.1 Framework principles

#### 13.1.1 Finding classes
| Class | User-facing label | Meaning | Typical examples | Required evidence | Phase |
|---|---|---|---|---|---|
| **Likely Error** | "Likely error" | An objective, verifiable discrepancy | Arithmetic error; exact duplicate; price above printed MRP; more room days billed than the length of stay | Bill line(s) + computation | P1 |
| **Potential Concern** | "Worth asking" | A plausible problem that needs clarification from the hospital (or, from P2, the insurer) | Consumables far above typical for the stay; item possibly included in the package; investigation not referenced in the discharge summary | Bill line(s) + reason + reference (benchmark, package terms, clinical record) | P1 |
| **Observation** | "Explained" / information | Informational; helps understanding or future decisions | "This is the hospital's standard registration fee"; "Ask for the implant sticker for your records" | As relevant | P1 |
| **Policy Impact** | — | Rupees affected by insurance policy terms, whether correctly or possibly incorrectly applied | Proportionate deduction; co-pay; sub-limit; non-payable lists | Policy clause + computation | P2 |

The Bill Breakdown (AA-15) maps these classes onto four parts of the bill total: likely errors / worth asking / explained / not assessable. The North Star (§8.3) counts "Likely error" and "Worth asking" findings. *(The label mapping is inferred from v0.13 usage; please confirm.)*

#### 13.1.2 Confidence levels
- **High:** objective evidence in the documents; low chance of misreading.
- **Medium:** a strong indication that depends on an interpretation or a missing document.
- **Low:** pattern-based; worth asking only if the amount is material.

Low-confidence findings below a value threshold are shown collapsed under "Other things you may ask about", so users are not overwhelmed.

#### 13.1.3 "Who to Ask" classification (core differentiator)
| Label | Meaning | Phase |
|---|---|---|
| **Ask the hospital** | The issue comes from the hospital's billing: error, duplicate, unbundling, price above MRP/cap, missing itemisation. | P1 |
| **Explained — no billing concern** | The charge is consistent with the bill and records. We explain what it is so the user can stop worrying. In the MVP this says nothing about whether insurance will pay for it. | P1 |
| **Complex — consider professional advice** (P1) / **Needs expert review** (P1.5) | Complex, high-value or clinical. In P1 the report says so plainly and suggests seeking professional advice; from P1.5 it offers paid Expert Review. | P1 / P1.5 |
| **Ask the insurer / TPA** | The issue is in how the policy was applied or how the settlement was computed. | P2 |
| **Ask HR / broker** | Group policy terms or servicing issues (corporate employees). | P2 |
| **Legitimately payable by you** | Correct under the user's policy (e.g., co-pay, List I items without an add-on), explained so the user can stop worrying. | P2 |

#### 13.1.4 Impact and prioritisation
Each finding carries the amount potentially affected (₹), confidence, effort to pursue (Low/Medium/High) and route. These combine into the "Top 3 actions" list (RE-05).

#### 13.1.5 Language standards
- Use "appears", "may", "we could not find" and "worth asking" for findings that are not objective.
- Never use "fraud", "scam", "cheating" or "illegal" in findings or templates.
- Always reference the specific bill line and page.
- Explain jargon inline.

### 13.2 Audit categories

| Code | Audit category | Purpose | Phase |
|---|---|---|---|
| DOC | Document completeness & integrity | Is the input complete and consistent enough to audit and to claim? | P1 |
| CAL | Arithmetic & computation | Are the numbers on the bill internally correct? | P1 |
| DUP | Duplicates & redundancy | Is anything charged more than once? | P1 |
| LOS | Stay & time consistency | Do charges match the dates, times and length of stay? | P1 |
| RM | Room & ward | Is room billing correct, and how does it interact with the policy? | P1 (policy interaction P2) |
| PF | Professional fees | Are doctor fees consistent with the care documented? | P1 |
| RX | Pharmacy | Are drugs priced and quantified correctly? | P1 |
| CON | Medical consumables | Are consumables correctly billed, quantified and classified? | P1 |
| IMP | Implants & devices | Are implants documented, counted and priced within caps? | P1 |
| DX | Diagnostics | Are investigations documented, not duplicated and appropriately priced? | P1 |
| OT | Operation theatre & procedure | Are OT and procedure charges consistent with the procedure documented? | P1 |
| PKG | Package billing | Is package billing consistent with the package terms? | P1 |
| CLN | Clinical–billing consistency | Do billed services match what the discharge summary documents? (Observations only.) | P1 |
| ADM | Administrative charges & taxes | Are admin charges and taxes appropriate? | P1 |
| PRC | Price benchmarking | How do prices compare with references? | P2–P3 |
| POL | Policy application | Are policy terms applied correctly to this claim? | P2 |
| SET | Settlement reconciliation | Does the insurer's settlement reconcile with the bill and policy? | P2 |
| TAT | Process & timeline compliance | Were regulatory timelines met; are user rights triggered? | P2 |
| RMB | Reimbursement readiness & missed value | Is the claim complete; is the user leaving claimable money unclaimed? | P2 |

POL, SET, TAT and RMB together form the **Insurance Check** (P2).

### 13.3 Types of billing errors
| # | Error type | Description |
|---|---|---|
| E1 | Arithmetic error | Quantity × rate ≠ amount; wrong subtotal or total. |
| E2 | Duplicate entry | The same service or item billed twice (same or different sections; interim and final bill). |
| E3 | Wrong patient / wrong case entry | Items belonging to another patient (name, age or gender inconsistent). |
| E4 | Date/time error | Charges outside the admission–discharge window; room days miscounted. |
| E5 | Wrong rate | The rate differs from the hospital's own tariff, or within the same bill for the same item. |
| E6 | Wrong category / room-linked rate | Services billed at a higher room-category rate than the room actually occupied. |
| E7 | Uncredited payments | Advance, deposit, insurer approval or discount not reflected. |
| E8 | Uncredited returns | Unused or returned medicines or consumables not credited. |
| E9 | Tax error | Tax applied to exempt items or at the wrong rate. |
| E10 | Transcription / data-entry error | An extra zero, wrong quantity or misplaced decimal. |
| E11 | Carry-forward error | Interim bill amounts carried into the final bill twice. |

### 13.4 Types of overcharging
| # | Overcharging type | Description | Objective vs judgement |
|---|---|---|---|
| O1 | Above MRP | Packaged drug, consumable or device charged above the printed MRP. | Objective |
| O2 | Above statutory ceiling | Scheduled drug above the NPPA ceiling price; stent or knee implant above the NPPA cap. | Objective (if correctly identified) |
| O3 | Unbundling | Items that are part of a procedure, room or package billed separately (incl. IRDAI List II–IV items). | Semi-objective |
| O4 | Upcoding | A higher-intensity service billed than was delivered (e.g., ICU rate for a ward stay; complex procedure rate for a simpler one). | Judgement — needs records |
| O5 | Quantity inflation | Consumables or drugs in quantities implausible for the stay or procedure. | Judgement — benchmark |
| O6 | Phantom billing | Services or investigations billed without evidence they were provided. | Judgement — needs records |
| O7 | Excess stay billing | Extra day(s) billed because of discharge delays not caused by the patient (incl. insurer authorisation delays). | Semi-objective |
| O8 | Room-linked escalation | Professional, nursing or investigation rates raised because of the room category. | Policy / hospital tariff |
| O9 | Price outlier | A charge much higher than peer benchmarks. | Judgement — benchmark; a question, not an accusation |
| O10 | Opaque catch-alls | "Miscellaneous", "Others" or "Hospital services" without itemisation. | Missing detail |
| O11 | Package scope creep | Package price plus extras that should be inside the package. | Semi-objective — needs package terms |
| O12 | Insurance-status pricing | Higher rates for insured patients than for self-pay patients for the same service. | Judgement — needs tariff evidence (P2+) |

### 13.5 Risk indicators
Risk indicators are patterns that **raise the priority of review**. They are never shown to users as conclusions about intent. Indicators R13–R17 (insurer direction) apply from P2.

| # | Indicator | Why it matters | Direction |
|---|---|---|---|
| R1 | Summary bill only; no itemisation offered | Hides unbundling and quantity issues | Hospital |
| R2 | High share of "miscellaneous / others / hospital services" | Opaque charges | Hospital |
| R3 | Consumables share of the bill unusually high for the procedure type | Quantity inflation / unbundling | Hospital |
| R4 | Many low-value repeated lines (e.g., gloves/syringes daily in large numbers) | Quantity inflation | Hospital |
| R5 | Charges clustered on the discharge day | Possible catch-up or carry-forward billing | Hospital |
| R6 | ICU, ventilator or monitoring billed without matching documentation | Upcoding / phantom billing | Hospital |
| R7 | Implant billed without a sticker or invoice | Price and count cannot be verified | Hospital |
| R8 | Room category upgraded late or mid-stay | Proportionate-deduction exposure; rate escalation | Both |
| R9 | Professional visits exceed plausible frequency | Duplication | Hospital |
| R10 | Package bill with a long list of "exclusions" | Package scope creep | Hospital |
| R11 | Bill revised multiple times / handwritten alterations | Integrity questions | Hospital |
| R12 | Round-figure charges | Estimation rather than actual usage | Hospital |
| R13 | Settlement deductions labelled generically ("as per policy", "non-medical") | Unexplained deductions | Insurer |
| R14 | Deduction percentage of the claim unusually high | Possible misapplication | Insurer |
| R15 | Proportionate deduction applied across all heads | Likely incorrect application | Insurer |
| R16 | Repeated deficiency or query letters | Process friction; documents the insurer should collect | Insurer |
| R17 | Long gap between discharge request and final authorisation | Delay costs borne by the insurer under the rules | Insurer |
| R18 | Name or date inconsistencies across documents | Risk of claim rejection | User / Hospital |

### 13.6 Check catalogue — hospital billing

**Phase column**
- **P1 · Launch**: live at launch (the 18-check launch set).
- **P1 · W2 / P1 · W3**: Phase 1 rollout waves after launch (§13.8).
- **P1 · Guidance**: DOC checks that run from launch as document guidance.
- **P2 / P3**: deeper checks that need more data, benchmarks or experts.

**Inputs:** B = bill; IB = itemised bill; DS = discharge summary; PO = policy; SL = settlement/approval letter; IV = implant invoice/sticker; PB = pharmacy bill.

**"Who to ask" column legend**
- Hospital = *Ask the hospital*
- Explained = *Explained — no billing concern*
- Insurer = *Ask the insurer / TPA*
- Legit = *Legitimately payable by you*
- Expert = *Complex — consider professional advice* (P1) / *Needs expert review* (P1.5)
- User = the user must act (e.g., supply missing pages). Not yet a defined label (G-18).

#### DOC — Document completeness & integrity
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| DOC-01 | Itemised bill available | Detect summary-only bills and guide user to request itemisation | B | Observation | Hospital | P1 · Guidance |
| DOC-02 | Missing pages / page totals | Ensure all pages present and totals tie | B | Observation | User | P1 · Guidance |
| DOC-03 | Identity consistency | Patient name, age, gender, UHID match across bill pages, pharmacy bills and discharge summary (policy and settlement documents added in Phase 2) | B, DS | Potential Concern | Hospital | P1 · Guidance |
| DOC-04 | Date consistency | Admission/discharge dates and times match across bill and discharge summary | B, DS | Potential Concern | Hospital | P1 · Guidance |
| DOC-05 | Implant documentation | Implant billed → invoice/sticker present | IB, IV | Observation | Hospital | P1 · Guidance |
| DOC-06 | Pharmacy detail sufficiency | Drug lines carry batch/MRP/quantity | PB | Observation | Hospital | P1 · Guidance |
| DOC-07 | Discharge summary completeness | Diagnosis, procedure, dates, signatures present | DS | Observation | Hospital | P1 · Guidance |
| DOC-08 | Payment evidence | Receipts reconcile with amounts paid | B, receipts | Potential Concern | Hospital | P1 · Guidance |
| DOC-09 | Document alteration indicators | Overwriting, inconsistent fonts, manual edits | B | Observation (internal risk) | Expert | P2 |

#### CAL — Arithmetic & computation
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| CAL-01 | Line amount | Quantity × rate equals amount | IB | Likely Error | Hospital | P1 · Launch |
| CAL-02 | Section subtotals | Lines add to section subtotals | IB | Likely Error | Hospital | P1 · Launch |
| CAL-03 | Grand total | Sections add to bill total | B | Likely Error | Hospital | P1 · Launch |
| CAL-04 | Discounts | Stated discounts correctly applied | B | Likely Error | Hospital | P1 · Launch |
| CAL-05 | Taxes | Tax computed on correct base and only on taxable items **[VALIDATE GST treatment]** | B | Potential Concern | Hospital | P1 · W2 |
| CAL-06 | Credits | Advances, deposits and refunds credited (insurer approvals checked against approval letters in Phase 2) | B, receipts | Likely Error | Hospital | P1 · Launch |
| CAL-07 | Balance due | Balance due = bill total − discounts − advances/payments − any insurer-approved amount printed on the bill | B, receipts | Likely Error | Hospital | P1 · Launch |

#### DUP — Duplicates
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| DUP-01 | Exact duplicates | Same item, date, quantity, amount repeated | IB | Likely Error / Potential Concern | Hospital | P1 · Launch |
| DUP-02 | Same-day repeats | Same item/service multiple times a day beyond plausibility | IB | Potential Concern | Hospital | P1 · W2 |
| DUP-03 | Repeated investigations | Same test repeated in a short interval without documented reason | IB, DS | Potential Concern | Hospital | P1 · W3 |
| DUP-04 | Duplicate professional visits | Same doctor, same day, multiple visits billed | IB | Potential Concern | Hospital | P1 · W2 |
| DUP-05 | Package + itemised | Item billed separately and as part of package | IB | Potential Concern | Hospital | P1 · W2 |
| DUP-06 | Cross-bill duplicates | Same item on hospital bill and separate pharmacy bill | IB, PB | Likely Error | Hospital | P1 · Launch |
| DUP-07 | Interim carry-forward | Interim amounts counted twice in final bill | B (interim + final) | Likely Error | Hospital | P1 · Launch |

#### LOS — Stay & time consistency
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| LOS-01 | Room days vs stay | Room days billed match length of stay per hospital's day convention | IB, DS | Likely Error / Potential Concern | Hospital | P1 · Launch |
| LOS-02 | ICU days vs record | ICU/HDU days billed match documented ICU period | IB, DS | Potential Concern | Hospital | P1 · W3 |
| LOS-03 | Out-of-window charges | Charges dated before admission or after discharge | IB | Likely Error | Hospital | P1 · Launch |
| LOS-04 | Per-day service counts | Nursing, RMO, monitoring days vs stay | IB | Potential Concern | Hospital | P1 · W3 |
| LOS-05 | Discharge-delay charges | Extra charges attributable to delayed cashless final authorisation (insurer to bear per IRDAI) **[VALIDATE]** | IB, SL, timestamps | Policy Impact | Insurer | P2 |
| LOS-06 | Overlapping services | Services timed during OT/other locations | IB, DS | Observation | Expert | P2 |

#### RM — Room & ward
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| RM-01 | Room category consistency | Billed category matches category occupied (incl. transfers) | IB, DS | Potential Concern | Hospital | P1 · W3 |
| RM-02 | Room rate consistency | Rate constant within category over stay | IB | Likely Error | Hospital | P1 · Launch |
| RM-03 | Room vs eligibility | Room rent per day vs policy eligibility; quantify exposure | IB, PO | Policy Impact | Insurer / Legit | P2 |
| RM-04 | Room-linked rate escalation | Visit/nursing/investigation rates tied to room category | IB | Observation | Hospital | P2 |
| RM-05 | Subsumed room items | List II items billed separately | IB | Potential Concern | Hospital | P1 · W2 |
| RM-06 | Attendant/companion charges | Identify and explain attendant/companion charges (insurance treatment explained in Phase 2) | IB | Observation | Explained | P1 · W2 |

#### PF — Professional fees
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| PF-01 | Visit frequency | Consultant visits per day vs plausibility | IB | Potential Concern | Hospital | P1 · W3 |
| PF-02 | Undocumented doctors | Fees for doctors not mentioned in discharge summary | IB, DS | Potential Concern | Hospital | P1 · W3 |
| PF-03 | Assistant surgeon / anaesthetist | Fees consistent with documented procedure team | IB, DS | Potential Concern | Hospital | P2 |
| PF-04 | Fee proportion benchmark | Surgeon/anaesthesia fees vs typical proportion | IB | Potential Concern | Hospital | P2 |
| PF-05 | Surcharges | Emergency/after-hours surcharges identified and explained | IB | Observation | Hospital | P1 · W2 |

#### RX — Pharmacy
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| RX-01 | Above MRP | Charged price exceeds printed MRP | PB/IB | Likely Error | Hospital (NPPA if unresolved) | P1 · Launch |
| RX-02 | Above ceiling price | Scheduled formulation above NPPA ceiling | PB/IB | Likely Error | Hospital (NPPA) | P1 · Launch |
| RX-03 | Rate inconsistency | Same drug at different unit rates in the same bill | PB/IB | Likely Error | Hospital | P1 · Launch |
| RX-04 | Quantity plausibility | Quantities vs days of stay / typical dosing | PB/IB, DS | Potential Concern | Hospital | P1 · W3 |
| RX-05 | Returns credit | Unused medicines returned but not credited (prompt user) | PB | Observation | Hospital | P1 · W3 |
| RX-06 | Discharge medicines | Discharge medicines identified (claim treatment may differ) | PB, DS | Observation | Insurer | P2 |
| RX-07 | Diagnosis-inconsistent drugs | Drugs not plausibly related to diagnosis/age/gender | PB, DS | Observation | Expert | P2 |

#### CON — Medical consumables
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| CON-01 | List I items | Identify optional/non-covered items; check add-on | IB, PO | Policy Impact | Legit / Insurer if add-on | P2 |
| CON-02 | List II–IV items billed separately | Unbundled items that should be subsumed | IB | Potential Concern | Hospital | P1 · W2 |
| CON-03 | Consumable quantity plausibility | Quantities per day/procedure vs typical | IB | Potential Concern | Hospital | P1 · W3 |
| CON-04 | Consumables above MRP | Price above printed MRP | IB | Likely Error | Hospital | P1 · Launch |
| CON-05 | Kit + components | Kit billed alongside its components | IB | Potential Concern | Hospital | P1 · W2 |
| CON-06 | Unspecified consumables | "Consumables / miscellaneous" without detail | IB | Potential Concern | Hospital | P1 · W2 |

#### IMP — Implants & devices
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| IMP-01 | Implant evidence | Invoice/sticker present and matches bill | IB, IV | Potential Concern | Hospital | P1 · W2 |
| IMP-02 | Price cap | Coronary stents/knee implants within NPPA caps; others within MRP | IB, IV | Likely Error | Hospital (NPPA) | P1 · Launch |
| IMP-03 | Implant count | Number billed vs documented in discharge summary/procedure note | IB, DS | Potential Concern | Hospital | P1 · W3 |
| IMP-04 | Implant identity | Brand/model/batch consistent across bill and sticker | IB, IV | Potential Concern | Hospital | P2 |

#### DX — Diagnostics
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| DX-01 | Unreported investigations | Investigations billed without report/mention | IB, DS | Potential Concern | Hospital | P1 · W3 |
| DX-02 | Imaging consumables | Film/CD/DVD charges (List III) | IB | Potential Concern | Hospital | P1 · W2 |
| DX-03 | Diagnostic price outliers | Test prices vs benchmarks | IB | Potential Concern | Hospital | P2 |

#### OT — Operation theatre & procedure
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| OT-01 | Procedure documented | Billed procedure matches discharge summary | IB, DS | Potential Concern | Hospital | P1 · W3 |
| OT-02 | Multiple OT charges | More than one OT charge for a single surgery | IB, DS | Potential Concern | Hospital | P1 · W3 |
| OT-03 | OT duration | OT hours billed vs documented duration | IB, OT notes | Potential Concern | Hospital | P2 |
| OT-04 | Anaesthesia type | Billed anaesthesia type vs documented | IB, DS | Potential Concern | Hospital | P2 |

#### PKG — Package billing
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| PKG-01 | Package rate | Billed package price matches stated package/estimate | IB, estimate | Likely Error | Hospital | P1 · Launch |
| PKG-02 | Inclusions billed separately | Items typically inside the package billed extra | IB, package terms | Potential Concern | Hospital | P1 · W2 |
| PKG-03 | Stay beyond package | Extra days billed beyond package days; reason documented? | IB, DS | Potential Concern | Hospital | P1 · W3 |
| PKG-04 | Package terms missing | Package billed without inclusion/exclusion list | IB | Observation | Hospital | P1 · W2 |
| PKG-05 | Network agreed tariff | Patient asked to pay difference above insurer-agreed package/tariff **[VALIDATE]** | IB, SL | Potential Concern | Hospital/Insurer | P2 |

#### CLN — Clinical–billing consistency (observations only; no medical judgement)
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| CLN-01 | Gender/age-inconsistent items | Items implausible for the patient profile | IB, DS | Potential Concern | Hospital | P1 · W3 |
| CLN-02 | Diagnosis-unrelated services | Services not plausibly related to diagnosis | IB, DS | Observation | Expert | P2 |
| CLN-03 | Blood products | Units billed vs units documented | IB, DS | Potential Concern | Hospital | P2 |
| CLN-04 | Critical-care equipment | Ventilator/monitor/infusion billed vs documented use | IB, DS | Potential Concern | Hospital | P2 |

#### ADM — Administrative charges & taxes
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| ADM-01 | Registration/admission | Admission/registration charges (List IV) identified | IB | Potential Concern | Hospital | P1 · W2 |
| ADM-02 | Documentation charges | Medical records/documentation/discharge processing (List II) | IB | Potential Concern | Hospital | P1 · W2 |
| ADM-03 | Service charges / surcharges | Percentage service charges on top of itemised services | IB | Potential Concern | Hospital | P1 · W2 |
| ADM-04 | GST applicability | Tax on items/services where applicability is questionable **[VALIDATE current GST treatment of hospital room rent and services]** | IB | Potential Concern | Hospital | P1 · W3 |


#### PRC — Price benchmarking
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| PRC-01 | Hospital's own tariff | Billed rate vs hospital's displayed/published tariff | IB, tariff | Likely Error | Hospital | P2 |
| PRC-02 | Line price benchmark | Line-item price vs anonymised peer benchmark (city, hospital tier) | IB | Potential Concern | Hospital | P2 |
| PRC-03 | Procedure total benchmark | Total for procedure vs peer distribution | B | Observation | Hospital | P3 |
| PRC-04 | Government schedule context | CGHS/PM-JAY rates shown as context only, explicitly not as caps | IB | Observation | — | P2 |


### 13.7 Check catalogue — Phase 2 Insurance Check
Inputs and legend as in §13.6. Four further insurance checks sit in the hospital categories above: LOS-05, RM-03, RX-06 and CON-01.

#### POL — Policy application
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| POL-01 | Room-rent cap applied correctly | Eligibility amount and excess computed correctly | IB, PO, SL | Policy Impact | Insurer | P2 |
| POL-02 | Proportionate deduction scope | Not applied to pharmacy, implants/devices, diagnostics, ICU **[VALIDATE]** | IB, PO, SL | Policy Impact / Likely Error | Insurer | P2 |
| POL-03 | Proportionate deduction computation | Ratio and base correctly computed | IB, PO, SL | Policy Impact | Insurer | P2 |
| POL-04 | Co-pay | Correct %, base and single application | PO, SL | Policy Impact | Insurer | P2 |
| POL-05 | Deductible | Correct application | PO, SL | Policy Impact | Insurer | P2 |
| POL-06 | Sub-limits | Correct sub-limit for the procedure/disease; correct amount | DS, PO, SL | Policy Impact | Insurer | P2 |
| POL-07 | Sum insured availability | Bonus/restore/top-up considered | PO, SL | Policy Impact | Insurer | P2 |
| POL-08 | Add-on covers | Consumables/room-rent waiver add-ons honoured | PO, SL | Likely Error | Insurer | P2 |
| POL-09 | Modern treatment limits | Correct cap and applicability | DS, PO, SL | Policy Impact | Insurer | P2 |
| POL-10 | Network/zone co-pays | Applicability correct | PO, SL | Policy Impact | Insurer | P2 |
| POL-11 | Waiting period/PED exposure | Informational risk flag only | DS, PO | Observation | Expert | P2 |
| POL-12 | Multiple policies order | Optimal order and balance-claim eligibility | PO(s), SL | Observation | Insurer/HR | P2 |
| POL-13 | Reasonable & customary | R&C deductions identified and basis requested | SL | Potential Concern | Insurer | P2 |

#### SET — Settlement reconciliation
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| SET-01 | Deduction without reason | Any deducted amount without a stated reason | SL | Potential Concern | Insurer | P2 |
| SET-02 | Generic reasons | Reasons like "as per policy"/"non-medical" without clause or item mapping | SL | Potential Concern | Insurer | P2 |
| SET-03 | Reason vs policy | Stated reason inconsistent with policy wording | SL, PO | Potential Concern | Insurer | P2 |
| SET-04 | Deduction amount vs bill | Deducted amount doesn't match the bill lines it refers to | SL, IB | Likely Error | Insurer | P2 |
| SET-05 | Double deduction | Same rupees deducted under two heads (e.g., non-payable and proportionate) | SL, IB | Likely Error | Insurer | P2 |
| SET-06 | Claimed vs billed | Claimed amount on settlement ≠ total bills (missing bills) | SL, B | Potential Concern | Insurer/User | P2 |
| SET-07 | Subsumed items deducted | List II–IV items deducted when the guidance treats them as part of payable heads **[VALIDATE]** | SL, IB | Potential Concern | Insurer | P2 |
| SET-08 | Final vs initial authorisation | Final approval reduced vs earlier approvals without explanation | SL | Observation | Insurer | P2 |

#### TAT — Process & timeline compliance
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| TAT-01 | Cashless discharge authorisation time | Final authorisation within 3 hours of discharge request **[VALIDATE]** | SL, timestamps | Policy Impact | Insurer | P2 |
| TAT-02 | Settlement delay | Reimbursement settled within regulatory timeline; interest entitlement **[VALIDATE]** | SL, submission date | Policy Impact | Insurer | P2 |
| TAT-03 | Written reasons | Partial/full rejection communicated with reasons and clause references | SL | Potential Concern | Insurer | P2 |
| TAT-04 | Grievance response window | Insurer GRO response overdue → next escalation eligible | User log | Observation | Insurer / Bima Bharosa | P2 |
| TAT-05 | Ombudsman eligibility & limit | Eligibility and filing window computed | User log | Observation | Ombudsman | P2 |
| TAT-06 | Documents insurer should collect | Policyholder asked for documents insurer/TPA should obtain from hospital (cashless) **[VALIDATE]** | Query letters | Observation | Insurer | P2 |

#### RMB — Reimbursement readiness & missed value
| ID | Check | Business intent | Inputs | Class | Who to ask | Phase |
|---|---|---|---|---|---|---|
| RMB-01 | Intimation deadline | Intimation done within policy window | PO, dates | Observation | User | P2 |
| RMB-02 | Submission deadline | Documents submitted within policy window | PO, dates | Observation | User | P2 |
| RMB-03 | Document checklist | Required documents present (claim form, bills, receipts, discharge summary, reports, prescriptions, KYC, bank details; MLC/FIR for accidents) | All | Observation | User | P2 |
| RMB-04 | Identity mismatch | Name/age/policy details consistent with policy records | PO, all | Potential Concern | Hospital | P2 |
| RMB-05 | Pre/post-hospitalisation value | Eligible pre/post bills not included | PO, user bills | Observation (value) | User | P2 |
| RMB-06 | Unclaimed items | Ambulance, day-care, pharmacy bills not included | PO, all | Observation (value) | User | P2 |
| RMB-07 | Expected payout range | Estimate of payable and likely deductions before filing | All | Observation | User | P2 |


### 13.8 Check counts and Phase 1 rollout

**Counts.** The catalogue lists 115 checks:

| Group | Checks |
|---|---|
| Phase 1 hospital-bill checks | 59 |
| Insurance checks reserved for the Insurance Check (POL, SET, TAT, RMB + LOS-05, RM-03, RX-06, CON-01) | 38 |
| Deeper P2–P3 checks | 18 |
| **Total** | **115** |

**Rollout rule (D11, D21).** There are no experts, and launch is 3–4 months after the team is in place, so checks go live in waves. A check goes live only when it meets the precision target (§8.4) against the Labelled Test Set; until then it runs in shadow mode (KB-04). Each check's wave is shown in the Phase column of §13.6.

| Wave | Checks | Notes |
|---|---|---|
| Guidance | 8 (DOC-01–DOC-08) | Run from launch as document guidance |
| Launch | 18 | The most objective checks: arithmetic, credits, exact and cross-bill duplicates, room days, out-of-window charges, MRP/NPPA, package rate |
| Wave 2 (after launch) | 17 | Includes the unbundling checks based on the IRDAI lists (D16), and CAL-05, which depends on GST treatment (Q-D5) |
| Wave 3 (as validated) | 16 | Plausibility and clinical-consistency checks; they need typical-quantity reference data (G-03) |

### 13.9 Phase 2 reference: insurance dispute areas and non-payable categories

> **Use in Phase 1.** IRDAI Lists II–IV (§13.9.2) are also used in the Hospital Bill Check, as an industry reference for *unbundling*: items commonly considered part of room, procedure or treatment charges that were billed separately (RM-05, CON-02, DX-02, ADM-01, ADM-02; tagging DP-18). This applies to all patients, including self-pay, and is always phrased as a question (D16). The MVP does not tell the user what an insurer will or won't pay. Whether this use is legally appropriate has not been reviewed (D27).

#### 13.9.1 Common insurance dispute areas
| # | Dispute area | Typical presentation |
|---|---|---|
| I1 | Room-rent cap and proportionate deduction | Deduction from all associated expenses because the room exceeded eligibility; frequently applied wrongly to pharmacy, implants, diagnostics or ICU **[VALIDATE current IRDAI position]** |
| I2 | Non-payable / "non-medical" deductions | Lump-sum deductions labelled "non-medical" without line-level detail; List II–IV items deducted even though guidance says they are subsumed in room, procedure or treatment costs |
| I3 | "Reasonable and customary" deductions | Deductions for charges deemed above customary levels, often without a disclosed basis |
| I4 | Network / agreed-tariff differences | The insurer pays the agreed tariff and the hospital seeks the difference from the patient **[VALIDATE contractual position]** |
| I5 | Sub-limits | Disease- or procedure-wise caps (e.g., cataract, joint replacement) applied incorrectly or to the wrong procedure |
| I6 | Co-pay | Applied at the wrong rate, on the wrong base (pre- vs post-deduction amount), or twice |
| I7 | Pre-existing disease / waiting period | Partial or full rejection citing PED or waiting periods |
| I8 | Medical necessity / hospitalisation not required | "Could have been done as OPD"; "admission for investigation only" |
| I9 | Modern treatments | Caps on robotic surgery, biologics, etc. |
| I10 | Consumables add-on not honoured | Optional cover bought, but consumables still deducted |
| I11 | Missing documents / deficiency | Claim closed or rejected for missing papers |
| I12 | Delay | Settlement beyond regulatory timelines; interest not paid **[VALIDATE]** |
| I13 | Pre-/post-hospitalisation | Eligible expenses not claimed, or rejected |
| I14 | Multiple policies | Wrong order of claim; balance claim from a second policy refused |
| I15 | Cashless denied or downgraded to reimbursement | Leads to an upfront payment burden |

#### 13.9.2 IRDAI standardised lists (guidelines on standardisation of exclusions / non-payable items)
> Examples are representative. They must be validated against the current IRDAI list and each policy's own annexure before use in the rule library. **[VALIDATE]**

| List | Treatment by insurer | Representative examples | Typical "Who to Ask" |
|---|---|---|---|
| **List I — Items not covered (optionally coverable by add-on)** | Not payable unless the policy or an add-on covers them | Personal/comfort items (toiletries, baby food/utilities, laundry, telephone/TV, guest services), attendant charges, certain bandages/braces/belts/collars/walking aids, private nursing, various certificate/courier/photocopy charges, certain kits without details | Legitimately payable (unless add-on) — but check MRP and quantity with the hospital |
| **List II — Items subsumed into room charges** | Expected to be part of room charges, not separately payable | Housekeeping, air-conditioning, linen/blanket, admission kit, documentation/administrative charges, discharge procedure charges, toiletries such as toothbrush/paste, slippers, disposable caps/shoe covers, bed pan, IM/IV injection charges, diabetic chart charges | **Ask the hospital** why they are billed separately when they are part of the room charge |
| **List III — Items subsumed into procedure charges** | Expected to be part of procedure/OT charges | Surgical blades, drapes, gauze/cotton, surgical apron, hair-removal items, X-ray film, DVD/CD charges, ward/theatre booking, microscope/camera covers, eye kit/drape, orthobundle/gynaec bundle | **Ask the hospital** |
| **List IV — Items subsumed into cost of treatment** | Expected to be part of overall treatment cost | Admission/registration charges, infusion pump (cost), alcohol swabs, urine bag, glucometer strips, disinfectants, blood reservation charges, nutrition/dietician charges | **Ask the hospital** |

#### 13.9.3 Other non-payable / reduced-payout categories
| # | Category | Notes |
|---|---|---|
| N1 | Room-rent excess | Difference between actual and eligible room rent |
| N2 | Proportionate deduction | Reduction of associated expenses because of the room choice (subject to exclusions) |
| N3 | Co-pay | Policy-specified share borne by the insured |
| N4 | Deductible / aggregate deductible | Amount below which the policy does not pay |
| N5 | Sub-limit excess | Amount above the disease or procedure cap |
| N6 | Policy exclusions | Permanent exclusions (e.g., cosmetic) and specific exclusions per wording |
| N7 | Waiting period / PED | Conditions within waiting periods |
| N8 | Reasonable & customary excess | Where the insurer applies this principle |
| N9 | Expenses unrelated to the condition treated | E.g., unrelated investigations |
| N10 | OPD / non-hospitalisation expenses | Unless the policy covers them |
| N11 | Pre/post-hospitalisation outside the window | Outside the policy's day limits |
| N12 | Non-network surcharges / zone co-pays | Policy-specific |
| N13 | Sum insured exhausted | Beyond the available SI/bonus/restore |

### 13.10 Reference data required (business view)

**Phase 1 ownership (D22).** The product team builds and maintains all Phase 1 reference data from public sources (IRDAI, NPPA, published hospital tariffs). There is no domain lead or expert panel in Phase 1. The owner roles below therefore apply from P1.5/P2; Phase 1 owners still need to be named (G-13).

| Reference data | Source | Update cadence | Owner (P1.5+) | Needed from |
|---|---|---|---|---|
| IRDAI non-payable lists I–IV and related guidelines | IRDAI | On circular | Insurance domain lead | P1 (Lists II–IV for unbundling) |
| NPPA ceiling prices (scheduled formulations), device caps | NPPA | Monthly check | Healthcare domain lead | P1 |
| Drug, consumable, procedure and investigation normalisation catalogue | Built internally; public drug databases | Continuous | Healthcare domain lead | P1 |
| Typical quantity ranges per procedure/day (for plausibility) | Expert panel + case data | Quarterly | Healthcare domain lead | P1 Wave 2–3 (no Phase 1 source — G-03) |
| Hospital tariffs (where published) | Hospital displays/websites; user uploads | Continuous | Data ops | P1 |
| Government schedule rates (CGHS, PM-JAY HBP) — context only | Government | On revision | Data ops | P1 (context) |
| Anonymised internal benchmarks | Our cases (consented) | Continuous; published only when statistically valid | Data science | P2 |
| Insurer product wordings (by product/version), CIS | Insurer websites / IRDAI filings / user uploads | Quarterly + on demand | Insurance domain lead | P2 |
| Grievance and claim timelines (GRO, Ombudsman, settlement TATs) | IRDAI, Insurance Ombudsman Rules | On change | Legal/compliance | P2 (hospital-side timelines needed in P1 — G-11) |
| Insurer-specific claim document checklists | Insurers/TPAs | Semi-annual | Ops | P2 |

---

## 14. Non-Functional Requirements

> These are business-level quality attributes. Engineering will translate them into technical specifications.

| ID | Attribute | Requirement |
|---|---|---|
| NFR-01 | Speed — first insight | Summary and top flags within 5 minutes (P90) of upload completion for typed/PDF bills, and within 10 minutes for photographed multi-page bills. |
| NFR-02 | Speed — full report | Full AI report within 15 minutes (P90). Discharge-urgent cases are prioritised in any queue. |
| NFR-03 | Expert SLA *(P1.5)* | Expert reviews delivered by email within a committed turnaround during defined service hours (to be set in P1.5, Q-P1). |
| NFR-04 | Availability | 99.5% monthly for upload, processing and report download; planned maintenance outside peak hours. |
| NFR-05 | Accuracy | Extraction and finding-precision targets as in §8.4. Checks below target are hidden from users (shadow mode). |
| NFR-06 | Explainability | Every finding can be traced to source evidence (page/line), rule ID and reference version. |
| NFR-07 | Reproducibility | Re-running a report with the same inputs and rule versions gives the same findings. |
| NFR-08 | Security | Personal data encrypted at rest and in transit. Strong authentication for internal staff (users have no login). Role-based, least-privilege internal access; full audit logs of every access to personal data; periodic third-party security testing. |
| NFR-09 | Privacy | No user profile data collected (D6). Original uploads deleted after extraction (D8). Only de-identified structured data is retained, with consent asked after the report (D19), and deleted after 3 months (D24). Data minimisation and purpose limitation; no sale of data. |
| NFR-10 | Data residency | Personal and health data stored and processed in India **[recommendation — decision pending, G-14]**. |
| NFR-11 | Incident response | Breach detection and notification processes that meet the DPDP Rules and CERT-In directions, incl. CERT-In's 6-hour reporting requirement for specified incidents **[VALIDATE]**. |
| NFR-12 | Device and network | Works on low/mid-range Android phones and patchy hospital networks; uploads resume within the session; low data usage. |
| NFR-13 | Accessibility | Readable on small screens; large-text options; plain language; screen-reader friendly. Supports users with low digital literacy through assisted flows (a family member can run the check and forward the PDF). |
| NFR-14 | Localisation | Indian number formats (lakh/crore), ₹, IST dates. Content is structured so Hindi and regional languages can be added in P3 (language scope per ON-09). |
| NFR-15 | Scalability | Handles 10× pilot volume without SLA degradation; seasonal peaks (e.g., monsoon disease seasons) are planned for. |
| NFR-16 | Auditability of rules | Rule library changes are reviewed, approved and logged (who, what, why, source). |
| NFR-17 | AI governance | Model outputs are monitored for accuracy drift. Every prompt/model change and every new check is evaluated against the Labelled Test Set before release. No human review of users' audits in Phase 1 (D11). |
| NFR-18 | Content safety | No accusatory language; disclaimers present; templates follow the language guardrails (TS-02, §13.1.5). |
| NFR-19 | Observability | Real-time tracking of processing queues (and expert queues from P1.5), with alerting. |
| NFR-20 | Vendor risk | Any third-party processor of personal data is contracted on data-processing terms. Third-party models are not trained on user data without explicit consent. |

---

# Part IV — Delivery and Governance

## 15. MVP Prioritisation (MoSCoW)

The MVP objective and launch shape are in §10.2.

### 15.1 Must Have
| # | Feature | Requirement refs |
|---|---|---|
| M1 | Anonymous entry (no sign-up, mobile number or verification) with a layered privacy notice and granular consent, incl. relationship to the patient, minors and third-party patient consent | ON-01–ON-09 |
| M2 | Single-session case with auto-inferred details and progress | CM-01–CM-03 |
| M3 | Upload of hospital bills, pharmacy bills, implant invoices/stickers, discharge summary, estimate/package letter and receipts (PDF + photos) | DU-01–DU-03, DU-05–DU-08, DU-10, DU-11 |
| M4 | Summary-vs-itemised detection and itemised-bill request template | DU-08, RE-06(a) |
| M5 | Extraction of header, line items, totals and discharge-summary facts, with confidence and user correction | DP-01–DP-04, DP-07–DP-10 |
| M6 | Normalisation and categorisation of line items, incl. unbundling tags | DP-11–DP-13, DP-15–DP-18 |
| M7 | Cross-document validation and completeness statements; user confirmation of low-confidence fields | DP-19–DP-22 |
| M8 | **Launch set of 18 objective checks live** (D21; "P1 · Launch" in §13.6), DOC checks as document guidance, and Waves 2–3 following as each passes Labelled Test Set testing (§13.8). The audit-engine capabilities AA-03 and AA-04 depend on Wave 3 checks (G-04). | AA-01–AA-06, AA-08, AA-12–AA-14, AA-16, AA-17 |
| M9 | MRP and NPPA ceiling-price / device-cap checks | AA-11 (MRP/NPPA) |
| M10 | Bill Breakdown and hospital-side Who to Ask | AA-15, §13.1.3 |
| M11 | Recommendation engine: explanation, observation, concern, follow-up action; Top 3 actions | RE-01–RE-05, RE-07–RE-09 |
| M12 | Hospital-side templates: itemised bill request, billing query list, implant/package document request, revised bill / explanation / refund request | RE-06(a–d) |
| M13 | Audit Report and one-page Billing Desk Summary; PDF download in the session; regeneration on corrections; limitations and "hospital billing only" scope statement | RP-01–RP-04, RP-06, RP-08 |
| M14 | In-session "Will ask" checklist; hospital escalation ladder in the report | AD-01, AD-06 |
| M15 | Quality measurement against the Labelled Test Set; shadow mode until a check passes (no experts, D11) | KB-04, DP-22, LT-01–LT-11 |
| M16 | Mobile web and desktop web (D18) | — |
| M17 | Session and data storage: no user profile; original files deleted; de-identified structured data stored only with consent; download prompt; session expiry; 3-month deletion | SD-01–SD-08, SD-10 |
| M18 | Finding-level feedback and in-session outcome questions | FB-01, FB-02, FB-04 |
| M19 | Rule library with versioning, sources, on/off and shadow-mode switches; regulatory change process | KB-01–KB-05 |
| M20 | Ops views, role-based access, audit logs, quality dashboard | OP-01–OP-04 |
| M21 | Disclaimers, language guardrails, no ads or referral fees | TS-01, TS-02, TS-04 |
| M22 | Security, privacy, data residency and incident-response NFRs | NFR-08–NFR-11 |

### 15.2 Should Have
| # | Feature | Requirement refs |
|---|---|---|
| S1 | Share into the platform from messaging apps (e.g., a bill forwarded on WhatsApp) | DU-04 |
| S2 | Hospital grievance escalation letter; NPPA complaint pointer | RE-06(e–f) |
| S3 | Wave 2 and Wave 3 checks after launch ("P1 · W2" and "P1 · W3" in §13.6) | KB-04 |
| S4 | Dispute pack inside the downloaded report | AD-05 |
| S5 | Handling of password-protected PDFs | DU-09 |
| S6 | "What to do next time" learnings | RE-10 |
| S7 | Urgent-situation guidance (denial of discharge or treatment) | TS-03 |
| S8 | Anonymous count of interest in the Insurance Check from insured users (sizes P2 demand) | New — no requirement ID yet |
| S9 | Identity-free abuse controls for the free tier | SD-09 |
| — | Implant make/model/batch capture; Labelled Test Set coverage reporting and top-ups | DP-14, LT-12, LT-13 |

### 15.3 Could Have
| # | Feature | Requirement refs |
|---|---|---|
| C1 | Public knowledge content hub aligned with the rule library | KB-06 |
| C2 | Implant identity checks; OT duration and anaesthesia checks | IMP-04, OT-03, OT-04 |
| — | Content moderation guidance for user-edited templates | OP-05 |

### 15.4 Won't Have (this release)
See §10.3 (W1–W22) and §12.17.

---

## 16. Assumptions and Validation Plan

Confirmed decisions are not repeated here; they are in Appendix A (the former A1–A3 duplicated D1, D2 and D5). Targets live in §8.4 and are referenced, not repeated.

| ID | Assumption / hypothesis | Phase | How we test it | Success signal | Status |
|---|---|---|---|---|---|
| A4 / H1 | Users can obtain itemised bills when guided (at least on request) in most private hospitals | P1 | Share of cases with an itemised bill after guidance | ≥ 60% of cases | To validate |
| H2 / A18 | Our bill findings are accurate, and the product team can build accurate rules and a reliable Labelled Test Set from public sources, without domain experts (D22) | P1 | Precision against the Labelled Test Set; user ratings of findings | Precision targets met (§8.4) | To validate |
| H3 | Findings lead to action | P1 | Action intent in the session | Action-intent target met (§8.4) | To validate |
| H4 | Audits find real issues | P1 | Actionable Audit Rate and objective value flagged (D13) | Baselines set in the first month; stable or improving | To validate |
| A7 | Hospitals revise bills in a meaningful share of cases when shown specific, evidence-backed queries | P1 | Outcome tracking — **not available in P1 (G-02)** | To be defined | To validate |
| A10 / A17 | An all-India, English-only launch is acceptable to launch users and enough to reach product-market-fit signals (D9, D17) | P1 | Completion and usefulness by State; drop-off by city; unreadable-line rate | Unreadable-line rate tracked by State (§8.4) | To validate |
| A12 | Family members can lawfully share a patient's documents with appropriate consent mechanisms | P1 | Legal opinion (not before the P1 launch, D27) | — | To validate with counsel |
| A13 | Patients' willingness to share health documents with a new brand can be earned through transparency and privacy guarantees | P1 | Drop-off at the consent step | — | To validate |
| A14 | A bill-only check is valuable enough to pay for, especially for self-pay and reimbursement patients | P1 / P1.5 | CSAT by payment mode (P1); conversion (P1.5) | — | To validate |
| A15 / H7 | Insured users who try the bill check will want the Insurance Check | P1 | Anonymous interest count among insured users | ≥ 30% of insured users join **[HYPOTHESIS]** (wording, G-06) | To validate |
| A16 / H9 | Users can complete the check in one sitting and will download the report before leaving | P1 | Session completion and report download rates | Report-download target met (§8.4) **[HYPOTHESIS]** | To validate |
| H6 | Users recommend the product | P1 | NPS; referral share | NPS and referral targets met (§8.4) | To validate |
| A6 / H5 | Users will pay for Expert Review at the moment of need | P1.5 | Price tests; conversion rates | To be set in P1.5 | To validate |
| A11 | Qualified billing/claims experts (ex-TPA, ex-hospital billing) can be recruited to a flexible panel at viable cost | P1.5 | Recruitment pilot | — | To validate |
| A5 | A material share of insured hospitalisations involve contestable patient payables | P2 | Share of cases with ≥ 1 contestable finding; value distribution | — | To validate |
| A8 | Insurers revise deductions on evidence-based GRO complaints in a meaningful share of cases | P2 | Outcome tracking | — | To validate |
| A9 | Public IRDAI lists and policy wordings give enough structure for reliable policy checks | P2 | Domain-expert review of 100 settlements | — | Likely; to validate |
| H8 | B2B pull exists (for P3) | Before P3 | 10 conversations each with employers, brokers and claim consultants | ≥ 2 LOIs for P3 pilots | To validate |

---

## 17. Constraints

| # | Constraint | Implication |
|---|---|---|
| C1 | **Regulatory perimeter:** no sale or solicitation of insurance; no representation; no legal advice; no clinical opinions by AI | Product scope and copy (§19) |
| C2 | **Data protection:** DPDP Act 2023 and Rules 2025 (phased enforcement to May 2027), plus the existing IT Act SPDI rules for sensitive personal data including medical records | Consent design, minors, retention, breach handling |
| C3 | **Input quality:** photos, summary bills, multi-format documents | Robust capture guidance; graceful partial audits |
| C4 | **No universal price standard** for private hospitals (§6.3) | Price findings framed as questions; benchmarks only when statistically valid |
| C5 | **Time pressure at discharge** (cashless final authorisation within ~3 hours; §6.2) | Speed NFRs; interim-bill checks |
| C6 | **Variation across insurers, products and versions** (P2) | Policy library; fallback to user-provided wording |
| C7 | **Variation across States** in clinical-establishment regulation | Hospital-side escalation guidance must be State-aware (full coverage P2) |
| C8 | **Team:** product team only, with no domain hire or consultant (D22) | Rules and Labelled Test Set built from public sources; launch with the most objective checks |
| C9 | **Expert availability** outside business hours *(P1.5)* | Defined service hours; clear SLA communication |
| C10 | **Non-interference:** the platform must not encourage users to withhold payment in ways that block discharge or treatment | Guidance to pay under protest where appropriate and pursue later **[VALIDATE wording with counsel]** (G-12) |
| C11 | **No identity or contact details; single session** (D6, D7) | No reminders, notifications, family sharing, cross-session Stay Mode or later outcome capture; Expert Review by email only, from P1.5 |
| C12 | **No legal or tax review before the Phase 1 launch** (D27) | Consent flows, de-identification, retention, minors and template wording rest on founder decisions; risk accepted (RK28) |

---

## 18. Risks

Scoring: Likelihood (L) and Impact (I) on 1–5; Score = L × I. RK25 is resolved and has been removed (Appendix A, D10).

| # | Risk | L | I | Score | Mitigation | Owner |
|---|---|---|---|---|---|---|
| RK1 | **Low B2C frequency → high CAC relative to LTV** | 4 | 5 | 20 | Event-triggered acquisition; referral and word-of-mouth loops; Policy Health Check hook (P2); B2B2C in P3; measure contribution per case early | CEO / Growth |
| RK2 | **False positives damage user credibility** (a user challenges a correct charge and is embarrassed or harmed) | 4 | 4 | 16 | Finding classes and confidence; checks go live in waves only after passing Labelled Test Set testing; shadow mode; neutral language | Product |
| RK3 | **Defamation or legal action by hospitals** over "overcharging" claims (reports, marketing, any public benchmark naming hospitals) | 3 | 5 | 15 | No accusatory language; no public hospital rankings; disclaimers; reports are private to the user (see also RK28) | Product / CEO |
| RK4 | **Regulatory: IRDAI treats the activity as claims intermediation**, or Advocates Act issues with legal-style help. Exposure is lower in the MVP (no insurer-facing features) and rises with the Insurance Check. | 2 | 5 | 10 | No representation; no success fee on insurance recoveries; self-help framing; legal opinion before P1.5 (§19.6) | Legal / CEO |
| RK5 | **DPDP non-compliance:** third-party data (family uploading patient data), minors' data, deceased patients, consent withdrawal | 3 | 5 | 15 | Consent flows ON-03–ON-07; de-identification (D14); 3-month retention (D24) (see also RK28) | Product / CEO |
| RK6 | **Breach of health data** | 2 | 5 | 10 | Security NFRs; minimisation; audits; incident playbook | Eng / Security |
| RK7 | **AI hallucination:** invented line items, wrong amounts, wrong regulatory references | 3 | 5 | 15 | Only evidence-linked findings; arithmetic re-verification; references only from the curated library; user confirmation of low-confidence fields; "Complex" label above the value threshold | Eng / Product |
| RK8 | **Itemised bills cannot be obtained** → shallow audits → poor perceived value | 3 | 4 | 12 | Request templates; partial audits with clear limits; insurer-side documents as an alternative source (P2) | Product |
| RK9 | **Low realised savings:** most patient payables turn out to be legitimate | 3 | 4 | 12 | Value also from understanding and prevention; honest messaging; track Verified Patient Value once possible; reposition if needed | Product / CEO |
| RK10 | **Front-line staff at hospitals or insurers ignore queries** | 3 | 3 | 9 | Professional, evidence-based templates; escalation ladder; expert-drafted letters (P1.5) | Product |
| RK11 | **Regulatory change** (IRDAI circulars, NPPA updates) makes rules stale | 4 | 3 | 12 | Rule versioning; regulatory-monitoring SLA (KB-03); named rule owner (the product team in P1) | Domain |
| RK12 | **Expert supply or quality varies** *(P1.5)* | 3 | 3 | 9 | Credential checks; QA sampling; calibration sessions; scorecards | Ops |
| RK13 | **Competitive response:** brokers or employer platforms add AI audits | 3 | 3 | 9 | Neutrality; data moat; speed of execution; partner rather than compete in B2B2C | CEO |
| RK14 | **Perceived conflict of interest** if insurers or hospitals become customers | 2 | 4 | 8 | Patient-side charter; disclosure; patient-aligned B2B buyers first (§6.7) | CEO |
| RK15 | **Users withhold payment, delaying discharge** and harming the patient | 2 | 5 | 10 | Guidance prioritises patient welfare; "pay under protest and pursue" scripts (no requirement yet, G-12); urgent support (TS-03) | Product / Legal |
| RK16 | **Misuse:** users submit altered bills to inflate claims (the platform is used to optimise fraudulent claims) | 2 | 4 | 8 | Terms of use; no guidance that encourages misrepresentation; integrity indicators for internal review (DOC-09, P2) | Product / Legal |
| RK17 | **Benchmark bias:** early data skewed towards complainers or certain hospitals | 3 | 3 | 9 | Statistical thresholds; disclose sample sizes; no benchmarks until valid | Data |
| RK18 | **The bill-only MVP looks like free checkers** (eVaakil and others offer free bill checks) → weak differentiation and low willingness to pay | 4 | 4 | 16 | Win on depth, objective checks, discharge-summary consistency and the Billing Desk Summary (§5.4); Expert Review from P1.5; keep the free bill check competitive with free tools (G-01) | Product / CEO |
| RK19 | **Cashless insured users see less value:** the insurer/TPA also scrutinises the bill, and the patient pays only part of it | 3 | 3 | 9 | Explain that overcharging still uses up the sum insured and drives deductions; target self-pay and reimbursement users first; Insurance Check in P2 | Product |
| RK20 | **P2 slips**, delaying the main long-term differentiator (three-way reconciliation) | 3 | 4 | 12 | Set a target date for P2 before the MVP launch (not yet set — G-08); validate insurance rules (Q-D1–Q-D4, Q-D6) during the MVP; consented research cohort for P2 data | Product / Domain |
| RK21 | **Lost reports:** the user closes the session before downloading and cannot get the report back | 3 | 3 | 9 | Prominent download prompt; PDF generated automatically before results are shown in full; clear warning (SD-07); session timeout (G-10) | Product |
| RK22 | **Measurement blind spot:** without return visits, we cannot prove bills were corrected | 4 | 4 | 16 | P1 North Star based on findings, not savings (D13); in-session outcome questions only (D12); Verified Patient Value returns when outcomes can be tracked (see G-02) | Product / CEO |
| RK23 | **Hindi on bills:** lines in Hindi or regional scripts are skipped, so some bills are only partly audited; worse with an all-India launch (D17) | 4 | 3 | 12 | List unread lines clearly (DP-08); track unreadable-line rate by State; bring Hindi forward if needed | Product |
| RK24 | **Minors' bills:** a parent's declaration may not meet DPDP's verifiable-consent requirement | 3 | 4 | 12 | Founder decision to allow on declaration (D15); revisit with legal advice | CEO |
| RK26 | **Abuse of an anonymous free tier** (bulk uploads, scraping, fake documents) | 3 | 2 | 6 | Identity-free abuse controls (SD-09; see G-09) | Eng |
| RK27 | **Domain-knowledge gap:** rules and labels built without billing expertise may be wrong in ways the team can't see (D22) | 4 | 4 | 16 | Launch with the most objective checks only; record every rule's public source; user ratings flag disputed findings; revisit a domain hire at P1.5 | Product / CEO |
| RK28 | **Launching without legal or tax review:** consent, minors, de-identification, retention, the GST check and template wording are unreviewed (D27) | 3 | 5 | 15 | Accepted by the founder; conservative wording; GST check (CAL-05) held back to Wave 2; legal review recommended before P1.5 | CEO |
| RK29 | **3-month retention limits learning:** benchmarks and quality trends rely on a rolling 3-month window (D24) | 3 | 2 | 6 | P2 to decide whether aggregate statistics (no bill-level rows) may be kept longer (Q-P12). Score may be understated (G-05). | Product |
| RK30 | **Skewed test set:** network bills over-represent a few cities and large hospitals, so accuracy may not hold elsewhere in India (D17) | 3 | 4 | 12 | Synthetic bills for missing formats; recruit beta testers across States and hospital types; coverage reporting (LT-12) | Product |

> Owner roles "Domain", "Legal" and "Data" do not exist in the Phase 1 team (D22). See G-13.

### 18.1 Hidden risks for leadership attention
1. **The deceased-patient case.** A significant share of high-value bills involve patients who died in hospital. Bereaved families are both highly motivated and highly vulnerable. Consent, tone and support need special handling (DPDP nomination provisions; empathy-driven flows). See Persona 5 and ON-06.
2. **Medico-legal entanglement.** Billing questions can surface suspected medical negligence. The platform must not drift into assessing negligence; it should signpost users to appropriate legal help.
3. **Insurer data-sharing incentives.** If we later partner with any insurer, users may fear their data will affect future underwriting. We recommend a public data charter: "we never share your data with insurers for underwriting" (G-19).
4. **Reliance on a single model or vendor** for document understanding creates cost and continuity risk. Procurement should keep alternatives viable (G-19).
5. **Brand weaponisation on social media.** Users may post our reports publicly to shame hospitals. Terms and report watermarks should discourage misuse and make clear the report is a private, non-conclusive analysis (G-19).

---

## 19. Compliance and Regulatory Considerations

> This section identifies issues for legal counsel; it is not legal advice. Every item needs confirmation by Indian counsel specialising in insurance, health and data-protection law.
>
> **Phase 1 status (D27):** the founder has decided to launch Phase 1 without legal or tax review and accepts the risk (C12, RK28). The checklist in §19.6 is recommended before P1.5, when payments and email addresses are introduced.

### 19.1 Insurance regulation (IRDAI)

> The MVP has no insurer-facing features, so most items below become active with the Insurance Check (P2). The MVP must still avoid marketing that implies claim assistance or insurance advice.

| Topic | Consideration | Product stance |
|---|---|---|
| Insurance intermediation | Soliciting or selling insurance, or acting as an intermediary, requires IRDAI registration (broker, corporate agent, web aggregator, insurance marketing firm). | Do **not** sell, recommend specific products or earn commissions. Informational "consider add-ons" content stays generic. |
| Claims assistance for a fee | IRDAI has cautioned the public against unregistered entities offering claim-settlement services for fees. **[VALIDATE scope of the caution and its applicability to audit/self-help services]** | Fixed-fee audit and self-help templates; no representation; no contact with insurers on the user's behalf; no success fee on insurance recoveries pending a legal opinion. |
| Surveyors and loss assessors | A licensed activity for certain classes; health claims are typically handled by TPAs/insurers. | Avoid any description implying we "assess" or "settle" claims. |
| Use of regulations in content | IRDAI circulars and master circulars must be cited accurately. | Reference library with versioning; legal review. |
| Grievance channels | Insurer GRO, Bima Bharosa, Insurance Ombudsman Rules 2017 (as amended). | Accurate, up-to-date guidance; never imply we file on the user's behalf. |

### 19.2 Legal services
| Topic | Consideration | Product stance |
|---|---|---|
| Practice of law | The Advocates Act 1961 restricts the practice of law to enrolled advocates. Bar Council of India rules prohibit advocates from soliciting work and from contingency-fee arrangements. | Templates framed as consumer self-help; no legal opinions. Any future advocate panel needs a compliant structure (e.g., independent advocates, no fee-sharing, no advertising of advocates). |
| Consumer protection | Consumer Protection Act 2019; Consumer Protection (E-Commerce) Rules 2020; CCPA guidelines on misleading advertisements and dark patterns (2023). | Truthful marketing (no guaranteed-savings claims); no dark patterns in free-to-paid conversion; clear refund policy; a grievance officer for our own service. |

### 19.3 Health and pricing regulation
| Topic | Consideration | Product stance |
|---|---|---|
| Clinical Establishments Act 2010 and Rules 2012; State laws | Display of rates; charging within government-determined ranges (not yet notified); applicability varies by State. | Use as the basis for "request tariff/itemisation" guidance; do not assert breaches of rate ranges that have not been notified. |
| Drug and device pricing | DPCO 2013 / NPPA ceiling prices; MRP rules under the Legal Metrology (Packaged Commodities) Rules. | Objective MRP and ceiling-price checks; guide users to NPPA grievance channels for unresolved cases. |
| Medical advice | Telemedicine Practice Guidelines (2020) and NMC rules apply if doctors give opinions. | AI gives no clinical opinions. Any doctor-expert opinion (P1.5) is framed as a billing–clinical consistency review, within professional rules. |
| Patients' rights | MoHFW/NHRC Patients' Rights Charter (advisory), e.g., the right to information on rates and itemised bills. **[VALIDATE status]** | Referenced in user guidance where appropriate. |

### 19.4 Data protection and security
| Topic | Consideration | Product stance |
|---|---|---|
| DPDP Act 2023 and DPDP Rules 2025 | Notice; consent; purpose limitation; data principal rights; breach notification to the Data Protection Board and individuals; children's data (verifiable parental consent); nomination; retention and erasure. Substantive obligations enforceable from 13 May 2027; consent managers from 13 Nov 2026. **[VALIDATE]** | Build to the DPDP standard from day one, not from the enforcement date. |
| Significant Data Fiduciary | The Government may notify entities that process large volumes of sensitive data as SDFs (DPIA, audits, DPO in India). | Design as if SDF obligations may apply at scale; appoint a DPO early (owner: G-13). |
| IT Act 2000 and SPDI Rules 2011 | Medical records and health information are "sensitive personal data"; reasonable security practices (e.g., ISO 27001-type controls) are required. | Comply until superseded; obtain written consent. |
| CERT-In Directions (2022) | Mandatory reporting of specified cyber incidents within 6 hours; log retention. **[VALIDATE]** | Incident response plan. |
| Anonymous, single-session use (D6–D8) | Bills still contain the patient's personal and health data, so DPDP obligations apply during processing. Whether stored de-identified data falls outside DPDP depends on how well identifiers are removed. Deletion rights are hard to honour once data is no longer linked to a person. | Consent before upload; identifiers stripped before storage (SD-05); counsel to confirm. |
| Third-party and minors' data | Family members uploading patient data; parents uploading minors' data. | Consent flows ON-04 to ON-06. Minors allowed on declaration (D15), an accepted risk (RK24). |
| AI-specific | Government AI advisories and governance guidelines are still evolving. | Human oversight; transparency about AI use; accuracy monitoring. |
| ABDM / NHCX (future) | Health Data Management Policy and consent artefacts if integrating with ABDM; NHCX participation rules. | P4 consideration. |

### 19.5 Tax and commercial (from P1.5)
- GST registration and invoicing for paid services.
- Payment aggregator compliance, through a licensed PA.
- Expert panel contracts: confidentiality, data-processing terms, conflict-of-interest declarations.

### 19.6 Pre-launch compliance checklist (business level; recommended before P1.5)
1. Legal opinion on the IRDAI perimeter, the Advocates Act and success-fee viability.
2. Privacy notice, consent flows and DPIA reviewed by counsel.
3. Terms of use, disclaimers and all templates legally reviewed.
4. A policy for substantiating marketing claims.
5. Security assessment and incident response plan.
6. Expert panel contracts and a credential verification process.
7. An approved data retention schedule.

---

## 20. Open Questions

These are questions for the founder or leadership unless stated otherwise. Each shows the working default used in this draft. Resolved questions are recorded against their decision in Appendix A. Issues found in this review are listed separately in **CS.5 (G-01–G-23)** pending triage.

### 20.1 Business and strategy
| ID | Question | Why it matters | Working default |
|---|---|---|---|
| Q-B2 | Price points and free tier after Phase 1? (Phase 1 resolved: free, D25) | Funnel design, revenue model | Open for P1.5 (G-01) |
| Q-B3 | Will we ever accept insurers/TPAs or hospitals as customers? | Brand neutrality, conflict of interest | Not in P1–P2; patient-aligned B2B only |
| Q-B4 | Is a success-fee / contingency model wanted long term? | Requires a legal structure; affects positioning | Not in the MVP; legal opinion first |
| Q-B7 | Final product name and brand positioning? (Codename "Project RS" until launch, D23) | Marketing, trust | Open |
| Q-B8 | Which B2B channel to validate first in P3: employers, brokers or claim consultants? | Roadmap | Claim consultants (fastest to adopt; supply side for experts) |
| Q-B9 | How should the Insurance Check be priced: inside the Audit Report, as an add-on, or as a higher tier? | Revenue model; upgrade path for MVP users | Higher tier that includes the bill check **[HYPOTHESIS]** |
| Q-B10 | What is the target date for the Insurance Check? | Prevents the roadmap stalling after a bill-only MVP (§6.9) | Deferred to P2 (see G-08) |

### 20.2 Market and users
| ID | Question | Why it matters | Working default |
|---|---|---|---|
| Q-M4 | Should we serve government-scheme beneficiaries (CGHS/PM-JAY) in the MVP? | Different rules | No (P3) |

### 20.3 Domain and regulatory (for domain experts and counsel)
| ID | Question | Owner | Status |
|---|---|---|---|
| Q-D1 | Confirm the current IRDAI position on proportionate-deduction exclusions (pharmacy, implants, diagnostics, ICU), and how it applies to policies issued before and after the guideline dates. | Insurance expert | P2 (deferred) |
| Q-D2 | Confirm the current IRDAI non-payable Lists I–IV and how insurers must treat List II–IV items in settlements. | Insurance expert | P2 (deferred) |
| Q-D3 | Confirm regulatory timelines: cashless authorisation, reimbursement settlement, interest on delay, GRO response window, Ombudsman eligibility and limits. | Insurance expert / Counsel | P2 (deferred) |
| Q-D4 | Can a network hospital collect from the patient the difference between its tariff and the insurer-agreed tariff for payable items? | Insurance expert | P2 (deferred) |
| Q-D5 | What is the current GST treatment of hospital room rent and other healthcare services? | Tax advisor | Open; accepted risk (D27) |
| Q-D6 | Does a fixed-fee claims audit and self-help service fall within any IRDAI registration requirement? | Counsel | P2 (deferred) |
| Q-D7 | What are the lawful basis and consent mechanics for family members uploading an adult patient's records, for deceased patients and for minors? | Counsel | Open; accepted risk (D27) |
| Q-D8 | Can doctors on the expert panel give billing–clinical consistency opinions without creating a doctor–patient relationship or teleconsultation obligations? | Counsel / Medical advisor | P1.5 |
| Q-D9 | Which States' clinical establishment rules give patients enforceable rights to itemised bills and rate display? | Healthcare expert / Counsel | Open; accepted risk (D27) |

### 20.4 Product and operations
| ID | Question | Working default |
|---|---|---|
| Q-P1 | Expert service hours and turnaround commitments | P1.5 |
| Q-P2 | The value threshold above which a finding is marked "Complex" (P1) or recommended for Expert Review (P1.5) | To be set from usage data |
| Q-P11 | What is kept while an Expert Review is open (original files, email address), and when is it deleted? | Deferred to P1.5 |
| Q-P12 | May aggregate statistics (no bill-level rows) be kept beyond 3 months for benchmarks? | Deferred to P2 (see G-05) |

---

# Appendices

## Appendix A — Founder Decision Log

| # | Topic | Decision | Implication | Resolves |
|---|---|---|---|---|
| D1 | Business model | **B2C first, B2B later** | The MVP is designed for individuals and families. B2B (employers, brokers, claim consultants) is a P3 line, but the MVP data and consent model must not block it. | — |
| D2 | Service depth | **AI audit + optional human expert review** | Self-serve AI audit by default, with paid escalation to a human billing/claims expert. The platform does **not** represent users before insurers or courts. The expert part moves to P1.5 (D10). | — |
| D3 | MVP journey moments | ~~At discharge; reimbursement claim preparation; post-settlement deductions~~ — *superseded by D5* | Pre-admission estimate review stays deferred (now P3). | — |
| D4 | Output format | Markdown in the repository | This file | — |
| D5 | MVP scope (3 Oct 2026) | **MVP = Hospital Bill Check**: the user uploads the bill the hospital hands over and checks it for overcharging. **Insurance checks move to P2.** | Policy checks, settlement reconciliation, Reimbursement Readiness and Settlement Deduction Review become the Insurance Check. B2B moves to P3. Self-pay patients become a primary MVP segment. | Q-M2 |
| D6 | Identity (4 Oct 2026) | **Fully anonymous use**: no mobile number, no verification, no sign-up; **no user profile information saved** | No accounts, saved profiles, notifications or reminders. Consent is still required (§12.1). | — |
| D7 | Sessions (4 Oct 2026) | **Single session only**: upload, review and download in one sitting; a case cannot be reopened | No Stay Mode across days, no family sharing, no follow-up reminders, no later outcome capture | Q-M3 |
| D8 | Storage (4 Oct 2026) | **Structured data only**: extracted bill data is stored; original files are not kept | Experts and quality reviewers cannot see original bills after the session (§12.2) | — |
| D9 | Language (4 Oct 2026) | **English only** for the MVP; all Hindi removed, including reading Hindi text on bills | Bills partly printed in Hindi or regional scripts are only partly audited; languages move to P3 | Q-M1 |
| D10 | Expert review | **Moved to a new Phase 1.5.** The user gives an **email address only**, and only if they want a review. The expert replies **manually by email with a PDF** and may correspond manually. Phone numbers deferred to P2. | Phase 1 collects no contact details at all. Resolves risk RK25 (expert review cannot be delivered to an anonymous, single-session user). | Q-P6 |
| D11 | Internal experts | **No experts in Phase 1**, not even for internal quality review | Accuracy is measured only against the Labelled Test Set | Q-P8 |
| D12 | Outcome measurement | **In-session questions only** for the MVP | Money actually saved cannot be measured in Phase 1 | Q-P7 |
| D13 | MVP North Star | **Share of completed audits with ≥ 1 actionable finding**; supporting measures: median ₹ value of "Likely error" findings flagged per audit, and action intent | Verified Patient Value returns as the North Star when outcomes can be tracked | — |
| D14 | De-identification | Strip name, UHID/IP number, phone, address and policy number. Keep age band, gender, hospital, city, admission month, length of stay and line items. | SD-05 | Q-P9 |
| D15 | Minors' bills | **Allowed on the uploader's declaration** of being parent or guardian | Legal risk accepted (RK24) | Q-P10 |
| D16 | IRDAI lists | Used as an **industry reference for all patients**, always phrased as a question | Unbundling checks apply to self-pay bills too | Q-M6 |
| D17 | Launch geography | **All-India from day one** | More bills with Hindi/regional lines that Phase 1 can't read (RK23) | Q-B1 |
| D18 | Channel | **Mobile web + desktop web**; no app in Phase 1 | — | Q-P5 |
| D19 | Storage consent | Asked **at the end of the session, after the report**; nothing is stored if declined or unanswered | — | Q-P4 |
| D20 | Cashless patients | **Served, not targeted**; the report states that insurance is not assessed | — | Q-M5 |
| D21 | Timeline | **Launch ~3–4 months after the team is in place**, with **~15–20 objective checks live** (the launch set has 18); the rest follow after launch | Launch set in §13.6 / §13.8 | Q-B5 |
| D22 | Team | **Product team only**, building rules and the Labelled Test Set from public sources | Domain-knowledge risk (RK27) | Q-B6 |
| D23 | Name | **"Project RS"** until launch | Final name still open (Q-B7) | Q-B7 (partly) |
| D24 | Retention | De-identified structured data **deleted after 3 months** | Limits long-term benchmarks (RK29) | Q-P3 |
| D25 | Pricing | **Phase 1 is completely free**; payments arrive with P1.5 | PY module moves to P1.5 | Q-B2 (Phase 1) |
| D26 | Personas | Rewritten for the bill-only scope; **bereaved family** persona and **billing desk staff** stakeholder added | §9 | — |
| D27 | Legal review | **Phase 1 launches without legal or tax review**; risk accepted by the founder | Q-D5, Q-D7 and Q-D9 stay open as accepted risks (RK28) | — |
| D28 | Labelled Test Set — sources | Start with **consented bills from the team's personal network** and **synthetic bills with planted errors**; then a **closed pre-launch beta** in which testers explicitly agree their original bills are kept for testing | The beta is the only exception to D8, and only for testers (§12.14) | Q-P13 |
| D29 | Labelled Test Set — labelling | **Real bills** (network and beta) are labelled **independently by two team members**, who reconcile differences against public sources. **Synthetic bills** use their planted answers. | Labels stay independent of the AI being tested | Q-P13 |
| D30 | Labelled Test Set — retention | Identifiers **blacked out on receipt**; redacted originals and labels **kept for the life of the test set** (e.g., through P2); testers can **withdraw 