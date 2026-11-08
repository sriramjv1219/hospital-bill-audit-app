# Contract Summary — Hospital Bill Check (Phase 1)

This document pins down every boundary the product has to honour:
- **Between units:** the 12 units in `inception/units-generation/unit-of-work.md`, along the edges of `unit-of-work-dependency.md`.
- **With the outside:** the family's browser, the staff browser, the bill-reading service, the identity provider and the offline accuracy runner's output file.

For each boundary it gives what crosses, how, who owns the spec, and what happens on failure. Construction builds against these contracts, so units can be designed and built in parallel.

Entity names and fields come from `inception/domain-design/components.md`. Data types beyond what is needed at the boundary are set in Functional Design. Limits and targets come from `inception/requirements-analysis/requirements.md`.

## Sources

- Answers Q1 to Q8 and the follow-up Q2a in `contract-design-questions.md` in this folder, confirmed through the consolidated summary.
- Units and decisions D-UG1 to D-UG8: `inception/units-generation/unit-of-work.md`.
- Dependency edges and integration points: `inception/units-generation/unit-of-work-dependency.md`.
- Entities and component behaviour: `inception/domain-design/components.md`, and the decisions in `inception/domain-design/decisions.md` (ADR-002 deletion, ADR-003 coordinator, ADR-004 screen routes, ADR-005 adapter, ADR-006 checks and promotion, ADR-007 on-device PDF, ADR-009 withdrawal, ADR-014 beta hand-over).
- Limits and targets: `inception/requirements-analysis/requirements.md`, including FR2.3, FR2.8, FR2.10, FR4.4, FR4.9, FR11.3, NFR1, NFR2, NFR6, NFR11 and NFR19.

## Decisions taken in this stage

| ID | Decision | Source |
|---|---|---|
| D-CD1 | Every browser-to-server call, on the family side and in the internal tools, is a REST route handler described with OpenAPI. The internal tools use their own path prefix and sign-in | Q2 B, Q2a A |
| D-CD2 | The progress screen polls every 3 seconds. The progress endpoint is read-only and never counts as session activity | Q3 A; D-US13 |
| D-CD3 | Uploads use our own chunked protocol: fixed-size chunks (size in D-CD16), an offset query before resuming, and a SHA-256 checksum to complete each file | Q4 A; NFR11, D-US16 |
| D-CD4 | In-process contracts change in the same commit as every consumer, enforced by the type check. Adding a field is always allowed; removing or changing one needs a change note here. Every stored or file shape carries `schemaVersion`, and readers refuse an unknown version | Q5 A |
| D-CD5 | Every unit interface returns a typed `Result`: success, or a named error code with a plain reason. Nothing throws across a unit boundary. Retries happen only at outside edges, and the coordinator gives each step a time limit | Q6 A |
| D-CD6 | The bill-reader adapter returns a reader-neutral per-page result. It never logs page content, gives up on a page after 120 seconds and retries a failed page at most 3 times | Q7 A; ADR-005 |
| D-CD7 | The accuracy runner hands over one strict JSON file of IDs and scores. The import refuses any field the schema does not list | Q8 A; ADR-009 |
| D-CD8 | Every state-changing REST endpoint checks the `Origin` header against our own site, on top of the same-site session cookie, to block cross-site requests. This follows from D-CD1 with no other sensible choice | Q2 B (consequence) |
| D-CD9 | The carried review point R-01 is settled by a `ScheduledJob` interface in contracts. feedback-insights provides the 3-month deletion job, and case-processing's worker only registers and runs whatever jobs it is given | Q1 A |
| D-CD10 | The carried review point R-03 is settled by a `BetaHandoverSink` interface in contracts. test-set-quality implements it, and document-intake calls only the interface, so document-intake's code no longer depends on test-set-quality. The approved dependency map keeps its beta edge as a runtime link that ends at launch | Q1 A; ADR-014 |
| D-CD11 | Carried point R-02: NFR Requirements, NFR Design and Infrastructure Design cover the stores of bill-analysis (U4) and findings-report (U6), and U4's bill-reading service, exactly as they cover a service unit's, even though both units are tagged "library" | Q1 A |
| D-CD12 | Carried point R-04 goes to Delivery Planning: the first build slice cuts across several units and also owns repository scaffolding, test tooling and the automatic checks. R-05 stays accepted: the 8 Won't Have stories are deferred | Q1 A |
| D-CD13 | Every session ends through one operation, `endSession`, with a reason: `finished`, `idle` or `discarded`. Three triggers lead to it: "Discard everything" (`discarded`); the "I'm done" step on the before-you-go screen (`finished`); and a session-privacy sweep job that ends every session idle for 30 minutes (`idle`). The sweep runs every minute on the worker, so a closed phone still ends on time. The consented record is stored inside `endSession`, only for `finished` or `idle` with storage consent yes, and never for `discarded` | FR2.8, FR1.8, US7.2, AC7.2.6 |
| D-CD14 | Staff sign-in uses three routes outside the role-checked API: sign-in start, provider callback and sign-out. Every other internal route names the roles it allows | FR15.2, D-US4 |
| D-CD15 | Pages of one job are read in parallel, up to 6 at a time, within a read-step budget: 4 minutes for typed or PDF bills and 8 minutes for photographed bills. Within those budgets, each page's attempts stop after 6 minutes in total. The first look is ready once reading, the bill model and a first pass of checks on the lines as read are all done. These are the contract's starting values; NFR Design tunes them against the chosen reader's rate limits, but may not loosen NFR1 or NFR2 | NFR1, NFR2, D-US14 |
| D-CD16 | "25 MB" in FR4.4 means 25,000,000 bytes. Upload chunks are 4 MiB (4,194,304 bytes), under the 4.5 MB request-body limit of the working-assumption host, and are revisited when hosting is decided | FR4.4, OQ2 |

## Contracts

