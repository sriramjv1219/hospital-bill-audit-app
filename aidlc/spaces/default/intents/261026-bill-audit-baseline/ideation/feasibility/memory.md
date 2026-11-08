<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-11-01T06:12:50Z — Recorded the free-text hosting answer verbatim as an Other answer: Vercel is the leaning, not a decision, so the artifacts treat hosting as open with Vercel as the working assumption.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-11-01T06:12:50Z — Did not ask the stage file's 'AWS services in use' question as written; the product has no hosting yet and the AWS link in this project is only the AI assistant's setup, so hosting and account status were asked instead.

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-11-01T06:12:50Z — Asked two follow-ups instead of resolving silently: a solo builder conflicts with the PRD's two-person labelling rule (D29), and a trial-first decision with no real bills risks an accuracy figure based only on synthetic bills.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
- 2026-11-01T06:12:50Z — Whether a cloud account exists is unknown, and the hosting choice is deferred; if the founder chooses India-only residency, the leaning hosting option must be checked for where it stores and processes data.
