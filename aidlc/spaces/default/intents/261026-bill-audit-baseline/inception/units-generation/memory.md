<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
- 2026-11-08T08:00:00Z — grouped BillReading, BillModel and CheckEngine as one bill-analysis unit instead of the Q2 grouping; my first reason (unit-level loops) was wrong once Q7 moved accuracy runs to the offline runner, which I corrected with you; the grouping stays because the offline runner then depends only on bill-analysis and rules-access.
- 2026-11-08T08:00:00Z — TestSetQuality's accuracy-run responsibility is split: the run itself lives in U12 accuracy-runner, and U10 test-set-quality imports the IDs-and-scores results; so U10 no longer calls bill reading or the check engine.
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->

## Deviations
- 2026-11-08T08:00:00Z — 12 units instead of about 10 (Q2): the shared contracts unit (Q5) and the offline accuracy runner (Q7) each became a unit.
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->

## Tradeoffs
- 2026-11-08T08:00:00Z — kept the background worker inside the case-processing unit rather than as its own packaging unit; the worker is only an entry point that runs case-processing jobs and the feedback-insights deletion schedule, and a separate unit would add a design pass with almost no content.
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