| # | Provider Unit | Consumer | Mechanism | Owner |
|---|---|---|---|---|
| C1 | family-ui (U8) | External: family's browser (public web) | REST over HTTPS, OpenAPI | family-ui |
| C2 | internal-ui (U11) | External: staff browser (builder and founder) | REST over HTTPS, OpenAPI | internal-ui |
| C3 | External: bill-reading service | bill-analysis (U4), through the adapter | Outbound adapter, shared schema | contracts (U1); each adapter implementation is in bill-analysis |
| C4 | External: identity provider | rules-access (U9) | Outbound sign-in, standard to be named in NFR Requirements | rules-access |
| C5 | accuracy-runner (U12) | test-set-quality (U10), through the internal tools | JSON file, shared schema | contracts (U1) |
| C6 | contracts (U1) | every unit | In-process shared schema: `Result`, error codes, IDs, `RuleSnapshot`, `SessionDataOwner`, `ScheduledJob`, `BetaHandoverSink` | contracts |
| C7 | session-privacy (U2) | family-ui (U8) | In-process interface | session-privacy |
| C8 | document-intake (U3) | family-ui (U8), session-privacy (U2), case-processing (U5) | In-process interface | document-intake |
| C9 | case-processing (U5) | family-ui (U8), session-privacy (U2) | In-process interface | case-processing |
| C10 | bill-analysis (U4) | case-processing (U5), family-ui (U8), session-privacy (U2), feedback-insights (U7), accuracy-runner (U12) | In-process interface | bill-analysis |
| C11 | findings-report (U6) | case-processing (U5), family-ui (U8), session-privacy (U2), feedback-insights (U7) | In-process interface | findings-report |
| C12 | feedback-insights (U7) | family-ui (U8), session-privacy (U2), test-set-quality (U10); its deletion job is run by case-processing's worker through C6 | In-process interface | feedback-insights |
| C13 | rules-access (U9) | case-processing (U5), findings-report (U6), test-set-quality (U10), internal-ui (U11), accuracy-runner (U12) | In-process interface; a rule-snapshot export file for the runner | rules-access |
| C14 | test-set-quality (U10) | internal-ui (U11); its `BetaHandoverSink` is called by document-intake (U3) through C6 | In-process interface | test-set-quality |

Every edge in the `unit-of-work-dependency.md` edge block is covered by one of C6 to C14. The "delete session data" calls from session-privacy to U3, U4, U5, U6 and U7 all go through `SessionDataOwner` (C6).

## Cross-Cutting Rules

These apply to every contract below.

- **Results, not exceptions (D-CD5).** Every operation in C6 to C14 returns `Result<T>`: either `{ ok: true, value }` or `{ ok: false, error: { code, reason } }`. Each contract lists its error codes. An unexpected failure inside a unit is returned as `INTERNAL`, with no bill content in `reason`.
- **Session scoping.** Every session-scoped operation takes a `sessionId` that has already been resolved and checked by family-ui through session-privacy (ADR-004). No unit reads the session cookie itself.
- **No bill content in logs or errors.** No `reason`, log line or error report contains page content, line descriptions, patient identifiers or amounts (FR2.3).
- **Money and dates.** Amounts cross every boundary as integer paise with currency INR. Dates cross as ISO 8601 calendar dates; times are shown in IST (NFR13). Functional Design may add types, but not change these two rules.
- **Pinned versions.** Any operation that depends on rules takes a `RuleSnapshot` (C6) and records its `ruleLibraryVersion` and `referenceDataVersions` (FR8.17, NFR6).
- **Time limits and retries.**

| Edge | Time limit | Retries | On final failure |
|---|---|---|---|
| Bill-reader adapter, per attempt (C3) | 120 s | up to 3, backoff 2 s, 4 s, 8 s with jitter; all attempts for one page stop at 6 min in total | page marked `READ_FAILED`; job continues with other pages; original still deleted (FR2.3) |
| Coordinator step: read (all pages of a job, up to 6 in parallel, D-CD15) | 4 min for typed or PDF bills; 8 min for photographed bills | none at step level (per-page retries above) | unread pages marked `READ_FAILED`; checks that need them record "could not run" (FR7.3) |
| Any store call inside a unit | 5 s | up to 3, backoff 100, 200, 400 ms | `STORE_UNAVAILABLE` returned to the caller |
| Coordinator step: build bill model | 60 s | none (deterministic) | step `FAILED`, retry offered (FR7.3) |
| Coordinator step: run checks | 60 s | none | step `FAILED`, retry offered |
| Coordinator step: build findings and report | 60 s | none | step `FAILED`, retry offered |
| Whole job | 15 min (NFR2) | — | job `TIMED_OUT`, completed steps kept, retry offered |
| Idle sweep (C7 `idleSweep`, D-CD13) | runs every 60 s | next run picks up anything missed | a missed run raises an alert; a session is never kept past 31 minutes idle |
| Session deletion, per owner (C6) | 10 s | up to 5, backoff 1 s doubling | alert raised; the session stays `ENDING` until every owner confirms (ADR-002) |
| Consented-record store at close (C12) | 10 s | up to 3 | record dropped, alert raised, deletion goes ahead (R-02 answer) |
| Beta hand-over (C6, C14) | 30 s | up to 3 | original deleted anyway, alert raised (R-03 answer) |

### Error code mapping

In-process operations return `{ code, reason }`. C1 and C2 return the same code with an HTTP status, and the response field is `message` (the plain reason, safe to show). Codes that never reach a browser are handled inside the server as shown.

| In-process code | Raised by | C1 or C2 code | HTTP |
|---|---|---|---|
| SESSION_INVALID | C7 | SESSION_INVALID | 401 |
| GATE_NOT_PASSED (consent or declarations missing) | C7 | GATE_NOT_PASSED | 403 |
| DECLARATION_INCOMPLETE | C7 | VALIDATION_FAILED | 422 |
| VALIDATION_FAILED | any | VALIDATION_FAILED | 422 |
| LIMIT_FILES, LIMIT_FILE_SIZE, LIMIT_PAGES | C8 | same | 409 (LIMIT_PAGES: 422 at completion) |
| UPLOAD_OFFSET_MISMATCH | C8 | same | 409 |
| CHECKSUM_MISMATCH | C8 | same | 422 |
| PASSWORD_REQUIRED, PASSWORD_WRONG | C8 | same | 422 |
| JOB_RUNNING | C9 | same | 409 |
| NOT_FOUND | any | same | 404 |
| STORE_UNAVAILABLE | any | same | 503 |
| READ_FAILED, READ_TIMEOUT, READ_REJECTED | C3, C10 | not an HTTP error: the page shows as failed in progress, and checks needing it show as "could not run" | — |
| CANCELLED | C9 | not an HTTP error: the session has ended, so the next request gets SESSION_INVALID | 401 |
| STORE_FAILED (consented record) | C12 | not an HTTP error: the record is dropped and an alert raised (D-CD13) | — |
| Job past 15 minutes | C9 | not an HTTP error: progress shows `timed-out` with a retry | — |
| NOT_SIGNED_IN | C13 | same | 401 |
| ROLE_DENIED | C13 | same | 403 |
| MISSING_PROVENANCE | C13 | same | 422 |
| PROMOTION_REQUIRED | C13 | PROMOTION_GATE_FAILED | 422 |
| PROMOTION_GATE_FAILED | C14 | same | 422 |
| SCHEMA_VERSION_UNKNOWN, UNLISTED_FIELD | C13, C14 | same | 422 |
| ORIGIN_REJECTED | route layer | same | 403 |
| INTERNAL, or any code a consumer does not know | any | INTERNAL | 500 |

