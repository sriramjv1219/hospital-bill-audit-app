# Contract Design — Questions

This stage pins down what crosses each boundary: between the 12 units (`inception/units-generation/unit-of-work.md`) and between the product and the outside (the browser, the bill-reading service, the offline accuracy runner's output). For each boundary it settles the shape of the data, how it travels, who owns the spec, and what happens on failure.

Each question marks the option I recommend and gives a one-line reason (project practice).

## Already settled, not asked again

- **Units call each other in-process through module interfaces, with no internal network APIs** (Units Generation D-UG5). Inter-unit contracts are therefore typed interfaces, not HTTP APIs.
- **All cross-unit types live in the contracts unit (U1)** (D-UG1, Q5 of Units Generation).
- **Report PDFs are built on the device; letter edits never return to the server** (Domain Design ADR-007).
- **Pages with bill data are rendered per request and answer with `Cache-Control: no-store`** (FR2.10, ADR-004).
- **The session cookie is HttpOnly, Secure and same-site, and the session ID never appears in a URL** (US7.9).
- **Nothing is ever sent to a hospital or insurer** (FR11.3).

## Carried forward from Units Generation

The architecture reviewer's five points were approved as accepted risks. Per the project rule they are raised here first:

- R-01: the worker runs feedback-insights' scheduled deletion, but the dependency map has no line for it;
- R-02: bill-analysis and findings-report are tagged "library" although they hold health data;
- R-03: the beta-only link from document-intake to test-set-quality makes upload depend on five units;
- R-04: the first build slice (scaffolding, CI, test tooling) has no unit;
- R-05: the 8 Won't Have stories are accepted deferrals.

---

## Q1. How to handle the carried points R-01 to R-05

A. (Recommended) Settle R-01 and R-03 here as contracts, because both are about what crosses a boundary: feedback-insights publishes a "scheduled deletion job" interface in contracts that the worker only registers and runs, and the beta hand-over becomes an interface declared in contracts and implemented by test-set-quality, so document-intake depends only on contracts. Record R-02 as a note that NFR and Infrastructure Design cover the stores and the reader service in bill-analysis and findings-report as if they were services. Pass R-04 to Delivery Planning. R-05 stays accepted
B. Pass all five to Delivery Planning
C. Leave them as accepted risks with no further action
X. Other (please specify)

[Answer]: A

## Q2. How the family's browser talks to the server

The family screens need to: record consent and declarations, upload files, start processing, see progress, confirm details, fix a line, mark findings, rate findings and discard everything.

A. (Recommended) Next.js Server Actions for every form-like action (consent, confirm, fix a line, marks, ratings, discard), and a small set of route handlers only for file upload and progress, all described in the contract summary. This is the native pattern in this Next.js version and keeps the script per page small
B. REST route handlers for everything, described with OpenAPI
C. Server Actions for everything, including uploads and progress
X. Other (please specify)

[Answer]: B

## Q3. How the progress screen gets updates

A report can take up to 15 minutes. Background progress checks must never keep a session alive (D-US13).

A. (Recommended) Short polling: the progress screen asks every 3 seconds; the progress endpoint is read-only and does not count as activity. It works the same on every host and is easy to test
B. Server-sent events: the server pushes each step as it happens
C. WebSockets
X. Other (please specify)

[Answer]: A

## Q4. How uploads resume after a dropped connection

Finished files must not be sent again, and after a reload only unfinished files are chosen again (NFR11, D-US16). Each file is at most 25 MB.

A. (Recommended) Our own chunked upload: the browser sends each file in fixed-size chunks, asks the server how much it already has before resuming, and the server confirms a whole file with a checksum. Few endpoints, no extra server, works on any host
B. The open "tus" resumable-upload protocol with a ready-made server library
C. Upload each file in one request and retry the whole file on failure
X. Other (please specify)

[Answer]: A

## Q5. How contracts change over time

All units live in one codebase and are released together, but some shapes are stored or written to files: the consented record, rule snapshots, the accuracy runner's output.

A. (Recommended) In-process contracts change in the same commit as every consumer, and the type check enforces it; adding a field is always allowed; removing or changing a field needs a note in this contract summary. Every stored or file shape carries an explicit schema version, and readers refuse an unknown version
B. Version every contract separately, with semantic versions
C. No formal policy
X. Other (please specify)

[Answer]: A

## Q6. How errors cross unit boundaries

Team practice leaves error handling to the design stages; the construction rules require errors at integration boundaries to be surfaced, not swallowed.

A. (Recommended) Every unit interface returns a typed result (success, or a named error code with a plain reason) and never throws across a unit boundary. Retries happen only at outside edges (the bill reader, stores), and the coordinator gives each step a time limit. This makes "what can fail" visible in every contract and easy to test
B. Units throw typed error classes, caught by the caller
C. Leave it to Functional Design per unit
X. Other (please specify)

[Answer]: A

## Q7. What the bill-reader adapter returns

The bill reader is chosen by the reader trial (requirements OQ8). The adapter keeps it replaceable (ADR-005).

A. (Recommended) A reader-neutral result per page: header fields, line items and totals, each value with a confidence between 0 and 1 and a reference to its page and position; unread lines listed separately; page legibility; the reader's name and version. The adapter never logs page content, gives up on a page after 120 seconds, and retries a failed page at most 3 times. This lets the trial winner plug in without touching any check
B. Pass the reader's own output through, and let bill-analysis map it
C. Decide the shape after the reader trial
X. Other (please specify)

[Answer]: A

## Q8. What the offline accuracy runner hands to the internal tools

Only bill IDs and scores may leave the offline machine (Units Generation D-UG6; Domain Design ADR-009).

A. (Recommended) One JSON file with a schema version, run details (reader, model and rule-library versions, approved-service confirmation) and per-bill and per-check scores. The import refuses any field the schema does not list, so bill content cannot slip in
B. A CSV of per-bill scores
C. Let the runner write directly into the test-set store over the network
X. Other (please specify)

[Answer]: A

## Q2a. Follow-up: do the internal tools use REST too?

You chose REST route handlers for everything on the family side (Q2 B). The internal tools (rule library, test set, promotion, dashboard) also need a browser-to-server style.

A. (Recommended) Yes, the same REST style for the internal tools, under their own path prefix and sign-in; one pattern is simpler to build, test and secure
B. No, Server Actions for the internal tools, since only two staff use them
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Q1: settle R-01 and R-03 here as contracts (feedback-insights publishes a scheduled-deletion job interface that the worker only registers and runs; the beta hand-over is an interface declared in contracts and implemented by test-set-quality, so document-intake depends only on contracts); record R-02 as a note that NFR and Infrastructure Design cover the stores and the reader service in bill-analysis and findings-report as if they were services; pass R-04 to Delivery Planning; R-05 stays accepted (A).
- Q2 and Q2a: REST route handlers, described with OpenAPI, for every browser-to-server call, on the family side and in the internal tools, which use their own path prefix and sign-in (B, A).
- Q3: the progress screen polls every 3 seconds; the progress endpoint is read-only and does not count as activity (A).
- Q4: our own chunked upload: fixed-size chunks, the browser asks the server's received offset before resuming, and a checksum confirms each whole file (A).
- Q5: in-process contracts change in the same commit as every consumer, enforced by the type check; adding a field is always allowed; removing or changing one needs a note in the contract summary; every stored or file shape carries a schema version and readers refuse an unknown version (A).
- Q6: every unit interface returns a typed result (success, or a named error code with a plain reason) and never throws across a unit boundary; retries only at outside edges; the coordinator gives each step a time limit (A).
- Q7: the bill-reader adapter returns a reader-neutral result per page (fields, lines and totals with confidence 0 to 1 and page position; unread lines; legibility; reader name and version), never logs page content, gives up on a page after 120 seconds and retries a failed page at most 3 times (A).
- Q8: the accuracy runner hands over one strict JSON file with a schema version, run details and per-bill and per-check scores; the import refuses any field the schema does not list (A).

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
