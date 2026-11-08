# Intent Capture & Framing — Questions

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.

These questions are shaped by the PRD at `PRDs/app-baseline.md` ("Indian Medical Bill Audit Platform — codename Project RS", v0.14 / PRD-v2, 4 October 2026). The options quote or summarise what the PRD says so you can confirm, narrow or correct it. Nothing from the PRD goes into the intent statement unless you confirm it here.

Note: the PRD is about 211,000 characters. It was read through the project's document library, which stops at 200,000 characters, so reading ended partway through founder decision D30 in Appendix A. The glossary (Appendix B), sources (Appendix C) and version history (Appendix D) were not read.

---

## Q1. What business problem is this build solving?

The PRD frames four sub-problems: P1 bill opacity, P2 policy opacity, P3 settlement opacity and P4 the action gap (not knowing whom to ask, what to say, or the deadlines). It addresses P1 and the hospital side of P4 in Phase 1, and P2, P3 and the insurer side of P4 in Phase 2. Which framing should this build's intent use?

A. Phase 1 framing: patients and families cannot tell whether a hospital bill is correct and justified, and lack the knowledge, time and evidence to question it with the hospital (P1 bill opacity + P4 action gap, hospital side)
B. Full PRD framing: the Phase 1 problem plus policy and settlement opacity with insurers (P1–P4, insurer side included)
C. A narrower problem (please describe under X)
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q2. Who is the target customer for this build?

The PRD names five primary Phase 1 personas (self-paying patient, family member at discharge, remote NRI family manager, patient paying upfront for later reimbursement, bereaved family), one supported persona (senior citizen, usually helped by family), and the hospital billing desk executive as a key non-user stakeholder. Cashless insured patients are "served, not targeted" (D20).

A. The PRD's Phase 1 set: the five primary personas, seniors supported through a family member, cashless patients served but not targeted, and billing desk staff as the key stakeholder whose acceptance decides outcomes
B. Self-paying patients and their families only
C. All hospital patients equally, insured (cashless and reimbursement) and uninsured
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q3. Which customer pains matter most for this build? (select all that apply)

These come from the PRD's pain-point map (§3.6) for Phase 1.

A. Cannot read the bill, or only has a summary bill instead of an itemised one (PP1)
B. No time to review at discharge; pressure to pay and leave (PP2)
C. Does not know whom to complain to, what to say or how to escalate on the hospital side (PP7)
D. Fear and emotional stress, especially for families of very ill or deceased patients (PP8)
E. Managing the hospitalisation remotely, e.g. NRI children working from forwarded photos (PP9)
F. Not knowing whether a price is reasonable (PP5; the PRD treats this as a question, never an accusation)
G. Not yet defined
X. Other (please specify)

[Answer]: A, B, C, D, E, F

## Q4. What does success look like for this build?

The PRD's Phase 1 metric tree (§8.3–8.5) uses the North Star "Actionable Audit Rate" (share of completed audits with at least one "Likely error" or "Worth asking" finding) plus initial targets such as: audit completion ≥ 60%; report download ≥ 70%; first insight ≤ 5 minutes and full report ≤ 15 minutes (P90); line-item extraction accuracy ≥ 95% on digital PDFs and ≥ 90% on good photos; "Likely error" precision ≥ 90% and "Worth asking" precision ≥ 75% against the Labelled Test Set; action intent ≥ 40%; CSAT ≥ 80%; NPS ≥ 40; zero personal-data incidents. Money actually saved is not measured in Phase 1.

A. Adopt the PRD's Phase 1 metric tree and guardrails as written, as initial hypotheses to recalibrate after the first 500 completed audits
B. Adopt the PRD metrics and add a measure of whether billing desks actually correct bills (the gap the PRD flags as G-02)
C. For this build, focus success on launch-readiness only: quality (extraction accuracy, finding precision) and speed targets
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q5. Why now? What is triggering this initiative? (select all that apply)

The PRD (§4.2) cites several forces.