## C1 Family public web API (family-ui)

All routes are route handlers under `/api/`. They are rendered per request and answer with `Cache-Control: no-store` (FR2.10). Every route resolves the session cookie through session-privacy and refuses with `401 SESSION_INVALID` when the session is missing or ended. Routes that need consent or declarations refuse with `403 GATE_NOT_PASSED` until those are recorded (US1.4 to US1.7). State-changing routes refuse a request whose `Origin` is not our own site with `403 ORIGIN_REJECTED` (D-CD8). Only real user actions count as activity; `GET /api/progress` never does (D-CD2, D-US13).

```yaml
openapi: 3.1.0
info:
  title: Hospital Bill Check - family API
  version: 1.0.0
servers:
  - url: /api
components:
  securitySchemes:
    sessionCookie:
      type: apiKey
      in: cookie
      name: hbc_session   # HttpOnly, Secure, SameSite=Lax; never in a URL (US7.9)
  schemas:
    Error:
      type: object
      required: [code, message]
      properties:
        code:
          type: string
          enum: [SESSION_INVALID, GATE_NOT_PASSED, ORIGIN_REJECTED, VALIDATION_FAILED, LIMIT_FILES, LIMIT_FILE_SIZE, LIMIT_PAGES, UPLOAD_OFFSET_MISMATCH, CHECKSUM_MISMATCH, PASSWORD_REQUIRED, PASSWORD_WRONG, JOB_RUNNING, NOT_FOUND, STORE_UNAVAILABLE, INTERNAL]
        message: { type: string, description: plain-language text safe to show; never contains bill content }
        limit: { type: integer, description: present for LIMIT_* codes, the limit that was hit }
    Relationship:
      type: string
      enum: [self, spouse, parent, child, other-family, authorised-representative, professional]
    BillStage:
      type: string
      enum: [still-in-hospital, at-discharge, already-paid]
    Progress:
      type: object
      required: [jobState, steps]
      properties:
        jobState: { type: string, enum: [queued, running, completed, failed, timed-out, cancelled] }
        steps:
          type: array
          items:
            type: object
            properties:
              step: { type: string, enum: [read, bill-model, checks, findings, report] }
              state: { type: string, enum: [pending, running, done, failed] }
        reportReady: { type: boolean }
        tooLong: { type: boolean, description: true when the job is past its expected time; drives the "This is taking too long" state (R-17) }
        firstLookReady: { type: boolean, description: "true once reading, the bill model and a first checks pass on the lines as read are done (D-CD15)" }
    Paise: { type: integer, description: money in paise, INR }
    ConfidentValue:
      type: object
      required: [value, confidence, confirmed]
      properties:
        value: { type: string, description: as shown to the user; dates are ISO 8601 }
        confidence: { type: number, minimum: 0, maximum: 1 }
        confirmed: { type: boolean }
    Case:
      type: object
      properties:
        hospitalName: { $ref: '#/components/schemas/ConfidentValue' }
        hospitalCity: { $ref: '#/components/schemas/ConfidentValue' }
        admissionDate: { $ref: '#/components/schemas/ConfidentValue' }
        dischargeDate: { $ref: '#/components/schemas/ConfidentValue' }
        treatmentType: { $ref: '#/components/schemas/ConfidentValue' }   # planned | emergency
        paymentMode: { $ref: '#/components/schemas/ConfidentValue' }     # cashless | reimbursement | self-pay
        insurerOrTpa: { $ref: '#/components/schemas/ConfidentValue' }
        billStage: { $ref: '#/components/schemas/BillStage' }
        billStageMismatch: { type: boolean, description: documents disagree with the stated bill stage (AC2.11.3) }
    CaseConfirmation:
      type: object
      description: only the fields the user confirms or corrects; unknown fields are refused
      properties:
        hospitalName: { type: string }
        hospitalCity: { type: string }
        admissionDate: { type: string, format: date }
        dischargeDate: { type: string, format: date }
        treatmentType: { type: string, enum: [planned, emergency] }
        paymentMode: { type: string, enum: [cashless, reimbursement, self-pay] }
        insurerOrTpa: { type: string }
        billStage: { $ref: '#/components/schemas/BillStage' }
    Line:
      type: object
      required: [lineId, originalDescription, category, netAmount, confirmed]
      properties:
        lineId: { type: string }
        sourcePage: { type: integer }
        originalDescription: { type: string }
        standardDescription: { type: string }
        category: { type: string, description: one of the 21 categories (FR6.7) }
        serviceDate: { $ref: '#/components/schemas/ConfidentValue' }
        quantity: { $ref: '#/components/schemas/ConfidentValue' }
        unitRate: { $ref: '#/components/schemas/ConfidentValue' }
        netAmount: { $ref: '#/components/schemas/ConfidentValue' }
        confirmed: { type: boolean }
    LineCorrection:
      type: object
      required: [field, newValue]
      properties:
        field: { type: string, enum: [description, serviceDate, quantity, unitRate, grossAmount, discount, tax, netAmount, category] }
        newValue: { type: string, description: amounts in paise as a decimal string; dates ISO 8601 }
    Checklist:
      type: object
      properties:
        present: { type: array, items: { type: string } }
        missing: { type: array, items: { type: string } }
        optional: { type: array, items: { type: string } }
        pagePrompts:
          type: array
          items:
            type: object
            properties:
              fileId: { type: string }
              pageNumber: { type: integer }
              prompt: { type: string, enum: [missing-page, duplicate-page, out-of-order, unreadable, not-a-hospital-document, summary-bill] }
    FirstLook:
      type: object
      properties:
        count: { type: integer }
        topics: { type: array, items: { type: string } }
    PlanAnswer:
      type: object
      required: [answer]
      properties:
        answer: { type: string, description: one of the fixed plan choices set in Functional Design }
security:
  - sessionCookie: []
paths:
  /session:
    post:
      summary: Start a session (sets the cookie). Idempotent for an existing live session.
      security: []
      responses: { '201': { description: session started }, '200': { description: existing session kept } }
    delete:
      summary: Discard everything (FR1.8). endSession with reason discarded; nothing is kept; the cookie is cleared.
      responses: { '202': { description: deletion started }, '401': { description: SESSION_INVALID } }
  /session/finish:
    post:
      summary: The "I'm done" step on the before-you-go screen. endSession with reason finished; the consented record is stored first when storage consent is yes (D-CD13)
      responses: { '202': { description: session ending }, '401': { description: SESSION_INVALID } }
  /session/keep-alive:
    post:
      summary: The "Keep going" button at 25 minutes; resets the idle timer (FR2.8)
      responses: { '204': { description: timer reset } }
  /session/status:
    get:
      summary: Idle state for the warning; read-only and not activity
      responses:
        '200':
          content:
            application/json:
              schema: { type: object, properties: { idleSeconds: { type: integer }, warn: { type: boolean }, paused: { type: boolean } } }
  /consent:
    post:
      summary: Record notice shown and processing consent, or decline (FR1.2, FR1.3, FR1.11)
      requestBody:
        content:
          application/json:
            schema: { type: object, required: [noticeShown, processingConsent], properties: { noticeShown: { type: boolean }, processingConsent: { type: boolean }, betaConsent: { type: boolean, description: closed beta only } } }
      responses: { '204': { description: recorded }, '422': { description: VALIDATION_FAILED } }
  /declaration:
    post:
      summary: Relationship and authority declarations (FR1.5 to FR1.7)
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [relationship, authorisedToShare]
              properties:
                relationship: { $ref: '#/components/schemas/Relationship' }
                authorisedToShare: { type: boolean }
                patientIsMinor: { type: boolean }
                guardianDeclared: { type: boolean }
                adultConsentOrAuthorityDeclared: { type: boolean }
                patientDeceased: { type: boolean }
      responses: { '204': { description: recorded }, '422': { description: VALIDATION_FAILED, a required declaration is missing } }
  /storage-consent:
    post:
      summary: The optional storage choice after the report (FR1.4)
      requestBody:
        content: { application/json: { schema: { type: object, required: [keep], properties: { keep: { type: boolean } } } } }
      responses: { '204': { description: recorded } }
  /case/bill-stage:
    put:
      summary: Answer "When are you checking?" (US2.11)
      requestBody:
        content: { application/json: { schema: { type: object, required: [billStage], properties: { billStage: { $ref: '#/components/schemas/BillStage' } } } } }
      responses: { '204': { description: recorded }, '422': { description: VALIDATION_FAILED } }
  /uploads:
    post:
      summary: Announce a file before sending chunks; checks the file, size and page limits (FR4.4)
      requestBody:
        content:
          application/json:
            schema: { type: object, required: [fileName, sizeBytes, sha256, mediaType], properties: { fileName: { type: string }, sizeBytes: { type: integer, minimum: 1, description: "no schema maximum; over 25,000,000 bytes returns LIMIT_FILE_SIZE (D-CD16)" }, sha256: { type: string }, mediaType: { type: string } } }
      responses:
        '201':
          content: { application/json: { schema: { type: object, properties: { fileId: { type: string }, chunkSize: { type: integer, const: 4194304 } } } } }
        '409': { description: LIMIT_FILES (more than 20 files) or LIMIT_FILE_SIZE (over 25,000,000 bytes), with the limit named }
  /uploads/{fileId}:
    head:
      summary: How many bytes the server already has (resume after a drop or reload, D-US16)
      responses: { '200': { description: "header Upload-Offset carries the received byte count" } }
    patch:
      summary: Send the next chunk at the given offset
      parameters:
        - { name: Upload-Offset, in: header, required: true, schema: { type: integer } }
      requestBody: { content: { application/octet-stream: {} } }
      responses: { '204': { description: chunk stored }, '409': { description: UPLOAD_OFFSET_MISMATCH } }
    delete:
      summary: Remove a file (for example one flagged "not a bill")
      responses: { '204': { description: removed } }
  /uploads/{fileId}/complete:
    post:
      summary: Finish a file; the server checks the SHA-256 against the announced value
      responses: { '200': { description: file accepted }, '422': { description: "CHECKSUM_MISMATCH, LIMIT_PAGES (more than 60 pages in the session), or PASSWORD_REQUIRED (a protected PDF; the screen then asks for the password)" } }
  /uploads/{fileId}/password:
    post:
      summary: Unlock a protected PDF; the password is used once and never stored or logged (FR4.9)
      requestBody: { content: { application/json: { schema: { type: object, required: [password], properties: { password: { type: string } } } } } }
      responses: { '204': { description: unlocked }, '422': { description: PASSWORD_WRONG } }
  /uploads/{fileId}/type:
    put:
      summary: Correct a file's document type (FR4.5)
      requestBody: { content: { application/json: { schema: { type: object, required: [documentType], properties: { documentType: { type: string } } } } } }
      responses: { '204': { description: recorded } }
  /checklist:
    get:
      summary: Present, missing and optional documents, plus page prompts (FR4.6, FR4.7, FR4.10)
      responses:
        '200':
          content: { application/json: { schema: { $ref: '#/components/schemas/Checklist' } } }
  /processing:
    post:
      summary: Start processing (or re-run after an added document); queued in arrival order (US3.9)
      responses: { '202': { description: queued }, '409': { description: JOB_RUNNING } }
  /processing/retry:
    post:
      summary: Retry from the failed step (FR7.3)
      responses: { '202': { description: queued } }
  /progress:
    get:
      summary: Poll every 3 seconds; read-only; never counts as activity (D-CD2)
      responses:
        '200':
          content: { application/json: { schema: { $ref: '#/components/schemas/Progress' } } }
  /case:
    get:
      summary: Case details read from the documents, for confirmation (FR3.2)
      responses:
        '200':
          content: { application/json: { schema: { $ref: '#/components/schemas/Case' } } }
    put:
      summary: Confirm or correct case details; re-runs from the check step
      requestBody:
        content: { application/json: { schema: { $ref: '#/components/schemas/CaseConfirmation' } } }
      responses: { '202': { description: confirmed, re-run queued }, '409': { description: JOB_RUNNING }, '422': { description: VALIDATION_FAILED } }
  /lines:
    get:
      summary: Lines as read, unread lines, and items to confirm (FR5.6 to FR5.8, FR7.4)
      responses:
        '200':
          content:
            application/json:
              schema:
                type: object
                properties:
                  lines: { type: array, items: { $ref: '#/components/schemas/Line' } }
                  unreadLines: { type: array, items: { type: object, properties: { sourcePage: { type: integer }, reason: { type: string } } } }
                  toConfirm: { type: array, items: { type: string }, description: lineIds with low-confidence fields }
  /lines/{lineId}:
    patch:
      summary: Fix a line; recorded as a correction and re-runs from the check step (FR5.8)
      requestBody:
        content: { application/json: { schema: { $ref: '#/components/schemas/LineCorrection' } } }
      responses: { '202': { description: corrected, re-run queued }, '404': { description: NOT_FOUND }, '409': { description: JOB_RUNNING }, '422': { description: VALIDATION_FAILED } }
  /first-look:
    get:
      summary: Number of things worth a look and their topics only (D-US14)
      responses:
        '200':
          content: { application/json: { schema: { $ref: '#/components/schemas/FirstLook' } } }
        '409': { description: JOB_RUNNING, first look not ready }
  /report:
    get:
      summary: Report content for on-device rendering and PDF (FR10.1, FR10.2; ADR-007). The ReportContent schema is owned by findings-report and is fixed in its Functional Design, before family-ui's Code Generation starts
      responses: { '200': { description: ReportContent with rule and reference versions }, '409': { description: JOB_RUNNING, report not ready } }
  /findings/{findingId}/mark:
    put:
      summary: Will ask / Not pursuing (FR11.1)
      requestBody: { content: { application/json: { schema: { type: object, required: [mark], properties: { mark: { type: string, enum: [will-ask, not-pursuing] } } } } } }
      responses: { '204': { description: recorded } }
  /findings/{findingId}/rating:
    put:
      summary: Rate a finding with a fixed reason (FR12.1, D-US19)
      requestBody: { content: { application/json: { schema: { type: object, required: [rating], properties: { rating: { type: string, enum: [helpful, not-helpful, wrong] }, fixedReason: { type: string } } } } } }
      responses: { '204': { description: recorded } }
  /letters/{letterType}:
    get:
      summary: Generate a letter for the current findings; returned, never stored (FR9.4, ADR-007)
      responses: { '200': { description: letter text } }
  /plan:
    put:
      summary: '"What do you plan to do next" (FR12.2)'
      requestBody:
        content: { application/json: { schema: { $ref: '#/components/schemas/PlanAnswer' } } }
      responses: { '204': { description: recorded }, '422': { description: VALIDATION_FAILED } }
  /insurance-interest:
    post:
      summary: Add one to the anonymous Insurance Check total; counted once per session (FR12.4)
      responses: { '204': { description: counted or already counted } }
```

