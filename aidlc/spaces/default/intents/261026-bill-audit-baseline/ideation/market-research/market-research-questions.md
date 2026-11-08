# Market Research — Questions

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- The approved intent statement at `ideation/intent-capture/intent-statement.md`: Phase 1 Hospital Bill Check MVP only, primary customers are self-pay patients and families, the trigger is the gap left by generic free bill checkers, grievance firms and broker/employer claim desks, and the PRD's working defaults and suggested resolutions are used as decisions.

The PRD (`PRDs/app-baseline.md`) already contains a market and competitive section: ecosystem participants and "why now" forces (§4.1–4.2), a market-sizing approach with no published TAM (§4.3), nine competitor categories with strengths, weaknesses and a feature-gap table (§5.1–5.3), differentiation and positioning (§5.4), and strategic trade-offs for a bill-only MVP (§6). These questions do not ask you to repeat that material. They ask only about what this stage has to decide on top of it.

Already settled, so not asked again:

- Launch geography is all-India from day one (founder decision D17).
- Phase 1 is completely free (D25). Pricing after Phase 1 is outside this build's boundary (intent statement, Q12).
- The product team builds all Phase 1 reference data (IRDAI lists, NPPA ceiling prices, drug and procedure catalogue, published hospital tariffs) from public sources (D22).
- The brand stance is "patient-side, always": no hospital or insurer money (PRD §5.4, §6.7).

---

## Q1. How fresh does the competitor evidence need to be?

PRD §5 says its competitor information comes from public sources as of October 2026 and should be refreshed before investor use. Some players publish no pricing. The ideation guardrails require every market claim to carry a citation or a named source.

A. Use PRD §5 as the baseline, and re-check the direct Phase 1 competitors (free online bill checkers such as eVaakil, plus any new free checker found) on the web now, with links and dates
B. Use PRD §5 as written; cite the PRD as the source and do no new web research
C. Run fresh web research across all nine competitor categories in PRD §5.1, with links and dates
D. Not sure yet
X. Other (please specify)

[Answer]: A

## Q2. Which competitors count as direct competition for this build?

PRD §5.4 says that until the Insurance Check ships, the MVP "competes mainly with free bill checkers". The other categories (grievance firms, in-hospital claim desks, brokers, employer platforms, hospital billing software, insurer fraud analytics, government grievance bodies, US analogues) solve neighbouring problems or serve another side of the market.

A. Direct: free online bill checkers only. Indirect: grievance firms, in-hospital claim desks, brokers and employer platforms. Context only: hospital billing software, insurer fraud analytics, government bodies and US analogues
B. Direct: free bill checkers, grievance firms and in-hospital claim desks. Everything else indirect
C. Treat all nine categories as equal competitors
D. Not sure yet
X. Other (please specify)

[Answer]: A

## Q3. What should this stage do about market size?

PRD §4.3 deliberately publishes no TAM figure until its inputs are validated. It sizes the serviceable market from insured claims only (about 32.6 million health claims settled in FY25, marked [VALIDATE]), even though self-pay patients are a primary Phase 1 segment. PRD gap G-15 suggests adding a self-pay hospitalisation layer from national survey data (NSSO) and National Health Accounts (NHA) figures.

A. Add the self-pay layer G-15 asks for, from public sources, with every figure cited and marked indicative / [VALIDATE]; leave launch-year reach (SOM) to be modelled after the pilot, as the PRD says
B. Keep the PRD's approach unchanged: no sizing figures in this stage, only the inputs still needed
C. Size bottom-up only: the realistic number of audits in year one for an all-India digital launch, with the assumptions stated
D. Do both A and C
X. Other (please specify)

[Answer]: B

## Q4. Do you have first-hand customer evidence to include?

Customer expectations and trends are hypotheses until they are checked with real people. The PRD cites published surveys (for example LocalCircles) but no interviews, beta feedback or user testing of its own.

A. None yet: label every customer-expectation claim in this stage as a hypothesis, and list what research would confirm it
B. Yes, I can share notes or findings (please add a path or summary under X)
C. None yet, and I want this stage to include a short customer-research plan for the founder
X. Other (please specify)

[Answer]: A

## Q5. What should count as table stakes, and what as differentiators?

The free bill checker described in PRD §5.2 already does: bill upload, IRDAI non-payable and unbundling flags, room-rent cap impact, and query templates. PRD §5.4 says the MVP must be clearly better on five points: depth (every line extracted and normalised), objective findings (arithmetic, duplicates, MRP and NPPA ceiling-price breaches, room days vs length of stay), evidence and citations for every finding, follow-through (Billing Desk Summary, templates, escalation ladder), and usefulness before payment, including on interim bills.

A. Table stakes = what free checkers already do (listed above). Differentiators = the five PRD §5.4 points, plus "Who to Ask" (hospital side) and patient-side neutrality
B. Table stakes = as in A. Differentiators = only the five PRD §5.4 points
C. Leave the split to be derived from the refreshed competitor research in Q1
X. Other (please specify)

[Answer]: B

## Q6. For reading bills (pulling line items out of PDFs and phone photos), should we build, buy or partner?

This is the most expensive capability in the MVP. The extraction targets are at least 95% accuracy on digital PDFs and at least 90% on good-quality photos (intent statement). PRD hidden risk 4 warns that relying on a single model or vendor creates cost and continuity risk, and NFR-20 says third-party models must not be trained on user data without consent. This question is about the business choice only; the technical choice comes in later design stages.

