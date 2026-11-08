## Review

**Verdict:** NOT-READY
**Reviewer:** aidlc-architecture-reviewer-agent
**Date:** 2026-11-08T07:26:27Z
**Iteration:** 1

Advisory pass. Every edge in the unit-of-work-dependency.md edge block maps to one of C6 to C14 (checked edge by edge), and the stated limits (20 files, 60 pages, 5 min chunks, 25/30 minute idle, 15 minute job) are present. The findings below are gaps a developer would otherwise have to guess at.

### Findings

| ID | Severity | Location | Finding | Required action | Status |
|---|---|---|---|---|---|
| R-01 | Major | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > C7_session-privacy; C6 ScheduledJob; D-CD9 | FR2.8 requires an idle session to end at 30 minutes and its data to be discarded. A family that closes the phone sends no further requests, so nothing in the contracts triggers the end. C7 has no expire or end-session operation, and the only ScheduledJob registered is feedback-insights' 3-month deletion. Normal session end (which is when storeConsentedRecord is meant to run, per the edge map) also has no operation: only discardEverything exists. As written, abandoned sessions and consented-record storage have no owner. | Add an end/expire operation to C7 (endSession with reason: finished, idle, discarded) and a session-privacy ScheduledJob (or equivalent sweep) registered through case-processing's worker. State who calls storeConsentedRecord and when. Add a route or trigger for the "finished" end. | New |
| R-02 | Major | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > C2 (paths, staffSession, C4) | The staff cookie hbc_staff is "issued after sign-in" and the error enum has NOT_SIGNED_IN, but C2 has no sign-in start, identity-provider callback or sign-out route. C13 signIn takes an already verified identity, but nothing says which route receives the provider's response and calls it. Most routes also do not name the required role (only about half do), although the intro says each route names its role. | Add the sign-in start, callback and sign-out routes (or state that they are outside the OpenAPI spec and where they live). Give every C2 route its role. | New |
| R-03 | Major | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > Time limits and retries table; C3 | NFR1 (5 / 10 minutes to first insight) and NFR2 (15 minutes) depend on page reading, but the table has no time limit for the read step and no statement of read concurrency. One page can take up to 120 s x 4 attempts plus backoff (about 8 minutes worst case); up to 60 pages read serially cannot meet 15 minutes. Only the whole-job limit bounds it. Also, firstLookReady is exposed but no step or contract says when it becomes true. | Add a read-step limit and a page concurrency level (or say Functional Design/NFR Design must set it, and what NFR it must meet). Define the condition for firstLookReady. | New |
| R-04 | Minor | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > C1 /uploads, D-CD3, Open Questions | The fixed 5 MiB chunk exceeds the 4.5 MB request body limit of Vercel serverless functions, the working-assumption host. The open question names hosting but not this specific likely conflict. Also sizeBytes maximum 26214400 (25 MiB) has no stated unit match with FR4.4 "25 MB", and with a schema maximum an oversize file may return VALIDATION_FAILED instead of LIMIT_FILE_SIZE. | Choose a chunk size under the likely host limit, or record that the chunk size is tuned at hosting decision. Remove the schema maximum or state that LIMIT_FILE_SIZE is returned first, and say whether MB means 25,000,000 or 25 MiB. | New |
| R-05 | Minor | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > C1 Error enum vs C6/C7/C8 error codes | Error code vocabularies do not line up. C1 uses message, C6 uses reason. PASSWORD_REQUIRED is in the C1 enum but no route or C8 operation returns it. C7 DECLARATION_INCOMPLETE, C10 READ_FAILED, C12 STORE_FAILED, C13 PROMOTION_REQUIRED and the C9 timed-out states have no stated mapping to C1 or C2 codes. Also the 15-minute job timeout and 401 on DELETE /session path are only partially mirrored. | Add a short mapping table from in-process codes to HTTP status and C1/C2 codes; remove or wire PASSWORD_REQUIRED. | New |
| R-06 | Minor | aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/contract-design/contract-summary.md > C1 paths (/case, /lines, /report, /checklist, /first-look, /case/bill-stage etc.) | Many responses and request bodies are only prose ("lines", "case details") and PATCH /lines/{lineId}, PUT /case, PUT /plan, PUT /case/bill-stage have no schema for the body, so family-ui and the units it calls cannot be built in parallel against them. ReportContent is already listed as open. | Define at least field names and error codes for these bodies, or mark each explicitly as owned by the named unit's Functional Design with a due point. | New |

### Validation Tool Results

| Tool | Result | Interpretation |
|---|---|---|
| None declared | n/a | Manual checks: every unit-of-work-dependency.md edge is covered by C6 to C14; the dependency graph is acyclic; the document-intake to test-set-quality beta edge is resolved by BetaHandoverSink without a code dependency. |

### Summary

The contracts cover every dependency edge and the numeric limits mostly match the requirements. Three gaps would force guessing at implementation: who ends an idle or finished session, how staff sign in through C2, and how page reading meets the speed targets.