Not in C1 on purpose: no route returns an original file, and no route takes a report PDF back (ADR-007). No route or link carries a session or case ID in the URL (US7.9). No route sends anything to a hospital or insurer (FR11.3).

## C2 Internal tools API (internal-ui)

All routes sit under `/internal/api/`. The three `/auth/` routes are the only ones reachable without a staff session (D-CD14). Every other route requires a signed-in staff session and one of the roles in its `x-roles` list, checked through rules-access before any data is read. The roles are `ops`, `domain-admin` and `support` (FR15.2). Each access to test-set data is written to the access log (FR15.2). The Origin check (D-CD8) and `Cache-Control: no-store` apply as in C1. There is no route that searches for or opens a family's case (FR15.3).

```yaml
openapi: 3.1.0
info:
  title: Hospital Bill Check - internal tools API
  version: 1.0.0
servers:
  - url: /internal/api
components:
  securitySchemes:
    staffSession:
      type: apiKey
      in: cookie
      name: hbc_staff   # separate from the family cookie; issued after sign-in (C4)
  schemas:
    Error:
      type: object
      required: [code, message]
      properties:
        code: { type: string, enum: [NOT_SIGNED_IN, ROLE_DENIED, ORIGIN_REJECTED, VALIDATION_FAILED, MISSING_PROVENANCE, NOT_APPROVED, PROMOTION_GATE_FAILED, SCHEMA_VERSION_UNKNOWN, UNLISTED_FIELD, NOT_FOUND, STORE_UNAVAILABLE, INTERNAL] }
        message: { type: string }
security:
  - staffSession: []
paths:
  /auth/sign-in:
    get:
      summary: Start staff sign-in; redirects to the identity provider (C4). No staff session needed
      security: []
      responses: { '302': { description: redirect to the provider } }
  /auth/callback:
    get:
      summary: Receives the provider's response, verifies it, calls rules-access signIn (C13) and sets the hbc_staff cookie
      security: []
      responses: { '302': { description: signed in, redirect to the tools }, '401': { description: NOT_SIGNED_IN } }
  /auth/sign-out:
    post:
      summary: End the staff session and clear the hbc_staff cookie
      responses: { '204': { description: signed out } }
  /rules/checks:
    get: { summary: "List checks with state and version", x-roles: [ops, domain-admin], responses: { '200': { description: checks } } }
  /rules/checks/{checkId}/state:
    put:
      summary: "Switch a check to shadow or off. Live is refused here; use /promotions (ADR-006)"
      x-roles: [ops]
      requestBody: { content: { application/json: { schema: { type: object, required: [state], properties: { state: { type: string, enum: [shadow, off] } } } } } }
      responses: { '202': { description: change proposed, awaits approval }, '422': { description: VALIDATION_FAILED } }
  /rules/reference-items:
    get: { summary: "Reference data", x-roles: [domain-admin, ops], responses: { '200': { description: items } } }
    post:
      summary: "Propose a new or changed reference item; refused without source, effective dates, version, owner and review date (FR14.2)"
      x-roles: [domain-admin]
      responses: { '202': { description: change proposed }, '422': { description: MISSING_PROVENANCE } }
  /rules/changes:
    get: { summary: "Pending and past rule changes with who, what, why and source (NFR15)", x-roles: [ops, domain-admin], responses: { '200': { description: changes } } }
  /rules/changes/{changeId}/approve:
    post:
      summary: "Approve a change; self-approval allowed and logged (D-US18); publishes a new library version"
      x-roles: [ops, domain-admin]
      responses: { '200': { description: approved, new library version }, '409': { description: NOT_APPROVED, change already closed } }
  /rules/snapshot-export:
    get: { summary: "Download a rule snapshot file for the offline runner (C13)", x-roles: [ops], responses: { '200': { description: snapshot file with schemaVersion } } }
  /test-bills:
    get: { summary: "List test bills with source and coverage fields, never content", x-roles: [ops, domain-admin], responses: { '200': { description: bills } } }
    post: { summary: "Register a consented or synthetic test bill (FR16.2)", x-roles: [ops], responses: { '201': { description: registered } } }
  /test-bills/{testBillId}/labels:
    post:
      summary: "Submit one labeller's labels; the other labeller's labels and any AI output are never returned (FR16.7)"
      x-roles: [ops, domain-admin]
      responses: { '201': { description: stored } }
  /test-bills/{testBillId}/reconciliation:
    post: { summary: "Record the reconciliation with its reason (FR16.5)", x-roles: [ops, domain-admin], responses: { '201': { description: stored } } }
  /test-bills/{testBillId}/withdrawal:
    post:
      summary: "Withdraw a contributor's bill: delete original, labels and reconciliation; clear caches; record 30-day backup expiry (ADR-009)"
      x-roles: [ops]
      responses: { '202': { description: withdrawal recorded } }
  /accuracy-runs:
    post:
      summary: "Import an accuracy-runner result file (C5); refused on unknown schemaVersion or any unlisted field"
      x-roles: [ops]
      requestBody: { content: { application/json: {} } }
      responses: { '201': { description: imported }, '422': { description: SCHEMA_VERSION_UNKNOWN or UNLISTED_FIELD } }
  /promotions:
    post:
      summary: "Promote a check to live; needs a run with at least 10 bills with the error, 30 without, and the precision target met (FR16.12)"
      x-roles: [ops]
      requestBody: { content: { application/json: { schema: { type: object, required: [checkId, accuracyRunId], properties: { checkId: { type: string }, accuracyRunId: { type: string } } } } } }
      responses: { '201': { description: promotion record created, check set live }, '422': { description: PROMOTION_GATE_FAILED } }
  /dashboard:
    get: { summary: "Reading accuracy, precision by check, missed-issue rate, ratings from consented records (FR15.4)", x-roles: [ops, domain-admin], responses: { '200': { description: dashboard data } } }
  /support-material:
    get: { summary: "Support material explaining data practices; no case lookup (FR15.3)", x-roles: [support, ops, domain-admin], responses: { '200': { description: support material } } }
  /access-log:
    get: { summary: "Access log entries", x-roles: [ops], responses: { '200': { description: entries } } }
```

