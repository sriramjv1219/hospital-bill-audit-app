<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->
- 2026-11-07T16:35:59Z — Read Q5 "use tailwind and shadcn" as Tailwind for styling with shadcn/ui components copied into the repo; mapped every product component to a shadcn component or a native element, and kept native elements as the fallback if a route breaks the 300 KB limit.
- 2026-11-07T16:35:59Z — Combined rough S1 and the S2 privacy notice into M1, and the S2 choices into M2, to meet at most 2 screens before upload; S3 became the top of M3.

## Deviations
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->
- 2026-11-07T16:52:04Z — Revision after Request Changes (fix all 14): added M6a line editor, discard save reminder, processor wording in "Who sees it", pause only during server jobs with a per-job 15-minute cap; red on the discard confirm recorded as design decision DS-1, an extension of Q7.
- 2026-11-07T16:35:59Z — Replaced the start-screen line "Nothing about you is saved" with "Your files are deleted after we read them", because optional storage keeps de-identified data; founder or counsel approval of the final text is still required (AC1.3.4).

## Tradeoffs
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->
- 2026-11-07T16:35:59Z — Chose deep teal as the primary colour so buttons never look like a finding class (amber, blue, grey); every token pair was measured, the lowest text contrast is 5.5:1.
- 2026-11-07T16:35:59Z — Built the prototype as plain HTML and script with no external files, so it opens offline from the stage folder; it mirrors the design tokens rather than using Tailwind or shadcn.

## Open questions
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
