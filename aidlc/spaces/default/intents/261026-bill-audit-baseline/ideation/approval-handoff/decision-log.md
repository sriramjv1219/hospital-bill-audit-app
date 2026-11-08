# Decision Log — Hospital Bill Check (Project RS, Phase 1), Ideation

This is the record of every decision made during ideation, in the order it was made. Each entry names the questions file where the decision is recorded in full. The summary of these decisions is in `initiative-brief.md`.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- Questions files under `ideation/`:
  - `intent-capture/intent-capture-questions.md` (IC);
  - `market-research/market-research-questions.md` (MR);
  - `feasibility/feasibility-questions.md` (FE);
  - `scope-definition/scope-definition-questions.md` (SD);
  - `rough-mockups/rough-mockups-questions.md` (RM);
  - `approval-handoff/approval-handoff-questions.md` in this folder (AH).
- The artifacts these decisions shaped:
  - `intent-statement.md` and `stakeholder-map.md` (intent capture);
  - `competitive-analysis.md` (market research);
  - `feasibility-assessment.md` and `constraint-register.md` (feasibility);
  - `scope-document.md` and `intent-backlog.md` (scope definition);
  - `wireframes.md` (rough mockups).
- There is no team assessment, because team formation was skipped.

How to read the source column: "IC Q12" means question 12 in the intent-capture questions file.

## 1. Intent capture

| ID | Decision | Source |
|---|---|---|
| DL-01 | The problem is the PRD's Phase 1 framing: patients and families cannot tell whether a hospital bill is correct and justified, and lack the knowledge, time and evidence to question it | IC Q1 |
| DL-02 | Target customers are the PRD's Phase 1 set. The five primary customer groups are targeted. Seniors are supported through a family member, cashless patients are served but not targeted, and billing desk staff are a stakeholder, not users | IC Q2 |
| DL-03 | All six customer pains are in: unreadable or summary-only bills; no time at discharge; not knowing whom to ask or how to escalate; fear and stress; managing remotely; not knowing whether a price is reasonable | IC Q3 |
| DL-04 | Success uses the PRD's Phase 1 metric tree and guardrails as written, as hypotheses to recalibrate after the first 500 completed audits | IC Q4 |
| DL-05 | The trigger is a market gap | IC Q5 |
| DL-06 | You build this on behalf of the founder or a client organisation | IC Q6 |
| DL-07 | Stakeholders are the PRD's audience list plus the billing desk executive. Domain experts and counsel are not part of the Phase 1 team | IC Q7 |
| DL-08 | The founder decides scope and priority. Product, engineering and design influence. Nobody else has decision authority | IC Q8, Q22 |
| DL-09 | A weekly status update goes to the founder or leadership | IC Q9 |
| DL-10 | The PRD's working defaults and suggested resolutions are treated as decisions. Where a suggestion offers a choice, that item is resolved at the stage that first needs it | IC Q10, Q15 |
| DL-11 | There is no fixed timeline. The PRD's 3–4-month launch estimate (D21) does not apply | IC Q11, Q17 |
| DL-12 | The product boundary is the Phase 1 Hospital Bill Check MVP with its internal modules and the P0 tooling: rule library, Labelled Test Set, ops/admin. Phase 1.5 and later phases are out | IC Q12 |
| DL-13 | The workflow runs the enterprise process but stops before the operation phase | IC Q13, Q18 |
| DL-14 | Not reading the end of the PRD (the rest of D30, the glossary, sources and version history) is accepted | IC Q14 |
| DL-15 | The metric tree stays as written. The bill-correction measure (PRD G-02) is left for the founder to decide | IC Q16 |
| DL-16 | Executives want progress on showing that the check is accurate, understood and acted on. Business and Growth, and investors, are kept informed only, the investors through the founder | IC Q19, Q20, Q21 |

## 2. Market research

| ID | Decision | Source |
|---|---|---|
| DL-17 | The PRD's competitive section is the baseline. The direct competitors were re-checked on the web | MR Q1 |
| DL-18 | Direct competition is free online bill checkers only. Grievance firms, in-hospital claim desks, brokers and employer platforms are indirect. Hospital and insurer software and government bodies are context only | MR Q2 |
| DL-19 | No market-size figures at this stage; only the inputs still needed | MR Q3 |
| DL-20 | There is no first-hand customer evidence yet, so every customer-expectation claim is labelled a hypothesis | MR Q4 |
| DL-21 | Differentiators are only the PRD's five points. "Who to Ask" is a product feature, not a differentiator | MR Q5, Q11 |
| DL-22 | The build-vs-buy decision on bill reading is left open, with criteria set. Data residency is left for the founder | MR Q6, Q7 |
| DL-23 | Build-vs-buy covers bill reading only | MR Q8 |
| DL-24 | The trends covered are price regulation and the itemised-bill rules | MR Q9 |
| DL-25 | The US analogues (Goodbill, Resolve) are left out | MR Q10 |

## 3. Feasibility