A. Regulatory momentum towards transparency: IRDAI Master Circular on Health Insurance (May 2024), Supreme Court attention to hospital rate standardisation, NPPA price caps, NHCX
B. DPDP Act 2023 and Rules 2025 raising the bar for, and legitimising, consented health-data handling
C. Generative AI document understanding making line-item extraction from photographed bills affordable at consumer price points
D. A market gap: existing options are generic free checkers, post-rejection grievance firms, or broker/employer desks that do not audit the hospital
E. A founder-led business opportunity to build a patient-side brand ("patient-side, always")
F. Not yet defined
X. Other (please specify)

[Answer]: D

## Q6. What is your role in this build?

This tells me whose priorities the stakeholder map should centre on.

A. I am the founder / CEO of the product
B. I am building this on behalf of the founder or a client organisation
C. I am part of the product or engineering team working under the founder
D. Not specified
X. Other (please specify)

[Answer]: B

## Q7. Who are the key stakeholders, and what does each care about?

The PRD's audience list is: CEO and executives; Product; Engineering; Design; Business and Growth; domain experts and counsel (for [VALIDATE] items); investors. It also names the hospital billing desk executive as a key external stakeholder. Founder decision D22 says Phase 1 has a product team only, with no domain hire or consultant.

A. Use the PRD's audience list plus the billing desk executive, noting that domain experts and counsel are not part of the Phase 1 team (D22)
B. Founder/CEO and the product team only (product, engineering, design); others are informed later
C. A different set (please list under X)
D. Not identified
X. Other (please specify)

[Answer]: A

## Q8. Who decides scope and priority, and who influences those decisions?

The PRD records 30 founder decisions (D1–D30) and lists its open questions as "questions for the founder or leadership".

A. The founder / CEO decides; the product team, engineering and design influence
B. A product owner decides day to day; the founder approves major changes
C. A client sponsor decides; the founder and product team influence
D. Not yet defined
X. Other (please specify)

[Answer]: A

## Q9. Are there communication requirements or a reporting cadence?

A. No separate cadence; the approval points in this workflow are the review points
B. A regular status update to the founder / leadership (please give the frequency under X if it is not weekly)
C. A stakeholder review of the PRD (to v1.0) is needed before the build goes further
D. Not yet defined
X. Other (please specify)

[Answer]: B (weekly status update to the founder / leadership)

## Q10. How should this build treat the PRD's unresolved items?

The PRD lists 23 review findings still awaiting a decision (CS.5, G-01 to G-23; five rated High, e.g. G-01 the free/paid boundary after Phase 1, G-04 Must Haves that depend on Wave 3 checks) and open questions in §20 (e.g. Q-B10 the Insurance Check target date, Q-P2 the "Complex" value threshold).

A. Carry them as open items and resolve each one at the stage that first needs it (scope, requirements, design)
B. Resolve the High-priority findings (G-01 to G-05) during ideation, before requirements
C. Treat the PRD's working defaults and suggested resolutions as decisions for this build, unless you say otherwise
D. Not applicable
X. Other (please specify)

[Answer]: C

## Q11. Is there a timeline expectation for this build?

Founder decision D21: launch about 3–4 months after the team is in place, with about 15–20 objective checks live (the launch set has 18); Waves 2–3 follow after launch.

A. Follow D21: launch about 3–4 months after the team is in place, with the 18-check launch set
B. A fixed target date (please give it under X)
C. No fixed timeline for this build
D. Not yet defined
X. Other (please specify)

[Answer]: C

## Q12. What product boundary should this build cover?

The PRD phases the product as P0 Foundations (rule library v0 and Labelled Test Set, before the build), P1 Hospital Bill Check (the MVP), P1.5 Expert Review, P2 Insurance Check, P3 Scale and B2B2C, P4 Ecosystem. Phase 1 also includes internal modules: knowledge base and rule library (KB), operations and admin (OP), and the Labelled Test Set (LT).

