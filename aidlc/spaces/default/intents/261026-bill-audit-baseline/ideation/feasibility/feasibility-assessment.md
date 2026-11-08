# Feasibility Assessment — Hospital Bill Check (Project RS, Phase 1)

This assessment asks whether the Phase 1 Hospital Bill Check, as bounded in the approved intent statement (`intent-statement.md`, in `ideation/intent-capture/`), can be built and run by the team you described, under the constraints that apply. It builds on the market research in `ideation/market-research/`: `competitive-analysis.md`, `market-trends.md` and `build-vs-buy.md`.

The full constraint list is in `constraint-register.md`. Risks, assumptions, issues and dependencies are in `raid-log.md`.

## Sources

- [Q1]–[Q12]: answers in `feasibility-questions.md` in this folder.
- PRD §n / D-n: `PRDs/app-baseline.md`, v0.14 (PRD-v2), 4 October 2026.
- Web sources, accessed 1 November 2026, are linked where used. **[VALIDATE]** marks a claim from a secondary source or one not yet checked against a primary source.
- Labels: estimates are deliberately conservative, as the ideation guardrails require. **Hypothesis** marks a belief that has not yet been tested.

## 1. Verdict

**Feasible with conditions.** Nothing found here makes Phase 1 impossible. Two things are unproven and could still stop or reshape it:

1. **Reading bills accurately enough.** The targets are at least 95% line-item accuracy on digital PDFs and at least 90% on good photos (`intent-statement.md`). No option has been tested, and the build-vs-buy decision waits for a trial [Q5].
2. **A solo builder carrying the whole Phase 1 boundary.** You are building alone with this AI-assisted workflow [Q1]. The boundary covers:
   - the bill check itself;
   - the rule library;
   - the Labelled Test Set and its labelling tool;
   - ops/admin (`intent-statement.md`).

   There is no fixed timeline (`intent-statement.md`), so this is a risk to pace and to quality, not to a deadline.

Everything else is feasible, given the decisions already made, or gated on a founder decision that is listed below.

| Area | Rating | Why |
|---|---|---|
| Bill reading (extraction) | **Unproven: highest risk** | Accuracy targets are untested; there are no real bills yet [Q6]; the decision waits for a trial [Q5] [Q12] |
| Checks and findings (rule library) | Feasible, with domain risk | The launch set favours the most objective checks (PRD C8); rules come from public sources without domain experts (PRD D22, RK27) |
| Report, templates, Billing Desk Summary | Feasible | Mostly content and presentation work on a standard web app [Q2] |
| Speed targets | Likely feasible; to be measured | First insight within 5 minutes and the full report within 15 minutes, both at the 90th percentile (PRD NFR-01, NFR-02). This depends on bill-reading speed, which is untested |
| Labelled Test Set | Feasible, but slow to build | It starts with synthetic bills only [Q6]; real bills are labelled by you and the founder [Q11] |
| Internal tools with their own strong sign-in | Feasible | There is no company sign-in to reuse, so the tools need their own [Q9]; this adds build and security work |
| Hosting and data residency | Feasible in one residency case; open in the other | Vercel is the leaning [Q3]. Whether everything can stay in India is unconfirmed (section 4) |
| Data protection and security | Feasible, and demanding | DPDP standard from day one (PRD §19.4) with no legal review before launch (PRD D27) |
| Running cost | Unknown but bounded | No budget is set [Q7]; volume is under 1,000 audits a month at first [Q8]. Bill reading is the main cost (section 5) |
| Team and organisation | Feasible, at a slower pace | One builder [Q1]; no blockers known [Q10] |

## 2. Technical viability

### 2.1 Bill reading

This is the foundation of every differentiator: depth, objective findings and evidence (`competitive-analysis.md`, section 3).

- **Why it is hard.** Indian bills come in many layouts. They are often multi-page, photographed at a desk, and sometimes partly in Hindi or regional scripts (PRD C3, RK23). Phase 1 reads English only, so unreadable lines must be listed clearly (PRD D9, DP-08).
- **What is known.** Nothing yet about accuracy on Indian bills. The build-vs-buy criteria and pass/fail conditions are set (`build-vs-buy.md`, section 4). The decision waits for a measured trial [Q5].
- **The trial** [Q12]:
  1. Run it on synthetic bills with planted errors first.
  2. Repeat it on real bills once some have been gathered and labelled.
  3. Only then is the decision final.