A. Buy: use third-party AI document-reading services, under data-processing terms and with a second vendor kept viable so we can switch
B. Build: train or host our own extraction capability
C. Hybrid: buy for the first launch, and plan to bring the most valuable parts in-house once the Labelled Test Set shows where vendors fall short
D. Leave it open for the Feasibility stage, with the criteria set here
X. Other (please specify)

[Answer]: D

## Q7. Follow-up to an earlier answer: does data residency limit the buy options?

In intent capture you chose to use the PRD's working defaults and suggested resolutions as decisions (Q10). The PRD recommends that personal and health data be stored and processed in India (NFR-10). Gap G-14 notes this is still only a recommendation and that AI vendors may process data outside India. Many AI document-reading vendors process outside India, so this decides which vendors the build-vs-buy assessment can recommend.

A. Hard requirement: personal and health data is stored and processed only in India; vendors that cannot do this are excluded
B. Strong preference: India first, but a vendor outside India is acceptable for de-identified data under data-processing terms
C. Leave it open; the build-vs-buy assessment shows both options and the founder decides
X. Other (please specify)

[Answer]: C

## Q8. Should the build-vs-buy assessment cover anything beyond bill reading? (select all that apply)

PRD §13.10 lists the reference data the checks need; D22 already says the product team builds it from public sources, so that is not re-opened here unless you choose E.

A. Escalation help: referring users with high-stakes bills to an outside grievance firm or legal service (a partner option; the PRD keeps Phase 1 as self-help templates only, §6.8)
B. Report generation and download (Bill Breakdown, Billing Desk Summary as a PDF)
C. Internal tooling: rule library editing, the Labelled Test Set labelling tool, and ops/admin
D. Upload and WhatsApp-friendly sharing of photos and the report
E. Price-benchmark data: licensing outside data instead of only public sources
F. None: bill reading only
X. Other (please specify)

[Answer]: F

## Q9. Which market and regulatory trends should the trends report cover? (select all that apply)

PRD §4.2 lists the forces behind "why now". The trends report will cite each one and mark any unchecked claim [VALIDATE], as the PRD does.

A. Data protection: the DPDP Act 2023 and DPDP Rules (consent managers from 13 Nov 2026; main obligations enforceable from 13 May 2027)
B. Price regulation: NPPA ceiling prices and device caps, and the Supreme Court's attention on hospital rate standardisation
C. Insurance rules: IRDAI's standard non-payable lists and the May 2024 Master Circular (kept as context, because insurance checks are Phase 2)
D. Digital health infrastructure: ABDM and the National Health Claims Exchange (NHCX)
E. Falling cost and rising quality of AI document reading
F. How Indian families find and share help: search at the moment of need, WhatsApp family groups, consumer-rights content
X. Other (please specify)

[Answer]: B, C

## Q10. How should the international analogues (Goodbill and Resolve in the US) be used?

PRD §5.2 notes they are proven bill-audit-plus-negotiation businesses with outcome-based fees, but US-specific, and that contingency fees may not be legal in India (PRD §6.8).

A. As a short "lessons" note only: what they prove about demand and trust, and why their fee model does not transfer
B. Leave them out; they are not relevant to an Indian free Phase 1
C. Full profiles alongside the Indian competitors
X. Other (please specify)

[Answer]: B

## Q11. Follow-up to Q5: where does "Who to Ask" sit?

In Q5 you chose the five PRD §5.4 points only as differentiators (depth, objective findings, evidence and citations, follow-through, usefulness before payment). That leaves out "Who to Ask", the classification of every rupee as "challenge the hospital", "challenge the insurer" or "legitimately payable". The PRD calls that classification "the core differentiator" (§6.4, §13.1.3) and lists it in its own differentiation table (§5.4). In intent capture you chose to use the PRD's working defaults as decisions (Q10), so the two answers point different ways.

A. Keep the five points as the headline differentiators, and list "Who to Ask" (hospital side) as a supporting differentiator underneath them
B. Add "Who to Ask" (hospital side) as a sixth headline differentiator
C. Leave "Who to Ask" out of the differentiation brief; it is a product feature, not a market differentiator
X. Other (please specify)

[Answer]: C

## Consolidated Summary Confirmation

- Competitor evidence (Q1): PRD §5 is the baseline; the direct competitors (free online bill checkers such as eVaakil, plus any new free checker found) are re-checked on the web now, with links and dates.
- Direct competition (Q2): free online bill checkers only. Indirect: grievance firms, in-hospital claim desks, brokers and employer platforms. Context only: hospital billing software, insurer fraud analytics, government bodies.
- Market size (Q3): keep the PRD's approach; no sizing figures in this stage, only the inputs still needed (the self-pay gap G-15 stays listed as open).
- Customer evidence (Q4): none yet; every customer-expectation claim is labelled a hypothesis, with the research that would confirm it.
- Table stakes and differentiators (Q5, Q11): table stakes = what free checkers already do (upload, IRDAI non-payable and unbundling flags, room-rent cap impact, query templates); differentiators = only the five PRD §5.4 points (depth, objective findings, evidence and citations, follow-through, usefulness before payment). "Who to Ask" is left out of the differentiation brief.
- Bill reading build vs buy (Q6): not decided here; this stage sets the decision criteria and the Feasibility stage decides.
- Data residency (Q7): left open; the assessment shows India-only and outside-India options and the founder decides.
- Build-vs-buy coverage (Q8): bill reading only.
- Trends to cover (Q9): price regulation (NPPA ceiling prices and device caps, Supreme Court attention on hospital rate standardisation) and insurance rules (IRDAI non-payable lists and the May 2024 Master Circular, as context for Phase 2).
- US analogues (Q10): left out.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
