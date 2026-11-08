# Build vs Buy — Reading Hospital Bills (Project RS, Phase 1)

This assessment covers one capability: reading hospital bills, that is, pulling every line item out of PDFs and phone photos [Q8]. It serves the Phase 1 Hospital Bill Check described in the approved intent statement (`intent-statement.md`, in `ideation/intent-capture/`).

**No decision is made here.** This document sets the criteria and the evidence needed; the Feasibility stage makes the decision [Q6]. Data residency stays open, so every option is shown both with and without an India-only requirement, and the founder decides [Q7]. It is a business view only; technology and vendor choices belong to later design stages.

## Sources

- [Q1]–[Q11]: answers in `market-research-questions.md` in this folder.
- PRD §n: `PRDs/app-baseline.md`, v0.14 (PRD-v2), 4 October 2026.
- Intent statement: `intent-statement.md` (success metrics and product boundary).

## 1. Why this capability matters

- Every differentiator starts with reading the bill well. Depth, objective findings and evidence for every finding (`competitive-analysis.md`, section 3) all depend on extracting every line correctly.
- The targets are fixed, measured against the Labelled Test Set (`intent-statement.md`; PRD §8.4):
  - line-item extraction accuracy of at least 95% on digital PDFs and at least 90% on good-quality photos;
  - first insight within 5 minutes and the full report within 15 minutes, both at the 90th percentile.
- Indian bills vary widely: multi-page, photographed at a hospital desk, inconsistent layouts, sometimes partly in Hindi or regional scripts. Phase 1 reads English only (PRD D9).
- A new national bill-format standard and Karnataka's itemised-billing rule may make bills more uniform over time (see `market-trends.md`, section 2.1), but most bills today will not follow a standard.

## 2. What is not open

These already constrain any option (PRD):

| Constraint | Source |
|---|---|
| Any third-party processor of personal data is contracted on data-processing terms | PRD NFR-20 |
| Third-party models are not trained on user data without explicit consent | PRD NFR-20 |
| Original uploads are deleted after extraction; only de-identified structured data is kept, for 3 months | PRD NFR-09, D8, D24 |
| Avoid relying on a single model or vendor; keep alternatives viable | PRD §18.1 hidden risk 4, G-19 |
| Every model or prompt change is evaluated against the Labelled Test Set before release | PRD NFR-17 |
| No domain experts in the Phase 1 team; the product team does the work | PRD D11, D22 |

## 3. Options

| Option | What it means in business terms |
|---|---|
| **Buy** | Use third-party document-reading services under data-processing terms, with a second supplier kept usable so we can switch |
| **Build** | Develop and run our own extraction capability |
| **Hybrid** | Buy for launch, then bring the most valuable parts in-house once the Labelled Test Set shows where suppliers fall short |

Data residency changes which suppliers each option can use. The founder decides it [Q7]:

| Residency choice | Effect on Buy and Hybrid | Effect on Build |
|---|---|---|
| **India only**: personal and health data stored and processed only in India (PRD NFR-10 recommendation) | Only suppliers that process in India qualify; fewer choices, possibly higher cost | Must also be hosted in India |
| **India first**: a supplier outside India is acceptable for de-identified data under data-processing terms | More suppliers qualify. Raw bill images contain personal data, so each supplier needs a clear answer on what leaves India and when | No extra constraint beyond the hosting choice |

## 4. Decision criteria for the Feasibility stage

Scoring scale per the market-research method: −2 (strongly favours Buy) to +2 (strongly favours Build). The "Initial lean" column is a **hypothesis** from this stage's reading of the PRD, not a score. Feasibility replaces it with evidence.

| Criterion | What to find out | Initial lean (hypothesis) |
|---|---|---|
| Is it a core differentiator? | The differentiators are what is done with the lines (normalisation, objective checks, evidence), not raw text capture (`competitive-analysis.md`, section 3). Does raw extraction itself set us apart? | Leans Buy |
| Accuracy on Indian bills | Does each option meet at least 95% on PDFs and 90% on good photos against the Labelled Test Set, broken down by bill type (PRD §8.4)? | Unknown until tested |
| Speed | Does each option fit within the 5-minute first-insight and 15-minute full-report targets at the 90th percentile (PRD NFR-01, NFR-02)? | Unknown until tested |
| Time to first usable version | No fixed timeline for this build (`intent-statement.md`), but the Labelled Test Set and launch-set checks depend on extraction working early (PRD §10.1) | Leans Buy |
| Three-year cost per audit | Usage fees and integration (Buy) vs development, hosting and upkeep (Build), at pilot volume and at 10× pilot volume (PRD NFR-15). Phase 1 is free, so every audit is a cost (PRD D25) | Unknown |
| Data sensitivity and residency | Can the option keep raw bill images within the residency choice above [Q7]? Can it delete originals after extraction (PRD D8)? | Depends on the founder's choice |
| Team skills | Phase 1 has a product team and no domain experts (PRD D22). Can the team build and maintain extraction to the accuracy targets? | Leans Buy |
| Upkeep burden | Who handles new bill layouts, model drift and re-testing against the Labelled Test Set (PRD NFR-17)? | Leans Buy |
| Lock-in and continuity | Can we switch suppliers without losing accuracy (PRD hidden risk 4)? | Favours keeping two suppliers usable, or Hybrid |
| Training on user data | Does the supplier contractually refuse to train on user data without consent (PRD NFR-20)? | A pass/fail condition, not a score |

**Pass/fail conditions.** Any option that fails one of these is out, whatever its score:

1. It meets the Labelled Test Set accuracy targets.
2. It meets the data-processing, no-training and deletion constraints in section 2.
3. It complies with whichever residency choice the founder makes.

**The method's default rule.** If a capability is not a core differentiator and a mature supplier exists, buy it; build only when the capability is central to the advantage or suppliers cannot meet the requirements. Applying that rule here is a **hypothesis** for Feasibility to test, not a recommendation from this stage [Q6].

## 5. Evidence Feasibility should gather

- Accuracy and speed results for each shortlisted option on the same sample from the Labelled Test Set: digital PDFs, good photos, poor photos, multi-page bills, bills with Hindi or regional-script lines.
- Cost per audit for each option at pilot volume and at 10× pilot volume.
- For each supplier: where data is processed and stored, retention and deletion terms, and the no-training commitment.
- The founder's decision on data residency (PRD G-14) [Q7].

## Assumptions & Open Questions

- The residency decision (PRD G-14) is open and blocks any final supplier choice [Q7].
- The "Initial lean" entries are hypotheses based on the PRD, not tested evidence.
- Whether a second supplier can reach the same accuracy is unknown until both are tested on the Labelled Test Set.