A. Phase 1 Hospital Bill Check MVP, including its internal modules and the P0 tooling it needs (rule library, Labelled Test Set, ops/admin); later phases out of scope
B. Phase 1 user-facing journey only; rule library and test-set tooling handled outside this build
C. Phase 1 plus Phase 1.5 (Expert Review, payments, email-only contact)
D. Phase 1 through Phase 2 (adds the Insurance Check)
E. Not yet defined
X. Other (please specify)

[Answer]: A

## Q13. Does the workflow-selected scope match how you want to run this build?

This workflow was started with the `enterprise` scope: all 33 stages from ideation through deployment and operations, with comprehensive depth (about 8–12 questions per stage, detailed documents). This is separate from the product boundary in Q12.

A. Confirm `enterprise`: run the full lifecycle at comprehensive depth for the boundary chosen in Q12
B. Keep `enterprise` but stop before the operation phase (deployment, observability, etc.) for now
C. Switch to a lighter process (e.g. `mvp` or `feature`, standard depth)
D. Not yet defined
X. Other (please specify)

[Answer]: B

## Q14. Is it acceptable that the end of the PRD was not read?

The reader stopped partway through founder decision D30 (Labelled Test Set retention) in Appendix A. The glossary, sources and version history were not read.

A. Yes, that is fine for intent capture; continue
B. I'll provide the remaining appendices as a separate file before requirements analysis
C. Not applicable
X. Other (please specify)

[Answer]: A

## Q15. Follow-up to Q10: what if the PRD's suggested resolution is itself a choice?

You chose to treat the PRD's working defaults and suggested resolutions as decisions (Q10, answer C). Several review findings suggest alternatives rather than one answer. For example, G-03 (no source for "typical quantity" ranges) suggests naming a public source, OR deriving ranges from the Labelled Test Set, OR moving those checks to Phase 1.5. G-02, G-05 and G-08 are similar.

A. Where the suggestion lists alternatives, resolve that item at the stage that first needs it; everything else uses the PRD defaults
B. Where the suggestion lists alternatives, take the first alternative listed
C. I'll decide each one now (please list under X)
D. Not applicable
X. Other (please specify)

[Answer]: A

## Q16. Follow-up: Q4 and Q10 point different ways on G-02

In Q4 you chose the PRD's metric tree as written (answer A), which has no measure of whether billing desks correct bills. In Q10 you chose to adopt the PRD's suggested resolutions (answer C), and G-02's suggestion is to add exactly such a measure. Which should hold?

A. Keep the metric tree as written; G-02 stays an open gap for the founder to decide
B. Add a bill-correction measure as G-02 suggests; the method (e.g. an in-session question for already-paid bills, or a follow-up study with billing desks) is decided during requirements
C. Not yet defined
X. Other (please specify)

[Answer]: A

## Q17. Follow-up: Q11 and founder decision D21

You chose no fixed timeline for this build (Q11, answer C). The PRD records founder decision D21: launch about 3–4 months after the team is in place. You also said the founder decides scope and priority (Q8, answer A). How should D21 be treated?

A. D21 stays the founder's launch goal, but this build is not committed to a date
B. D21 does not apply to this build
C. Not yet defined
X. Other (please specify)

[Answer]: B

## Q18. Follow-up to Q13: when should the operation phase come out of the plan?

You chose to keep the enterprise process but stop before the operation phase (Q13, answer B). The plan currently still includes the seven operation stages (deployment pipeline, environment provisioning, deployment execution, observability, incident response, performance validation, feedback and optimisation). Removing them is a plan change you approve separately.

A. Right after this stage is approved, I propose the exact change to the plan for your approval
B. Keep them in the plan for now and decide when we reach the operation phase
C. Not yet defined
X. Other (please specify)

[Answer]: A

## Q19. Follow-up: what do executives want from this build?

The stakeholder map lists executives but not their interest.

