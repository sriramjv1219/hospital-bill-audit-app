<!-- INVARIANT: examples are single-line HTML comments so a fresh template parses to total=0 (MEMORY_EMPTY). Do NOT un-comment or split across lines. t100 guards this. -->
> This file is kept up to date automatically while the stage runs. Add observations at the review step, not by editing here directly.

## Interpretations
<!-- example: 2026-05-29T10:14:32Z — chose REST over GraphQL; the consuming team only needs CRUD, revisit if subscriptions land -->

- 2026-11-08T06:40:00Z — added Q8 (change propagation) and Q9 (screens vs logic) on resume; the 7 questions drafted before the pause did not cover the stage's "interaction between components" and "UI component structure" topics, and Comprehensive depth expects 8-12 questions.

- 2026-11-08T07:05:00Z — letter edits stay on the device, never sent back to the server; FR9.4 only says letters are editable before download, and keeping typed text (which may include names) off our stores matches FR2.3 and the on-device PDF decision D-US9.
- 2026-11-08T07:05:00Z — the consented record is assembled at session close, not at the moment of "Yes, keep it"; this captures the latest ratings, and "Discard everything" counts as withdrawing storage consent (FR1.8).
- 2026-11-08T07:05:00Z — "When are you checking?" is owned by DocumentIntake's checklist rather than the Case in BillModel, because it is asked before any document is read and drives which documents are requested (FR4.10).

## Deviations
- 2026-11-08T07:05:00Z — added CaseProcessing as a 13th logic block beyond the ~12 in the Q4 answer; Q8 chose a single coordinator and none of the 12 named blocks was a natural home for job order, re-runs, version pinning and cancellation.
<!-- example: 2026-05-29T10:14:32Z — skipped the optional caching layer the stage prose suggested; the dataset is small enough that it adds risk -->

## Tradeoffs
- 2026-11-08T07:05:00Z — made CheckEngine and BillModel call no other block: CaseProcessing pins the rule-library version and passes it in. This keeps checks pure and reproducible (NFR6) and avoids cycles, at the cost of CaseProcessing being the hub every change passes through.
- 2026-11-08T07:05:00Z — screen routes resolve the session and gates and pass a trusted session ID to logic blocks, instead of each logic block calling SessionConsent; this removes cycles (SessionConsent must call every owner to delete) but means logic blocks must never have a public route of their own.
<!-- example: 2026-05-29T10:14:32Z — picked TDD over BDD this run; the team is unit-first and the domain is well-understood -->

## Open questions
- 2026-11-08T07:05:00Z — Contract Design should make "delete this session's data" a required interface on every session-data owner, so a new owner cannot be left out of SessionConsent's deletion list.
- 2026-11-08T07:05:00Z — the closed-beta hand-over from DocumentIntake to TestSetQuality must be switched off at launch; add it to the launch checklist with a test that no original is kept after launch.
<!-- example: 2026-05-29T10:14:32Z — confirm the retention window with compliance before the next stage hardens the schema -->
