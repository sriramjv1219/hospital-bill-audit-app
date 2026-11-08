# Competitive Analysis — Hospital Bill Check (Project RS, Phase 1)

This analysis covers the market the Phase 1 Hospital Bill Check enters, as framed in the approved intent statement (`intent-statement.md`, in `ideation/intent-capture/`). Its starting point is the competitive section of the PRD (`PRDs/app-baseline.md` §5). The direct competitors were re-checked on the web on 27 October 2026 [Q1].

## Sources

- [Q1]–[Q11]: answers in `market-research-questions.md` in this folder.
- PRD §n: `PRDs/app-baseline.md`, v0.14 (PRD-v2), 4 October 2026. Its competitor information is "from public sources as of October 2026" (PRD §5).
- Web sources, all accessed 27 October 2026, are linked where they are used and listed again at the end.
- Labels: **[VALIDATE]** means the claim comes from a secondary source and must be checked against a primary one. **Hypothesis** means a belief about customers that has not been tested with real customers yet [Q4].

## 1. How the competition is grouped

The MVP competes mainly with free bill checkers until the Insurance Check ships (PRD §5.4). This analysis groups the competitors that way [Q2].

| Group | Categories (PRD §5.1) | How they are treated here |
|---|---|---|
| **Direct** | A. Free online bill checkers | Re-checked on the web and compared feature by feature [Q1] [Q2] |
| **Indirect** | B. Claim grievance and dispute services; C. In-hospital claim facilitation; D. Broker and advisory claim support; E. Employer health-benefit platforms | Summarised from the PRD, not re-checked [Q1] [Q2] |
| **Context only** | F. Hospital revenue-cycle software; G. Insurer fraud analytics; H. Government grievance bodies and infrastructure | Listed only so the landscape is complete [Q2] |

US analogues (Goodbill, Resolve; PRD §5.1 category I) are left out of this analysis [Q10].

## 2. Direct competitors: free online bill checkers

### 2.1 What the web re-check found

