<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
- 2026-11-08T08:30:00Z — added an Origin-header check on every state-changing REST endpoint without asking; REST with a cookie session needs cross-site request protection that Server Actions would have given for free, and there is only one sensible way to do it.
- 2026-11-08T08:30:00Z — the user chose REST over the recommended Server Actions (Q2 B); the contract summary describes every browser call as an OpenAPI path so the 300 KB script limit is met with small fetch helpers rather than a client library.
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->

## Tradeoffs
- 2026-11-08T08:30:00Z — R-01 and R-03 settled by inverting the dependency through interfaces in contracts (scheduled-job registration, beta hand-over sink) rather than adding edges; the unit dependency map stays as approved, at the cost of a composition step that wires implementations at start-up.
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