## C3 Bill-reader adapter (outbound)

The only path for bill content to leave our system (ADR-005). An adapter implementation lives in bill-analysis, one per reading service, plus the recorded-output adapter for tests. Only services that have passed the data-processor check, under a no-training term, may be wired in (NFR19).

```yaml
shared-schema: BillReaderAdapter
schemaVersion: 1
owner: contracts
operation:
  name: readPage
  input:
    pageImage: bytes            # one page, already unlocked; never logged
    pageNumber: integer
    documentType: string        # from document-intake's classification
  output: "Result<PageReading>"
  timeLimitSeconds: 120
  retries: 3                    # backoff 2 s, 4 s, 8 s with jitter
  errors: [READ_TIMEOUT, READ_REJECTED, SERVICE_UNAVAILABLE, INTERNAL]
types:
  PageReading:
    readerName: string
    readerVersion: string
    pageNumber: integer
    pageOfPage: { current: "integer?", total: "integer?" }   # e.g. "Page 3 of 7" when printed
    legibility: number          # 0 to 1
    headerFields: [ReadValue]   # each with a field name, e.g. hospitalName, uhid, admissionDate
    lineItems: [ReadLine]
    totals: [ReadValue]
    dischargeSummaryFields: [ReadValue]
    unreadLines: [ { position: Position, reason: string } ]   # e.g. Hindi or regional script (FR5.6)
  ReadValue:
    field: string
    text: string                # as printed
    confidence: number          # 0 to 1 (FR5.7)
    position: Position
  ReadLine:
    cells: [ReadValue]          # description, code, quantity, unit, rate, gross, discount, tax, net, date, section
    position: Position
  Position:
    page: integer
    box: { x: number, y: number, w: number, h: number }   # fraction of page size
rules:
  - The adapter never writes page content, values or images to logs, error reports or temporary files.
  - A page that still fails after 3 retries is returned as READ_REJECTED or READ_TIMEOUT; the coordinator marks it READ_FAILED and continues.
  - Lakh and crore grouping is returned as printed text; parsing to paise happens in bill-analysis, not the adapter.
```

