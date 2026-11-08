# Constraint Register — Hospital Bill Check (Project RS, Phase 1)

This register lists the fixed boundaries the Phase 1 build must work within. It is grouped by type and draws on:

- the approved intent statement (`intent-statement.md`);
- the market research (`competitive-analysis.md`, `market-trends.md`, `build-vs-buy.md`);
- the PRD's constraints (§17) and compliance considerations (§19);
- this stage's answers.

The assessment that uses these constraints is in `feasibility-assessment.md`.

## Sources

- [Q1]–[Q12]: answers in `feasibility-questions.md` in this folder.
- PRD §n / C-n / D-n / NFR-n: `PRDs/app-baseline.md`, v0.14 (PRD-v2).
- **Hard** means non-negotiable for this build. **Soft** means a current choice that can change through a later decision.

## Regulatory and compliance constraints

| ID | Constraint | Hard / Soft | Source | Implication for the build |
|---|---|---|---|---|
| CR-R1 | Build to the DPDP Act 2023 and Rules 2025 from day one, not from the May 2027 enforcement date | Hard | PRD §19.4, C2 | Notice, consent before upload, purpose limitation, data rights, breach notification and children's data must all be designed in from the start |
| CR-R2 | Medical records are sensitive personal data under the IT Act SPDI Rules 2011; reasonable security practices are required | Hard | PRD §19.4, C2 | Security controls are needed from launch |
| CR-R3 | Specified cyber incidents must be reported to CERT-In within 6 hours **[VALIDATE]** | Hard | PRD §19.4, NFR-11 | A written incident plan that a solo builder can carry out |
| CR-R4 | Regulatory perimeter: no sale or solicitation of insurance, no representation, no legal advice, no clinical opinion from AI | Hard | PRD C1 | Product copy and templates stay self-help; findings are observations, never medical judgement |
| CR-R5 | Do not encourage users to withhold payment in ways that block discharge or treatment | Hard | PRD C10 | Guidance favours "pay under protest and pursue later" (wording not yet reviewed, PRD G-12) |
| CR-R6 | No accusatory language; never call a hospital fraudulent; price outliers are questions | Hard | PRD §6.3, NFR-18 | Language guardrails apply to every finding, template and report |
| CR-R7 | No legal or tax review before the Phase 1 launch (accepted risk) | Hard (founder decision) | PRD D27, C12 | Consent flows, de-identification, minors and templates rest on founder decisions |
| CR-R8 | Minors' bills allowed on a parent's declaration | Soft (founder decision, accepted risk) | PRD D15, RK24 | Revisit if legal advice changes |
| CR-R9 | Data residency is **not yet decided**: India only or India first | Open (founder) | PRD G-14, NFR-10; market research Q7 | Blocks the final hosting and bill-reading supplier choices (`feasibility-assessment.md`, section 4) |

## Data handling constraints

| ID | Constraint | Hard / Soft | Source | Implication for the build |
|---|---|---|---|---|
| CR-D1 | Fully anonymous use: no sign-up, no mobile number, no user profile | Hard | PRD D6, C11 | No accounts, reminders or notifications for users |
| CR-D2 | Single session: upload, review and download in one sitting; a case cannot be reopened | Hard | PRD D7, C11 | The report must be downloadable before the session ends (PRD RK21) |
| CR-D3 | Original uploads are deleted after extraction; only structured data is stored | Hard | PRD D8 | Nothing downstream can rely on the original image after the session |
| CR-D4 | Identifiers (name, hospital ID number, phone, address, policy number) are stripped before storage | Hard | PRD D14, SD-05 | De-identification runs before anything is stored |
| CR-D5 | Stored de-identified data is deleted after 3 months | Hard | PRD D24, SD-10 | Automatic deletion from launch |
| CR-D6 | Labelled Test Set: identifiers blacked out on receipt; redacted originals and labels kept for the life of the set; testers can withdraw | Hard | PRD D30 | The test set lives under different retention rules from product data |
| CR-D7 | Third-party AI models are not trained on user data without explicit consent; processors are contracted on data-processing terms | Hard | PRD NFR-20 | A pass/fail condition for any bill-reading supplier (`build-vs-buy.md`) |

## Technical constraints