| ID | Decision | Source |
|---|---|---|
| DL-26 | You build alone with this AI-assisted workflow | FE Q1 |
| DL-27 | The existing Next.js / React / TypeScript web app is the starting point | FE Q2 |
| DL-28 | Hosting is not decided. Your answer: "Mostly Vercel. But I would like to decide at a later point." Vercel is the working assumption only | FE Q3 |
| DL-29 | Whether a cloud account exists is not known | FE Q4 |
| DL-30 | No bill-reading decision yet. A trial runs on synthetic bills first and is repeated on real bills before the decision is final | FE Q5, Q12 |
| DL-31 | There are no real bills yet, so the Labelled Test Set starts with synthetic bills | FE Q6 |
| DL-32 | There is no running-cost budget; cost per audit is estimated and flagged | FE Q7 |
| DL-33 | Plan for under 1,000 audits a month at first | FE Q8 |
| DL-34 | Internal tools need their own strong sign-in, because no company sign-in exists | FE Q9 |
| DL-35 | No organisational blockers are known | FE Q10 |
| DL-36 | You and the founder are the two independent labellers of real bills | FE Q11 |

## 4. Scope definition

| ID | Decision | Source |
|---|---|---|
| DL-37 | Every Must Have and Should Have is in this build. Could Have items are deferred to a later build | SD Q1 |
| DL-38 | The first release follows the PRD's launch shape: all Must Haves, the 18-check launch set and the document checks as guidance | SD Q2 |
| DL-39 | Typical-quantity data: search for a public source first, and derive ranges from the Labelled Test Set only if none is found (PRD G-03) | SD Q3 |
| DL-40 | The two Must Haves that depend on Wave 3 checks are built for launch. Their checks appear only once they pass (PRD G-04) | SD Q4 |
| DL-41 | Work is ordered value first. The bill-reading trial starts first, alongside the features that do not depend on its result | SD Q5, Q8 |
| DL-42 | Internal tools are built in the leanest form that meets each requirement | SD Q6 |
| DL-43 | Urgent-situation and pay-under-protest guidance move to Phase 1.5. Your answer: "Move this scenario to phase 1.5". The rule against encouraging payment withholding that blocks discharge (C10) still applies | SD Q7 |
| DL-44 | PRD gaps with a single suggested resolution are applied as written: G-06, G-11, G-13, G-16, G-20, G-22 | `scope-document.md`, section 4 |

## 5. Rough mockups

| ID | Decision | Source |
|---|---|---|
| DL-45 | No brand yet: a plain, neutral look with the codename "Project RS" as a placeholder | RM Q1 |
| DL-46 | Phone and desktop are designed equally from the start | RM Q2 |
| DL-47 | Screens meet WCAG 2.1 level AA | RM Q3 |
| DL-48 | User-facing screens only; each internal tool is described in a short paragraph | RM Q4 |
| DL-49 | The mockups are simple text sketches inside the documents | RM Q5 |
| DL-50 | The rough mockups were approved as they were. The eight design-check points (R-01 to R-08) were accepted as open, each with an agreed change that is not yet applied | Rough mockups approval, 1 November 2026 |

## 6. Approval and handoff

| ID | Decision | Source |
|---|---|---|
| DL-51 | All eight agreed rough-mockup changes (R-01 to R-08) carry forward into Requirements Analysis and Refined Mockups; `initiative-brief.md` lists them | AH Q1 |
| DL-52 | The founder has already agreed to the intent and scope | AH Q2 |
| DL-53 | The main risks are accepted with the responses already planned | AH Q3 |
| DL-54 | No running-cost budget for now. The trial records cost per audit, and a ceiling is proposed to the founder once hosting is chosen | AH Q4 |
| DL-55 | The market research is enough to proceed; claims still to check stay open | AH Q5 |
| DL-56 | The team plan is confirmed: you build, the founder decides and labels, and there are no domain experts or legal review | AH Q6 |
| DL-57 | Data residency, hosting and the bill-correction measure (PRD G-02) are all decided before the build starts. This sets a date for DL-15 and DL-28 | AH Q7 |
| DL-58 | Go: proceed to detailed planning, with feasibility conditions F1–F5 tracked as gates on the build and the launch | AH Q8 |

## Assumptions & Open Questions

Decisions still open, each with when it is due:

| Item | Owner | Due |
|---|---|---|
| Data residency, India only or India first (PRD G-14) | Founder | Before the build starts (DL-57) |
| Hosting, confirmed against residency | You and the founder | Before the build starts (DL-57) |
| How to measure whether billing desks correct bills (PRD G-02) | Founder | Before the build starts (DL-57) |
| Bill-reading option (build, buy or partner) | You | After the trial on real bills (DL-30) |
| Running-cost ceiling | Founder | Once hosting is chosen (DL-54) |
| Session timeout (G-10), low-confidence collapse threshold, minimum examples per check (G-16) | You | Requirements Analysis |
| Gaps whose suggestion offers a choice: G-09, G-17, G-18, G-19, G-21 | You | At the stage that first needs each (DL-10) |