- **Conservative view.** Synthetic bills are cleaner than real photos, so the first trial will probably look better than real-world results. Treat it as a screen to rule options out, not as proof that one works.
- **Safety net already in the PRD.** These lower the harm if extraction is imperfect:
  - users confirm low-confidence fields;
  - arithmetic is re-verified;
  - only findings linked to evidence are shown;
  - checks below their accuracy target stay hidden (PRD RK7, NFR-05).

### 2.2 Checks, findings and the rule library

- Feasible for the objective checks: arithmetic, duplicates, MRP and NPPA ceiling prices, room days vs length of stay (PRD §5.4, C8).
- NPPA reference data must follow each notification. The stent prices were revised from April 2026, and the knee-implant cap was extended only to November 2026, which is this month (`market-trends.md`, section 2.2). The rule library therefore needs a dated, versioned reference set from launch (PRD NFR-16, RK11).
- Plausibility checks that need "typical quantity" data have no Phase 1 source (PRD G-03). They are not feasible in Phase 1 unless a public source is found or the Labelled Test Set is used.

### 2.3 Speed

- **Targets.** 5 minutes to first insight and 15 minutes to the full report, both at the 90th percentile; 10 minutes to first insight for photographed multi-page bills (PRD NFR-01, NFR-02).
- **Why they look reachable.** They are measured in minutes, not seconds, so they look achievable for a single bill **[hypothesis]**. They depend on bill-reading speed, which the trial must also measure (`build-vs-buy.md`, section 4).
- **Time limits.** Long-running work needs to fit within the hosting platform's time limits for a single request. This is a design concern for later stages; it is listed in `constraint-register.md`.

### 2.4 Starting point

The existing Next.js / React / TypeScript web app is the base [Q2]. It is a standard, widely used starting point for a mobile and desktop web product (PRD: mobile and desktop web). It carries no feasibility risk. Detailed design is for later stages.

## 3. Data, test set and labelling

- **What the targets depend on.** Every accuracy and precision target is measured against the Labelled Test Set, sized at 300–500 bills (PRD §10.1, §8.4).
- **What exists today.** There are no real bills yet [Q6]. The set starts with synthetic bills with planted errors (PRD D28), and real bills come later from your personal networks and a closed beta (PRD D28).
- **Labelling.** You and the founder are the two independent labellers of real bills [Q11], which satisfies PRD decision D29.
- **Workload.** Two-person labelling of 300–500 real bills is a large job for two people who are also building and running the product **[hypothesis]**. Labelling throughput is tracked as a risk in `raid-log.md`.
- **Independence.** Labels must stay independent of the AI being tested (PRD D29). With the same person building the AI and labelling bills, independence depends on discipline: label before seeing the AI's output.
- **Test-set skew.** Network bills will over-represent a few cities and large hospitals (PRD RK30). With an all-India launch (PRD D17), accuracy may not hold everywhere.

## 4. Hosting and data residency (platform engineer's view)

- **Hosting.** Your answer: "Mostly Vercel. But I would like to decide at a later point." [Q3]. Vercel is therefore the working assumption, not a decision. Whether a cloud account exists is not known [Q4].
- **Residency.** Still open for the founder (market research, `build-vs-buy.md`; PRD G-14). Both cases are assessed:

| | India only: store and process only in India | India first: some processing outside India acceptable for de-identified data |
|---|---|---|
| Web app and processing | Vercel lists a Mumbai region (`bom1`, mapped to AWS ap-south-1). Functions default to a US region and must be pinned to Mumbai ([Vercel regions](https://vercel.com/docs/concepts/edge-network/regions)) **[VALIDATE]** | No extra constraint |
| Stored data (de-identified results; uploads until deleted) | Whether Vercel's own storage products keep data only in India could not be confirmed; the storage would probably need to come from a provider that guarantees India storage **[VALIDATE]** | A wider choice of storage |
| Bill reading | Only suppliers that process in India qualify (`build-vs-buy.md`, section 3) | Raw bill images hold personal and health data, so each supplier needs a clear answer on what leaves India |
| Platform logs | Logs may contain personal data if not designed carefully. Where they are stored must be checked **[VALIDATE]** | Less constrained, but still personal data |
| **Verdict** | **Feasible, but Vercel alone may not be enough**; this needs confirming with the provider | **Feasible** |

**What this means.** The founder's residency decision should come before the hosting decision. If it comes later, the hosting choice may have to be revisited. Both are recorded as dependencies in `raid-log.md`.

## 5. Running cost

- **No budget or ceiling is set** [Q7]. Volume is planned at under 1,000 audits a month at first [Q8].
- **Cost drivers per audit, in likely order:**
  1. bill reading (per page or per image);
  2. analysis and report generation;
  3. hosting and storage, which is small at this volume.
- **Why there is no figure yet.** Bill-reading cost depends on the option chosen and on pages per bill, and neither is known. Quoting a figure now would be speculation.
- **How to get the figure.** The trial must record cost per audit alongside accuracy (`build-vs-buy.md`, section 5):

  > Monthly running cost ≈ audits per month × (pages per audit × reading cost per page + analysis cost per audit) + fixed hosting cost

- **Conservative view.** At under 1,000 audits a month, running cost is unlikely to be what decides feasibility **[hypothesis]**. It becomes a real question at 10 times that volume (PRD NFR-15), because Phase 1 earns no revenue (PRD D25).

## 6. Regulatory and compliance (compliance specialist's view)

### 6.1 Data classification

| Data | Classification | Where it exists | Phase 1 rule |
|---|---|---|---|
| Uploaded bills, discharge summaries, implant stickers (originals) | **Restricted**: sensitive personal data, health information (PRD §19.4, IT Act SPDI) | During the session only | Deleted after extraction (PRD D8) |
| Extracted line items before de-identification | Restricted | During the session | Identifiers stripped before storage (PRD D14, SD-05) |
| Stored de-identified data (age band, gender, hospital, city, admission month, length of stay, line items) | Confidential: may still be personal data if re-identification is possible (PRD §19.4) | Stored for 3 months | Deleted after 3 months (PRD D24) |
| Labelled Test Set: redacted originals and labels | Restricted | Kept for the life of the set (PRD D30) | Identifiers blacked out on receipt; testers can withdraw (PRD D30) |
| Internal staff accounts and access logs | Confidential | Ongoing | Strong sign-in [Q9]; every access to personal data is logged (PRD NFR-08) |

### 6.2 Obligations that apply to Phase 1

These come from PRD §19.4 and C2; this stage did not re-research them.

- **DPDP Act 2023 and Rules 2025.** Built to the standard from day one (PRD §19.4). Covers notice, consent, purpose limitation, the rights of the person the data is about, breach notification, children's data and nomination **[VALIDATE]**.
- **IT Act SPDI Rules 2011.** Medical records are sensitive personal data, and reasonable security practices are required.
- **CERT-In Directions.** Specified cyber incidents must be reported within 6 hours **[VALIDATE]**. A solo builder needs a written incident plan that one person can carry out.
- **Third-party, minor and deceased patients' data.** Families upload a patient's data. Minors are allowed on a parent's declaration (PRD D15, RK24). Bereaved families need care (PRD §18.1).
- **The regulatory perimeter.** No legal advice, no representation, no clinical opinion from AI, and no encouragement to withhold payment in a way that blocks discharge (PRD C1, C10).

### 6.3 Compliance feasibility

- **Feasible:** the product design already minimises data (no accounts, originals deleted, de-identification, 3-month retention).
- **Weak point:**
  - Launching without legal review is a risk the founder accepted (PRD D27, RK28).
  - A solo builder also means no one else checks security or privacy work. NFR-08 asks for periodic third-party security testing, and that is the main outside check left.
  - Recorded in `raid-log.md`.

## 7. Conditions for proceeding

Phase 1 is feasible if these conditions are met. Each has a matching entry in `raid-log.md`.

| # | Condition | Needed before |
|---|---|---|
| F1 | The bill-reading trial runs on synthetic bills and then on real bills, and at least one option meets the accuracy and speed targets and the pass/fail conditions in `build-vs-buy.md` | Construction depends on bill reading |
| F2 | The founder decides on data residency (PRD G-14) | The hosting choice and any bill-reading supplier |
| F3 | The hosting choice is confirmed against that residency decision, including where stored data and logs live | Construction of anything that stores data |
| F4 | Real bills start coming in from your personal networks, with consent, so the Labelled Test Set can grow beyond synthetic bills | Any check goes live (PRD NFR-05) |
| F5 | An incident-response plan and periodic third-party security testing are arranged, because no one else reviews the work | Launch |

## Assumptions & Open Questions

- The accuracy of every bill-reading option on Indian bills is unknown until the trial [Q5] [Q12].
- Whether Vercel can keep stored data and logs only in India is unconfirmed **[VALIDATE]**.
- Whether a cloud account already exists is not known [Q4].
- The labelling workload of 300–500 real bills for two people is an estimate, not a measurement **[hypothesis]**.
- The cost per audit is unknown until the trial records it [Q7].