A. Progress against the Phase 1 objective: showing that the free bill check is accurate, understood and acted on
B. To be kept informed only; no specific interest of their own
C. Not identified
X. Other (please specify)

[Answer]: A

## Q20. Follow-up: what do Business and Growth want from this build?

A. Acquisition and growth: reaching families at the moment of need and tracking the growth measures in the metric tree (referral share, NPS)
B. To be kept informed only; no specific interest of their own
C. Not identified
X. Other (please specify)

[Answer]: B

## Q21. Follow-up: what do investors want from this build?

A. Evidence of product-market fit for the bill check, and a credible path to the later phases
B. To be kept informed through the founder only; no direct interest in this build
C. Not identified
X. Other (please specify)

[Answer]: B

## Q22. Follow-up: does anyone besides the founder have decision authority?

You said the founder / CEO decides scope and priority, and product, engineering and design influence (Q8).

A. No one else: executives, Business and Growth, domain experts and counsel, and investors are informed or consulted only
B. Domain experts and counsel can hold back content that relies on facts marked [VALIDATE] until they confirm it
C. Executives share decision authority with the founder
D. Not identified
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Problem (Q1): patients and families cannot tell whether a hospital bill is correct and justified, and lack the knowledge, time and evidence to question it with the hospital (Phase 1 framing).
- Target customer (Q2): the PRD's Phase 1 set: self-paying patient, family member at discharge, remote NRI family manager, patient paying upfront, bereaved family; seniors supported through family; cashless patients served but not targeted; billing desk staff as the key stakeholder.
- Pains (Q3): can't read the bill or only has a summary bill; no time at discharge; doesn't know whom to ask or how to escalate; fear and emotional stress; managing remotely; not knowing whether a price is reasonable.
- Success (Q4, Q16): the PRD's Phase 1 metric tree and guardrails as written, as initial hypotheses recalibrated after the first 500 completed audits; G-02 (no bill-correction measure) stays an open gap for the founder.
- Why now (Q5): the market gap left by generic free checkers, post-rejection grievance firms, and broker/employer desks that do not audit the hospital.
- Your role (Q6): building on behalf of the founder or a client organisation.
- Stakeholders (Q7): the PRD audience list (CEO and executives, Product, Engineering, Design, Business and Growth, domain experts and counsel, investors) plus the hospital billing desk executive; domain experts and counsel are not in the Phase 1 team.
- Decision rights (Q8, Q22): the founder / CEO decides; product, engineering and design influence; executives, Business and Growth, domain experts and counsel, and investors are informed or consulted only.
- Stakeholder interests (Q19–Q21): executives want progress on showing the free bill check is accurate, understood and acted on; Business and Growth are kept informed only; investors are kept informed through the founder only.
- Communication (Q9): a weekly status update to the founder / leadership.
- PRD open items (Q10, Q15): use the PRD's working defaults and suggested resolutions; where a suggestion lists alternatives, resolve it at the stage that first needs it.
- Timeline (Q11, Q17): no fixed timeline; founder decision D21 does not apply to this build.
- Product boundary (Q12): the Phase 1 Hospital Bill Check MVP including its internal modules and the P0 tooling it needs (rule library, Labelled Test Set, ops/admin); Phase 1.5 onwards is out of scope.
- Process (Q13, Q18): keep the enterprise process but stop before the operation phase; right after this stage is approved, propose the exact plan change for approval.
- PRD coverage (Q14): the unread end of the PRD (rest of D30, glossary, sources, version history) is acceptable for intent capture.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct

## Assumption Confirmation

The stakeholder map leaves these items open because your answers named the groups but not what they want or what they can decide:

- [assumption] What executives want from this build is not yet known.
- [assumption] What Business and Growth want from this build is not yet known.
- [assumption] What investors want from this build is not yet known.
- [assumption] Whether executives, Business and Growth, domain experts and counsel, or investors have any decision authority over scope or priority is not yet known.

A. Accept assumptions
B. Convert to follow-up questions

[Answer]: B. Convert to follow-up questions