## C4 Identity provider (outbound sign-in)

rules-access signs staff in through an external identity provider (D-US4). The standard, provider and second factor are set in NFR Requirements. The contract fixed now:
- the provider returns a verified staff identity and nothing else;
- rules-access maps that identity to a `StaffAccount` and its roles, and issues the `hbc_staff` cookie (C2);
- the provider never receives family data.

## C5 Accuracy runner result file

Written on the offline machine by accuracy-runner (U12). It is carried to the internal tools and imported through `POST /internal/api/accuracy-runs` (C2). Only IDs and scores leave that machine (D-UG6, ADR-009).

```yaml
shared-schema: AccuracyRunResult
schemaVersion: 1
owner: contracts
additionalProperties: false     # the import refuses any field not listed here (D-CD7)
fields:
  schemaVersion: integer        # must equal a version the importer knows
  runId: string
  ranAt: string                 # ISO 8601 date-time
  readerName: string
  readerVersion: string
  promptOrModelVersion: string
  ruleLibraryVersion: string
  referenceDataVersions: { <list>: string }
  serviceApproved: boolean      # the reading service had passed the data-processor check
  perBill:
    - testBillId: string
      lineAccuracy: number      # 0 to 1
      format: { enum: [pdf, photo] }
  lineAccuracyPdf: number
  lineAccuracyPhoto: number
  perCheck:
    - checkId: string
      billsWithError: integer
      billsWithout: integer
      precision: number
      missedIssueRate: number
  coverage: { byState: object, byHospitalType: object, byProcedure: object, byFormat: object, byCaptureQuality: object }   # counts only
```

## C6 Shared contracts (contracts unit)

```yaml
shared-schema: Contracts
schemaVersion: 1
owner: contracts
types:
  "Result<T>":
    oneOf:
      - { ok: true, value: T }
      - { ok: false, error: { code: string, reason: string } }   # reason is plain and never carries bill content
  CommonErrorCodes: [NOT_FOUND, VALIDATION_FAILED, STORE_UNAVAILABLE, CANCELLED, INTERNAL]
  SessionId: string             # opaque, unguessable; resolved only by session-privacy
  Paise: integer                # money in paise, INR
  RuleSnapshot:
    schemaVersion: integer
    ruleLibraryVersion: string
    referenceDataVersions: { <list>: string }
    checks: [ { checkId: string, state: { enum: [live, shadow, off] }, thresholds: object } ]
    referenceItems: object
    catalogue: object
    labellingSettings: { complexCategories: [string], medicalNecessityIsComplex: boolean, complexValueThreshold: "Paise?" }
interfaces:
  SessionDataOwner:             # implemented by U3, U4, U5, U6, U7 (ADR-002; R-05 answer)
    deleteSessionData: { input: SessionId, output: "Result<{ deleted: true }>" }
    listSessionKeyedEntities: { output: [string] }   # used by the test that checks the deletion list is complete
  ScheduledJob:                 # R-01 answer (D-CD9); implemented by feedback-insights (3-month deletion, daily) and session-privacy (idle sweep, every 60 s, D-CD13)
    name: string
    schedule: string            # e.g. daily
    run: { output: "Result<{ processed: integer }>" }
  BetaHandoverSink:             # R-03 answer (D-CD10); implemented by test-set-quality; beta only
    acceptBetaOriginal: { input: { file: bytes, documentType: string, betaConsentId: string }, output: "Result<{ testBillId: string }>" }
```