| ID | Constraint | Hard / Soft | Source | Implication for the build |
|---|---|---|---|---|
| CR-T1 | Extraction accuracy of at least 95% on digital PDFs and at least 90% on good photos, measured on the Labelled Test Set | Hard (target) | `intent-statement.md`; PRD §8.4 | The bill-reading trial must measure this (F1 in `feasibility-assessment.md`) |
| CR-T2 | First insight within 5 minutes and the full report within 15 minutes, both at the 90th percentile; 10 minutes to first insight for photographed multi-page bills | Hard (target) | PRD NFR-01, NFR-02 | Bill-reading speed must be measured in the trial |
| CR-T3 | 99.5% monthly availability for upload, processing and report download | Hard (target) | PRD NFR-04 | Hosting choice must support it |
| CR-T4 | Works on low and mid-range Android phones and patchy hospital networks; uploads resume within the session | Hard | PRD NFR-12 | Upload and report must be light and resumable |
| CR-T5 | English only; lines in Hindi or regional scripts are listed as unread | Hard (Phase 1) | PRD D9, DP-08 | Unreadable lines are reported, never guessed |
| CR-T6 | Checks below their precision target stay hidden (shadow mode); every model or prompt change is evaluated on the Labelled Test Set before release | Hard | PRD NFR-05, NFR-17 | Release of any check is gated on the test set |
| CR-T7 | Every finding traces to its source (page and line), rule ID and reference version; re-running with the same inputs gives the same findings | Hard | PRD NFR-06, NFR-07 | Rule and reference data are versioned |
| CR-T8 | Avoid relying on a single model or vendor for bill reading | Soft (recommendation) | PRD §18.1 hidden risk 4 | A second option is kept usable (`build-vs-buy.md`) |
| CR-T9 | Build on the existing Next.js / React / TypeScript web app | Soft (current choice) | [Q2] | A standard base; detailed design comes later |
| CR-T10 | Hosting leans towards Vercel, but the decision is deferred: "Mostly Vercel. But I would like to decide at a later point." | Soft (leaning) | [Q3] | Long-running processing must fit the platform's request time limits; residency must be checked (`feasibility-assessment.md`, section 4) |
| CR-T11 | Internal tools need their own strong sign-in; there is no company sign-in to reuse | Hard | [Q9]; PRD NFR-08 | Extra build and security work for internal tools |
| CR-T12 | Handle 10 times pilot volume without missing speed targets; initial volume is under 1,000 audits a month | Hard (target) | PRD NFR-15; [Q8] | Capacity is planned for about 10,000 audits a month |

## Organisational constraints

| ID | Constraint | Hard / Soft | Source | Implication for the build |
|---|---|---|---|---|
| CR-O1 | One builder, working alone with this AI-assisted workflow | Hard (current) | [Q1] | All design, build, testing, security and operations work rests on one person |
| CR-O2 | Product team only: no domain hire or consultant in Phase 1; rules and the test set are built from public sources | Hard | PRD D22, C8 | Launch with the most objective checks (PRD C8, RK27) |
| CR-O3 | No experts in Phase 1, not even for internal quality review | Hard | PRD D11 | Accuracy is measured only against the Labelled Test Set |
| CR-O4 | Real bills are labelled independently by two people: you and the founder | Hard | PRD D29; [Q11] | Labelling throughput depends on both people's time |
| CR-O5 | No fixed timeline | Soft | `intent-statement.md` | Pace, not deadline, is the risk |
| CR-O6 | No running-cost budget is set | Soft | [Q7] | Cost per audit is estimated and flagged, not capped |
| CR-O7 | The founder decides; status goes weekly to the founder | Hard | `intent-statement.md` | Open founder decisions (residency, hosting) go through the weekly update |
| CR-O8 | No organisational blockers known | — | [Q10] | — |

## Market constraints

| ID | Constraint | Source | Implication for the build |
|---|---|---|---|
| CR-M1 | Phase 1 is free, so every audit is a cost with no revenue | PRD D25 | Cost per audit matters as volume grows (`feasibility-assessment.md`, section 5) |
| CR-M2 | All-India launch from day one | PRD D17 | Bills vary by State and include regional scripts (PRD RK23, RK30) |
| CR-M3 | No legal price benchmark for private patients; CGHS and PM-JAY rates are context only | PRD §6.3, C4; `market-trends.md` §2.3 | Price outliers are framed as questions |
| CR-M4 | NPPA ceiling prices change by notification; the knee-implant cap is due for decision in November 2026 | `market-trends.md` §2.2 | Reference data is versioned and dated from launch |

## Assumptions & Open Questions

- CR-R9 (data residency) and CR-T10 (hosting) are open and linked. They are tracked as dependencies in `raid-log.md`.
- The CERT-In 6-hour rule and the DPDP details are as the PRD states them; this stage did not re-check them **[VALIDATE]**.
