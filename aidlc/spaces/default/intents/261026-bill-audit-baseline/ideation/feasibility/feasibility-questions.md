# Feasibility & Constraints — Questions

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`.
- The approved intent statement (`ideation/intent-capture/intent-statement.md`) and the market research in `ideation/market-research/` (`competitive-analysis.md`, `market-trends.md`, `build-vs-buy.md`).
- The PRD's constraints (§17), risks (§18) and compliance considerations (§19) in `PRDs/app-baseline.md`.
- The project folder: a new web app started from the standard Next.js template (two commits, no product code yet).

Already settled, so not asked again:

- Build to the data-protection (DPDP) standard from day one, not from the May 2027 enforcement date (PRD §19.4).
- Phase 1 launches without legal or tax review, a risk the founder accepted (PRD D27).
- The team is the product team only, with no domain hire or consultant (PRD D22). Labelled Test Set sources are network bills and synthetic bills first, then a closed beta (PRD D28).
- Original uploads are deleted after extraction, and only de-identified data is kept, for 3 months (PRD D8, D14, D24).
- Whether personal and health data must stay in India is still open for the founder (market research Q7). This stage assesses both cases instead of asking again.

---

## Q1. Who will build this?

The PRD says the team has no domain experts (D22) but does not describe the engineering side. Team size and skills decide how much can be built in-house, especially the bill-reading capability.

A. You alone, working with this AI-assisted workflow
B. A small team (2–5 engineers)
C. A larger team (6 or more engineers)
D. Not decided yet
X. Other (please specify)

[Answer]: A

## Q2. Is the existing web app starter the intended starting point?

The project folder holds a new Next.js / React / TypeScript app with nothing built on it yet. This stage only checks that the base is viable; detailed technical choices come later.

A. Yes, build the product on this starter
B. It is a placeholder; the base is still open
X. Other (please specify)

[Answer]: A

## Q3. Where should the product be hosted?

Hosting decides which India regions, data-processing terms and managed services are available. This project's own development setup uses AWS for the AI assistant, but that does not decide where the product runs.

A. AWS
B. Another cloud provider (please name it under X)
C. Not decided: assess the options against the residency cases
X. Other (please specify)

[Answer]: X. Other: "Mostly Vercel. But I would like to decide at a later point."

## Q4. Is a cloud account for the product already in place?

A. Yes, an account or organisation exists and is ready to use
B. No, one needs to be set up
C. Not sure
X. Other (please specify)

[Answer]: C

## Q5. How should this stage close the bill-reading build-vs-buy decision?

Market research deferred the decision here (`build-vs-buy.md`). No option has been tested yet against the accuracy targets: at least 95% on digital PDFs and at least 90% on good photos.

A. Make a provisional decision now, and require a measured trial on a sample of the Labelled Test Set before Construction depends on it
B. Make no decision; record a trial as a dependency and decide after it
C. Make a final decision now using the default rule: buy what is not a core differentiator
X. Other (please specify)

[Answer]: B

## Q6. How many real bills can the team gather for the Labelled Test Set before the build starts?

The accuracy and precision targets can only be measured against the Labelled Test Set, which the PRD sizes at 300–500 bills (PRD §10.1). Its first sources are consented bills from the team's personal network, plus synthetic bills with planted errors (PRD D28).

A. None yet: start with synthetic bills only
B. Fewer than 50 real bills
C. 50–300 real bills
D. More than 300 real bills
X. Other (please specify)

[Answer]: A

## Q7. Is there a budget limit for running costs?

Phase 1 is free (PRD D25), so every audit is a cost with no revenue against it. Bill reading is likely to be the biggest cost per audit.

A. No fixed budget yet: estimate the cost per audit and flag it
B. A monthly ceiling (please give the amount under X)
C. A cost-per-audit ceiling (please give the amount under X)
X. Other (please specify)

[Answer]: A

## Q8. What volume should feasibility plan for in the first months after launch?

The PRD asks the system to handle 10 times the pilot volume (NFR-15) but never sets the pilot volume. Volume drives the cost and capacity checks.

A. Under 1,000 audits a month
B. 1,000–10,000 audits a month
C. Over 10,000 audits a month
D. Unknown: plan conservatively and state the assumption
X. Other (please specify)

[Answer]: A

## Q9. How should internal staff sign in to the internal tools?

The rule library, the Labelled Test Set labelling tool and ops/admin are internal. The PRD requires strong authentication for internal staff (NFR-08); users of the bill check have no login (D6).

A. Through an existing company sign-in (please name it, for example Google Workspace or Microsoft 365, under X)
B. No company sign-in exists; the tools need their own strong sign-in
C. Not decided yet
X. Other (please specify)

[Answer]: B

## Q10. Are there organisational blockers or competing priorities?

For example: other projects competing for the same people, a freeze period, or approvals needed before cloud spend.

A. None known
B. Yes (please describe under X)
X. Other (please specify)

[Answer]: A

## Q11. Follow-up to Q1: who does the second, independent labelling of real bills?

In Q1 you said you are building this alone with this AI-assisted workflow. The PRD's founder decision D29 says each real bill in the Labelled Test Set is labelled independently by two team members, who then reconcile the differences. The labels must also stay independent of the AI being tested. With one person, two independent labels are not possible as written.

A. You and the founder act as the two labellers
B. Bring in a second person (a friend, colleague or contractor) only for labelling
C. One person labels and a second person spot-checks a sample (this changes decision D29 and needs the founder's agreement)
D. Not decided yet: record it as an open dependency
X. Other (please specify)

[Answer]: A

## Q12. Follow-up to Q5 and Q6: what should the bill-reading trial run on?

You chose to decide build vs buy only after a measured trial (Q5), and the team has no real bills yet (Q6). A trial on synthetic bills alone may overstate accuracy, because real phone photos taken at a hospital desk are messier than generated ones.

A. Run the trial on synthetic bills first, then repeat it on real bills once some are gathered, before the decision is final
B. Wait until a minimum number of real bills is gathered (for example 50), then run one trial on real and synthetic bills together
C. Run the trial on synthetic bills only and accept the risk
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Who builds (Q1): you alone, working with this AI-assisted workflow.
- Starting point (Q2): build the product on the existing Next.js / React / TypeScript starter.
- Hosting (Q3): "Mostly Vercel. But I would like to decide at a later point." Vercel is the leaning; the final choice is made later.
- Cloud account (Q4): not sure whether one exists.
- Bill reading (Q5, Q12): no build-vs-buy decision in this stage. A measured trial is a dependency: first on synthetic bills, then repeated on real bills once some are gathered, before the decision is final.
- Real bills for the Labelled Test Set (Q6): none yet; start with synthetic bills only.
- Labelling (Q11): you and the founder are the two independent labellers of real bills (PRD decision D29).
- Budget (Q7): no fixed budget; estimate the cost per audit and flag it.
- Volume (Q8): plan for under 1,000 audits a month in the first months after launch.
- Internal sign-in (Q9): no company sign-in exists; the internal tools need their own strong sign-in.
- Blockers (Q10): none known.
- Carried in from earlier stages: data residency stays open for the founder, and both cases are assessed; DPDP standard from day one; no legal review before launch; product team only.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