## C7 to C14 In-process interfaces

Every operation returns `Result<T>` (C6), and every session-scoped operation takes a `SessionId` that family-ui has already checked. The error codes listed are added to the common ones.

```yaml
shared-schema: UnitInterfaces
schemaVersion: 1
C7_session-privacy:
  startSession: { output: "Result<{ sessionId: SessionId, isNew: boolean }>" }
  resolveSession: { input: { cookieValue: string }, output: "Result<{ sessionId: SessionId, gates: { noticeShown: boolean, processingConsent: boolean, declarationsComplete: boolean } }>", errors: [SESSION_INVALID] }
  recordConsent: { input: { sessionId, noticeShown, processingConsent, "betaConsent?" }, output: "Result<void>" }
  recordDeclaration: { input: { sessionId, relationship, authorisedToShare, "patientIsMinor?", "guardianDeclared?", "adultConsentOrAuthorityDeclared?", "patientDeceased?" }, output: "Result<void>", errors: [DECLARATION_INCOMPLETE] }
  recordStorageConsent: { input: { sessionId, keep: boolean }, output: "Result<void>" }
  touchActivity: { input: SessionId, output: "Result<void>" }          # real taps and typing only
  idleStatus: { input: SessionId, output: "Result<{ idleSeconds, warn, paused }>" }
  endSession:                    # the one way a session ends (D-CD13)
    input: { sessionId, reason: { enum: [finished, idle, discarded] } }
    output: "Result<{ ending: true }>"
    steps: [cancel the job (C9), store the consented record (C12) only when reason is finished or idle and storage consent is yes, ask every SessionDataOwner to delete, mark ended once all confirm]
  discardEverything: { input: SessionId, output: "Result<{ deletionStarted: true }>" }   # endSession with reason discarded
  idleSweep: ScheduledJob        # every 60 s on the worker: endSession(idle) for every session idle 30 min, skipping sessions whose report is being prepared (D-US13)
  implements: [ScheduledJob]
C8_document-intake:
  announceFile: { input: { sessionId, fileName, sizeBytes, sha256, mediaType }, output: "Result<{ fileId, chunkSize }>", errors: [LIMIT_FILES, LIMIT_FILE_SIZE] }
  receivedOffset: { input: { sessionId, fileId }, output: "Result<{ offset: integer }>" }
  appendChunk: { input: { sessionId, fileId, offset, bytes }, output: "Result<void>", errors: [UPLOAD_OFFSET_MISMATCH] }
  completeFile: { input: { sessionId, fileId }, output: "Result<void>", errors: [CHECKSUM_MISMATCH, LIMIT_PAGES, PASSWORD_REQUIRED] }
  unlockPdf: { input: { sessionId, fileId, password }, output: "Result<void>", errors: [PASSWORD_WRONG] }
  setDocumentType: { input: { sessionId, fileId, documentType }, output: "Result<void>" }
  setBillStage: { input: { sessionId, billStage }, output: "Result<void>" }
  checklist: { input: SessionId, output: "Result<{ present, missing, optional, pagePrompts }>" }
  pagesToProcess: { input: SessionId, output: "Result<[{ fileId, pageId, pageNumber, documentType }]>" }   # for case-processing
  pageImage: { input: { sessionId, pageId }, output: "Result<bytes>" }                                     # for case-processing only
  recordPageQuality: { input: { sessionId, pageId, legibility, pageOfPage }, output: "Result<void>" }
  releaseOriginal: { input: { sessionId, fileId }, output: "Result<{ deleted: true }>" }   # after extraction, success or failure (FR2.3); beta sessions go through BetaHandoverSink first
  implements: [SessionDataOwner]
C9_case-processing:
  startJob: { input: { sessionId, from: { enum: [read, checks] }, "onlyFileId?" }, output: "Result<{ jobId }>", errors: [JOB_RUNNING] }
  retryJob: { input: SessionId, output: "Result<{ jobId }>" }
  progress: { input: SessionId, output: "Result<Progress>" }   # same shape as C1 Progress
  isPreparing: { input: SessionId, output: "Result<boolean>" }  # pauses the idle timer (D-US13)
  cancel: { input: SessionId, output: "Result<void>" }
  registerScheduledJob: { input: ScheduledJob, output: "Result<void>" }   # worker start-up wiring (D-CD9)
  implements: [SessionDataOwner]
C10_bill-analysis:
  readPage: { input: { sessionId, pageId, pageImage, documentType, adapter }, output: "Result<ExtractionResult>", errors: [READ_FAILED] }
  buildBillModel: { input: { sessionId, snapshot: RuleSnapshot, billStage }, output: "Result<{ caseId, consistencyIssues }>" }
  applyCaseConfirmation: { input: { sessionId, fields }, output: "Result<void>" }
  applyLineCorrection: { input: { sessionId, lineId, field, newValue }, output: "Result<{ correctionId }>" }
  runChecks: { input: { sessionId, snapshot: RuleSnapshot }, output: "Result<{ checkRunId, outcomes: [CheckOutcome] }>" }   # deterministic for same lines and snapshot (NFR6)
  runChecksOnLabelled: { input: { labelledBill, snapshot: RuleSnapshot }, output: "Result<[CheckOutcome]>" }   # accuracy-runner only; no session
  viewCase: { input: SessionId, output: "Result<Case>" }
  viewLines: { input: SessionId, output: "Result<{ lines, unreadLines, toConfirm }>" }
  consentedExtract: { input: SessionId, output: "Result<{ case, lines, clinicalSummary }>" }   # for feedback-insights at close
  implements: [SessionDataOwner]
C11_findings-report:
  buildFindings: { input: { sessionId, outcomes: [CheckOutcome], snapshot: RuleSnapshot }, output: "Result<{ findingIds }>" }
  buildReport: { input: { sessionId, snapshot: RuleSnapshot }, output: "Result<{ reportId, revision }>" }
  firstLook: { input: SessionId, output: "Result<{ count, topics }>" }   # no class label, no rupee total (D-US14)
  reportContent: { input: SessionId, output: "Result<ReportContent>" }
  setMark: { input: { sessionId, findingId, mark }, output: "Result<void>" }
  generateLetter: { input: { sessionId, letterType }, output: "Result<{ text }>" }   # never stored (ADR-007)
  checkWording: { input: { text }, output: "Result<{ passes: boolean, problems }>" }   # the wording guard (FR13.2)
  consentedExtract: { input: SessionId, output: "Result<{ findings }>" }
  implements: [SessionDataOwner]
C12_feedback-insights:
  rateFinding: { input: { sessionId, findingId, rating, "fixedReason?" }, output: "Result<void>" }
  recordPlan: { input: { sessionId, answer }, output: "Result<void>" }
  addInsuranceInterest: { input: SessionId, output: "Result<{ counted: boolean }>" }   # counted once per session
  storeConsentedRecord: { input: SessionId, output: "Result<{ stored: true }>", errors: [STORE_FAILED] }   # called only by session-privacy's endSession (finished or idle, consent yes); failure means drop and alert (R-02 answer)
  groupedRatings: { output: "Result<object>" }   # consented records only (FR12.3), for test-set-quality
  deletionJob: ScheduledJob   # 3-month deletion, run by the worker (D-CD9)
  implements: [SessionDataOwner]
C13_rules-access:
  pinSnapshot: { output: "Result<RuleSnapshot>" }   # latest approved library version
  snapshotAt: { input: { ruleLibraryVersion }, output: "Result<RuleSnapshot>" }
  exportSnapshotFile: { output: "Result<bytes>" }   # for the offline runner
  loadSnapshotFile: { input: bytes, output: "Result<RuleSnapshot>", errors: [SCHEMA_VERSION_UNKNOWN] }   # used by accuracy-runner
  grievanceTimelines: { input: { ruleLibraryVersion }, output: "Result<object>" }   # for findings-report
  proposeChange: { input: { staffId, target, after, why, source }, output: "Result<{ changeId }>", errors: [MISSING_PROVENANCE] }
  approveChange: { input: { staffId, changeId }, output: "Result<{ ruleLibraryVersion }>" }
  setCheckLive: { input: { checkId, promotionRecordId }, output: "Result<void>", errors: [PROMOTION_REQUIRED] }   # only test-set-quality calls this
  signIn: { input: { verifiedIdentity }, output: "Result<{ staffId, roles }>", errors: [NOT_SIGNED_IN] }
  requireRole: { input: { staffId, role }, output: "Result<void>", errors: [ROLE_DENIED] }
  logAccess: { input: { staffId, action, targetType, targetId }, output: "Result<void>" }
C14_test-set-quality:
  registerTestBill: { input: { staffId, source, consent, coverageFields }, output: "Result<{ testBillId }>" }
  submitLabels: { input: { staffId, testBillId, labels }, output: "Result<void>" }   # never returns the other labeller's labels
  reconcile: { input: { staffId, testBillId, resolution, reason }, output: "Result<void>" }
  withdraw: { input: { staffId, testBillId }, output: "Result<{ withdrawalId }>" }
  importAccuracyRun: { input: { staffId, file: AccuracyRunResult }, output: "Result<{ accuracyRunId }>", errors: [SCHEMA_VERSION_UNKNOWN, UNLISTED_FIELD] }
  promote: { input: { staffId, checkId, accuracyRunId }, output: "Result<{ promotionId }>", errors: [PROMOTION_GATE_FAILED] }
  dashboard: { input: { staffId }, output: "Result<object>" }
  implements: [BetaHandoverSink]   # beta only; wired at start-up, removed at launch
```