| Competitor | What was confirmed on 27 Oct 2026 | What could not be confirmed | Status |
|---|---|---|---|
| **eVaakil Medical Bill Auditor** ([page](https://evaakil.com/indian-medical-bill-checker-audit-tool-detect-overcharging-irdai-non-payables/), dated 2 Feb 2026) | Compares an itemised bill with "standard market benchmarks" and the IRDAI list of non-payable items. Flags unbundling, for example gloves and masks or admission kits billed separately. Estimates proportionate deductions caused by room-rent capping. Drafts a query to the hospital billing desk before the bill is settled. Says it is general information, not legal advice. | Whether it is free (the PRD says it is), how bills are uploaded, file types, sign-up, human review, languages, and what the report looks like | Live. The page is the same one the PRD cites (PRD Appendix C). |
| **CleanBill** ([Devfolio project](https://devfolio.co/projects/cleanbill-e762), created 16 Nov 2025) | A hackathon project. The user scans or uploads a hospital bill; it compares the bill with government rate lists (PM-JAY, CGHS, NPPA) and flags overpricing, duplicates and suspicious entries in a "transparency report". | Any live service or public address | Concept only; not a live competitor today |
| **Other free Indian bill checkers** | None found by a standard web search | Whether others exist; the search returned mostly US tools and student projects **[VALIDATE]** | Gap in evidence |

US-focused free tools turned up in the search (for example MedBillAI, which offers a free first scan built on US billing codes). They are not usable on Indian bills, and US analogues are out of scope [Q10].

**What this means.** The direct field is still thin: one live, lightly documented checker (eVaakil) and at least one concept (CleanBill). CleanBill's idea of comparing bills with CGHS and PM-JAY rates also shows a positioning risk. The PRD is explicit that those rates are **not** legal caps for a private, self-paying or privately insured patient (PRD §6.3). A competitor that presents them as caps could set false expectations that our evidence-first reports then have to correct.

### 2.2 Feature comparison (direct competition)

Ratings: Strong / Adequate / Weak / Absent / Unknown. Our column is the Phase 1 target as the PRD defines it, not a built product.

| Capability | eVaakil | CleanBill (concept) | **Our product, Phase 1 target** |
|---|---|---|---|
| Upload bill | Unknown (not shown on the page) | Adequate (scan or upload, as described) | Strong: photos and PDFs, no sign-up, WhatsApp-friendly (PRD §5.4) |
| Line-by-line extraction and normalisation | Unknown | Unknown | Strong: every line extracted and normalised (PRD §5.4) |
| IRDAI non-payable and unbundling flags | Adequate | Absent | Adequate: IRDAI lists used as an industry reference for all patients, always phrased as a question (PRD D16) |
| Room-rent cap impact | Adequate | Absent | Absent in Phase 1: policy-aware checks are Phase 2 (PRD §5.3) |
| Objective findings: arithmetic, duplicates, MRP and NPPA ceiling-price breaches, room days vs length of stay | Unknown | Weak (duplicates only, as described) | Strong (PRD §5.4) |
| Evidence and citation for every finding, with a confidence level | Unknown | Unknown | Strong (PRD §5.4) |
| Query to the billing desk | Adequate (drafts a query) | Absent | Strong: Billing Desk Summary, templates and escalation ladder (PRD §5.4) |
| Useful before payment, including on interim bills | Adequate ("before you settle") | Unknown | Strong: interim bills checked one session at a time (PRD §5.4, §11.2) |
| Comparison with government rate lists | Unknown ("market benchmarks") | Strong as a claim, but those lists are not caps for private patients (PRD §6.3) | Context only (PRD §13.10) |
| Neutrality (no hospital or insurer money) | Weak: positioned as lead generation for legal services (PRD §5.2) | Unknown | Strong: "patient-side, always" (PRD §6.7) |
| Price | Free (PRD §5.2) | Unknown | Free in Phase 1 (PRD D25) |

## 3. Table stakes and differentiators

| Type | Items | Source |
|---|---|---|
| **Table stakes**: what free checkers already do, so customers will expect it | Uploading a bill; flags for IRDAI non-payables and unbundling; room-rent cap impact; query templates for the billing desk | [Q5], PRD §5.2 |
| **Differentiators**: where the MVP must be clearly better | 1. **Depth**: every line extracted and normalised, not just category totals or generic flags. 2. **Objective findings**: arithmetic, duplicates, MRP and NPPA ceiling-price breaches, room days vs length of stay, which a billing desk cannot easily dismiss. 3. **Evidence and citations**: every finding links to the bill line and the supporting record, with a confidence level. 4. **Follow-through**: Billing Desk Summary, templates and a hospital escalation ladder. 5. **Usefulness before payment**, including on interim bills. | [Q5], PRD §5.4 |

The differentiation brief contains only these five points [Q5]. "Who to Ask" (classifying each rupee by whom to challenge) is a product feature here, not a market differentiator, and is left out of the brief [Q11].

**A gap to watch.** Room-rent cap impact is table stakes because the free checker offers it, but the Phase 1 MVP does not offer it: policy-aware checks are Phase 2 (PRD §5.3). Insured users who compare the two tools may see the MVP as missing something. This matches PRD risk RK18 (the bill-only MVP looks like free checkers).

## 4. Indirect competitors (from the PRD, not re-checked)

These solve neighbouring problems. The summary follows PRD §5.1–5.2 and was not refreshed [Q1] [Q2].

| Category | Examples (PRD §5.1) | Why it is indirect | Overlap with Phase 1 |
|---|---|---|---|
| B. Claim grievance and dispute services | Insurance Samadhan, Bima Samadhan, local claim consultants | Work after an insurer rejects a claim; selective intake (reported about 3–4 of 100 cases); no audit of the hospital bill | Low: they handle the insurer side, after the fact |
| C. In-hospital claim facilitation | ClaimBuddy (250+ partner hospitals; $5M Series A, April 2024) **[VALIDATE]** | Paid by hospitals, so little incentive to challenge hospital billing; partner hospitals only | Medium: present at discharge, where our customers are |
| D. Broker and advisory claim support | Ditto, Beshak, Policybazaar, Turtlemint, Jio Insurance Brokers | Only for their own policyholders; focused on the insurer | Low for self-pay patients |
| E. Employer health-benefit platforms | Plum, Loop, Onsurity; TPA apps such as Medi Assist | Servicing group policies, not auditing hospital bills | Low in Phase 1 |

## 5. Context only

Hospital revenue-cycle software (for example Care.fi) works for hospitals to maximise collections. Insurer fraud analytics are insurer-aligned, and patients never see their results. Government bodies (Bima Bharosa, the Insurance Ombudsman, NHCX, PM-JAY grievance) are free and authoritative but slow, cover the insurer side only, and give no help preparing evidence (PRD §5.1–5.2) [Q2]. None of them competes for the patient who wants to check a hospital bill.

## 6. SWOT for the Phase 1 MVP

| | Helpful | Harmful |
|---|---|---|
| **Internal** | **Strengths**: depth of line-by-line extraction; objective findings; evidence for every finding; patient-side neutrality; free; no sign-up (PRD §5.4, D25) | **Weaknesses**: no policy-aware checks such as room-rent cap impact until Phase 2 (PRD §5.3); English only, so lines in Hindi or regional scripts are only partly audited (PRD D9); no experts in Phase 1 (PRD D11); a single session, so nothing carries over between interim bills (PRD D7) |
| **External** | **Opportunities**: a thin direct field (section 2.1); new rules for itemised hospital bills, including a national bill-format standard and Karnataka's requirement from 1 November 2026 (see `market-trends.md`); continuing public pressure on hospital pricing (see `market-trends.md`) | **Threats**: free checkers can add features quickly; competitors presenting CGHS and PM-JAY rates as caps set false expectations (PRD §6.3); hospital backlash if findings read as accusations (PRD §6.3, RK3); bills that are only summaries limit what any audit can do (PRD §6.5) |

How to act on it:

- **Strengths + Opportunities:** lead with the evidence-first, objective findings. Use the itemised-bill rules to coach users to ask for the itemised bill (PRD §6.5, DU-08).
- **Weaknesses + Threats:** say plainly that insurance is not assessed in Phase 1 (PRD D20). Keep the language guardrails strict (PRD §13.1.5).

## 7. Competitive forces (Porter's five forces, Phase 1)

| Force | Rating | Why |
|---|---|---|
| Threat of new entrants | High | Simple free checkers are cheap to launch, as a hackathon concept shows (section 2.1) |
| Supplier power | Medium | Reading bills depends on third-party or in-house document-reading capability; that choice is still open (see `build-vs-buy.md`) |
| Buyer power | High | Phase 1 is free and has no sign-up, so users can switch at no cost (PRD D6, D25) |
| Threat of substitutes | High | The family asks the billing desk directly, asks for an itemised bill, or goes to a consumer commission, NPPA or a lawyer; asks friends in healthcare or online forums (PRD §3.4–3.5) |
| Rivalry | Low to medium | Few live direct competitors were found (section 2.1) **[VALIDATE]** |

Overall, the market is easy to enter and costs users nothing to leave, so the MVP has to win on quality and trust rather than on being first or cheapest. This is consistent with the PRD's own conclusion (PRD §6.9).

## 8. Positioning (unchanged from the PRD)

"**The patient's billing expert in your pocket**": not a legal service, not an insurance seller, not a hospital partner. Lead with clarity and evidence rather than outrage, and never call a hospital fraudulent (PRD §5.4, §6.3).

## 9. What customers expect: hypotheses to test

None of these has been tested with customers yet [Q4].

| Hypothesis | Research that would confirm or reject it |
|---|---|
| H-M1. Families value a finding more when it shows the evidence (bill line and record) than when it only shows a flag | Usability test comparing an evidence-led report with a flag list; Finding usefulness rating (intent success metrics) |
| H-M2. Families will use the check before paying, not only afterwards | Share of sessions started before payment; interviews with families at discharge |
| H-M3. Billing desk staff take a neutral, evidence-led summary more seriously than an accusatory one | Interviews with billing desk staff, the key stakeholder in `intent-statement.md` |
| H-M4. Insured users notice that room-rent cap impact is missing compared with the free checker | Comparison test with insured users; "Action intent" metric by insurance status |
| H-M5. Users trust a free tool that is not selling legal services more than one that is | Trust question at the end of the session; Net Promoter Score |

## Assumptions & Open Questions

- Whether eVaakil is still free, and how it takes uploads and shows results, could not be confirmed from its page **[VALIDATE]**.
- A standard web search may miss Indian free checkers that are not well indexed; a deeper search is worth doing before any investor use **[VALIDATE]**.
- The indirect competitor facts (funding, partner counts, intake rates) are the PRD's and were not refreshed [Q1].

## Web sources (accessed 27 October 2026)

- eVaakil Medical Bill Auditor: https://evaakil.com/indian-medical-bill-checker-audit-tool-detect-overcharging-irdai-non-payables/
- CleanBill (Devfolio): https://devfolio.co/projects/cleanbill-e762
