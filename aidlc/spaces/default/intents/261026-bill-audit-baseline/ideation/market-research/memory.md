<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-10-27T17:19:33Z — Did not re-ask decisions the PRD already settles; launch geography (D17), Phase 1 free (D25), reference data built from public sources (D22) and the patient-side brand stance are listed as settled in the questions file instead of being asked again.
- 2026-10-27T17:19:33Z — Treated data residency as a narrow follow-up to intent-capture Q10 instead of silently applying the NFR-10 recommendation as a decision; the PRD itself marks it as only a recommendation (G-14), and it decides which vendors build-vs-buy can name. The user chose to leave it open for the founder.
- 2026-10-27T17:33:29Z — Included the national hospital-bill format standard (IS 19493:2025) and Karnataka's itemised-billing rule in the trends report under price regulation, though the PRD does not mention them; they surfaced during the competitor re-check and bear directly on the itemised-bill hurdle (PRD §6.5).

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-10-27T17:19:33Z — The stage steps mention a separate differentiation strategy brief, but the stage declares only three artifacts; the differentiation content goes into competitive-analysis.md as its own section instead of a fourth file.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-10-27T17:19:33Z — Asked a follow-up (Q11) because the Q5 answer (five PRD points only) left out "Who to Ask", which the PRD calls the core differentiator; one extra question was cheaper than carrying a conflict with the PRD forward. The user chose to leave it out of the differentiation brief.
- 2026-10-27T17:33:29Z — Build-vs-buy gives criteria, pass/fail conditions and an initial lean labelled as hypothesis, but no recommendation, because the user deferred the decision to Feasibility; naming no vendors also keeps the ideation document free of technology choices.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-10-27T17:19:33Z — Data residency (PRD G-14) is still open and the founder decides; Feasibility must pick it up before any bill-reading vendor is chosen.
- 2026-10-27T17:19:33Z — The user left data protection (DPDP) out of the trends report; Feasibility and the compliance view need to cover it as a constraint.