## Contract Ownership Rules

- **Owner per spec.** The owner named in the Contracts table writes and changes its spec. contracts (U1) owns every shared type and interface in C3, C5 and C6.
- **Changes in one commit (D-CD4).** A change to an in-process contract (C6 to C14) lands in the same commit as every consumer's update, and the type check fails the merge otherwise.
- **Additive changes are safe.** Adding an optional field or a new operation needs no agreement. Consumers ignore fields they do not know.
- **Breaking changes need a note.** Removing or renaming a field, changing its meaning, or adding a required input needs a change note under "Change Notes" below, naming every consumer it affects.
- **Stored and file shapes are versioned.** The consented record, rule snapshots and C5 result files each carry `schemaVersion`. A reader refuses a version it does not know, and a writer never reuses a version number for a different shape.
- **External APIs (C1, C2).** These are versioned in their OpenAPI `info.version`. The family's browser always runs the matching release, so there is no separate public API version to keep alive.
- **Error codes are part of the contract.** Adding a code is additive, but a consumer must handle unknown codes as `INTERNAL`. Removing a code is a breaking change.

## Change Notes

None yet. The first entry will be the first breaking change after approval.

## Open Questions

| Contract | Question | Blocks |
|---|---|---|
| C3 | Which reading service wins the reader trial (requirements OQ8), and does its output map cleanly to `PageReading`? | bill-analysis (U4) adapter implementation |
| C4 | Sign-in standard, provider and second factor for staff (D-US4) | rules-access (U9), internal-ui (U11) |
| C1, C2 | Hosting (OQ2): the chosen host's request-size and duration limits may change the 4 MiB chunk size (D-CD16) or the 3-second poll | document-intake (U3), family-ui (U8) |
| C6 `ScheduledJob` | Which scheduler runs the worker's jobs (OQ2); the idle sweep needs one that can run every 60 seconds | case-processing (U5), session-privacy (U2), feedback-insights (U7) |
| C3, D-CD15 | The chosen reader's rate limits: can it serve 6 pages in parallel per job at 10 times pilot volume (NFR14)? | bill-analysis (U4), case-processing (U5) |
| C6 `BetaHandoverSink` | Whether the closed beta runs before hosting is decided; if not, the hand-over may never be wired | document-intake (U3), test-set-quality (U10) |
| C11, C1 | The exact fields of `ReportContent` for the seven report parts (FR10.1); due in findings-report's Functional Design, before family-ui's Code Generation | findings-report (U6), family-ui (U8) |
| C1 `PlanAnswer` | The fixed list of plan choices (FR12.2); due in feedback-insights' Functional Design | feedback-insights (U7), family-ui (U8) |

## Assumptions & Open Questions

- Every unit runs in one codebase, so the in-process contracts (C6 to C14) are TypeScript interfaces in the contracts unit. The YAML above is their language-neutral description, and Code Generation writes the TypeScript from it. Type expressions such as `"Result<...>"` and optional markers such as `"betaConsent?"` are quoted so every block parses as YAML.
- The family and staff cookies are separate and never interchangeable. NFR Design confirms the cookie settings and session lifetimes.
- The money and date rules (integer paise, ISO dates, IST display) are fixed here because they cross every boundary. Functional Design may add stricter types, but not change them.
