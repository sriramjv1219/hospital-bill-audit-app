**Collaborator:** aidlc-developer-agent

## Contribution

This is the developer view of `stories.md` and `personas.md`: whether each story can be built, how big it is, hidden technical dependencies, ordering, and criteria that cannot be built as written or would cost a lot. Nothing here reopens Q1–Q16. Where a draft creates build risk under a confirmed answer, I propose wording that keeps the answer and makes it buildable and testable. New criteria use the next free ID in their story, so no existing ID changes. Where I mention Next.js behaviour, it comes from the bundled docs in `node_modules/next/dist/docs/` (16.4.3).

The items are in priority order.

### 1. The 300 KB script limit needs a measurement definition (AC1.1.2, AC2.10.2, NFR11)

As written, "loads at most 300 KB of JavaScript" does not say whether bytes are counted compressed or uncompressed, or whether chunks loaded later (on a click) count. If uncompressed bytes are counted, the limit fails from day one: React 19 and the Next.js client runtime alone are close to that size before any product code. If compressed transfer size on first load is counted, the limit can be met, but it is tight once a PDF library (item 2) and a file-upload client (item 3) are added.

Proposals:

- Replace the per-story copies (AC1.1.2, AC2.10.2) with one cross-cutting criterion that runs on every merge, and keep the 4-second check per screen:
  - **AC7.8.4 (new, or a shared "every user screen" criterion):** Given a production build, when the script-size check runs on every merge, then no user route loads more than 300 KB of compressed (gzip) JavaScript on first load, and the merge is blocked if any route does.
  - **AC1.1.2 (reworded):** Given the throttled 4G profile and a 4x CPU slowdown standing in for the 8 GB Android target, when the start screen loads, then its main action can be used within 4 seconds.
  - Apply the same wording to AC2.10.2 and to any other screen that gets a timing criterion.
- Say how the 8 GB Android device is represented. CI can only emulate it, using Playwright on Chromium with network and CPU throttling. A check on a real device is a manual step before launch. Proposed sentence for "Common test conditions": "CI emulates the target device (throttled network, 4x CPU slowdown); a pass on a real 8 GB Android phone is recorded once before launch."
- Whether chunks loaded on demand count towards the 300 KB is a judgment call (see Positions). My recommendation is that they do not count towards the first-load budget but get their own limit (for example, the PDF chunk no larger than 200 KB compressed).

### 2. The PDF built on the device, on phone and desktop (US5.4, D-US9, [Q15])

I agree this can be built, and it keeps report content off our servers. Four details need to go into the criteria, though:

- **Web Share needs a fresh user tap.** `navigator.share({ files })` only works within a short window after a tap. If building a PDF of several pages takes longer than that window on a mid-range phone, the share menu silently fails to open. Proposed AC5.4.2 (reworded): "Given I am on a phone, when I choose 'Save or share', then the PDF is built on the phone and, once it is ready, a 'Share' button opens the phone's own share menu." The PDF can be built ahead of the tap, but the share call itself must come from a tap.
- **Detect features, not device types.** Desktop Chrome on Windows and ChromeOS also supports file sharing, and some Android browsers do not. Proposed AC5.4.4 (reworded): "Given the browser cannot share files (`navigator.canShare({ files })` is false or missing), when I choose 'Save or share', then the PDF downloads instead."
- **The PDF library loads only when needed.** Proposed **AC5.4.5 (new):** "Given the report screen, when it first loads, then the PDF library is not loaded; it loads only when I choose Download or Save or share."
- **The ₹ sign and Indian digit grouping in the PDF.** The standard PDF fonts have no ₹ glyph, so a font that has U+20B9 must be embedded, which adds to the PDF chunk size. Proposed **AC5.4.6 (new):** "Given a synthetic report with amounts in lakh and crore, when the PDF is opened in a PDF reader, then every amount shows the ₹ sign and the grouping used on screen (for example ₹1,20,000)."
- **AC5.4.3 is too broad as written.** The report content does come from our server, because the server runs the checks. What must never reach a server is the generated PDF file. Proposed AC5.4.3 (reworded): "Given either platform, when the PDF is built, then it is built from report data already in the browser, and no network request made during or after building carries the PDF file."
- **The PDF generator is a hidden shared component.** US5.2 (full report), US5.3 (one A4 page), US6.1 (checklist), US6.2 (dispute pack) and the letters in US5.6 all go through it. AC5.3.1 ("fits one A4 page") can only be tested on the device-built PDF, and the test can parse the generated file in Jest and assert one page. Proposed split: **US5.4** (desktop download and the shared PDF builder, including the one-page summary layout) and **US5.11 (new) "Save or share the report from my phone"** (AC5.4.2, AC5.4.4 and the share-menu edge cases). Each fits within 5 days. Built together, they do not.
- The walking skeleton includes "a stub report … offered as a download" (`team-practices.md`). That puts the on-device PDF library into Bolt 1, which is the right time to set up the script-size check in item 1 and prove the budget.

### 3. Upload resume (US2.10, NFR11) and the upload path

- **The default Next.js upload paths cannot take a 25 MB file.** Server Actions accept 1 MB request bodies by default (`serverActions.bodySizeLimit`). A `proxy.ts` buffers at most 10 MB by default (`proxyClientMaxBodySize`). Hosted serverless platforms usually cap request bodies at a few MB. Uploads therefore have to be split into chunks: either a chunked Route Handler or direct, multipart upload to object storage. Resume needs that chunking anyway. Both options depend on the hosting and storage decision (OQ2), so US2.10, and in practice US2.1, are blocked by it. Delivery Planning should record this.
- **"Comes back within the session" needs a scope.** Resuming while the page stays open is cheap. Resuming after the tab is reloaded or discarded (Android does this when memory is short, for example after switching to the camera app) is not: the browser cannot reopen the user's files without asking, and keeping them in IndexedDB would leave a copy of the bill on the device. That is a judgment call (see Positions). My recommended wording, which keeps [Q10] and NFR11:
  - **AC2.10.1 (reworded):** "Given three files where two have finished uploading, when the connection drops and comes back while the page is still open, then the third file continues from where it stopped and the first two are not sent again."
  - **AC2.10.4 (new):** "Given the page was reloaded during an upload, when it opens again within the session, then the files already received are listed as uploaded and I am asked to choose only the files that did not finish."
- **Partial chunks are copies of the original.** Proposed **AC7.5.5 (new):** "Given an upload that is abandoned, discarded or timed out partway, when the session ends, then no partial upload or chunk of it remains in any store."

### 4. Deleting the original file (US7.5, FR2.3): make "no copy" concrete and testable

- **Copies we create.** Page images rendered from a PDF, thumbnails for "page 4 of the pharmacy bill" (US2.6), the decrypted copy of a password-protected PDF (US2.8) and upload chunks (item 3) are all copies of the original. Proposed **AC7.5.6 (new):** "Given a synthetic bill, when its content has been extracted, then every derived page image, thumbnail and decrypted copy is deleted along with the original."
- **Copies the bill-reading service holds.** We cannot test a file system we do not run. Proposed **AC7.5.7 (new):** "Given the bill-reading service, when a file is sent to it, then the call uses the service's no-retention option or is followed by its delete call, and the service's no-retention and no-training terms are recorded (NFR19)." The test can check our side of the call. The vendor's side is checked against its contract.
- **A concrete test method for AC7.5.4.** Put a unique marker string (a canary) in each synthetic bill, then search temp folders, server logs, error-tracker payloads and the cache directories for it after the deletion tests. Proposed AC7.5.4 (reworded): "Given synthetic bills that carry a unique marker string, when the deletion tests have run, then the marker appears in no temporary folder, log, error report or cache directory."
- **US2.8.** A password-protected file has to stay until the password is entered. Proposed **AC2.8.4 (new):** "Given I skip a protected file or the session ends before I enter its password, when the skip or end is handled, then that file is deleted."
- The deletion test (US7.5) and the canary search belong in Bolt 1, alongside the deletion step the skeleton already includes.

### 5. Rendering per request, prefetching and the back button (US7.1, US7.4, US7.6, FR2.10)

- **Knowledge point on the browser cache.** With `cacheComponents` and `partialPrefetching` on, a page shell that reads `cookies()` or `headers()` is "cached per session on the client rather than in the shared server cache" (bundled docs, `01-app/01-getting-started/08-caching.md`). The browser's back-forward cache can also bring back a page as it was. So after the session ends, the back button can still show the report from the browser's memory even though the server holds nothing. AC7.1.2 is right to require that this never happens, but the work it implies is not visible:
  - **AC7.1.2 (reworded):** "Given the session has ended (by timeout, 'Discard everything' or finishing), when I press back, open an old link or return to the tab, then the start screen is shown and no bill data from the ended session is rendered, including from the browser's back-forward cache or router cache."
  - **AC7.6.4 (new):** "Given every route that shows bill data or findings, when the build output and response headers are inspected, then none of those routes is prerendered or uses `use cache`, and their responses carry `Cache-Control: no-store`."
  - **AC7.6.5 (new):** "Given links to screens with bill data, when a user screen loads, then those screens are not prefetched with bill data in them."
  - A lint rule or code-scan check that blocks `'use cache'` in the modules that handle bill data would enforce FR2.10 on every merge. This is a technical task under US7.6, not a new requirement.
- **US7.4: the idle timer must be enforced on the server.** Android throttles or suspends timers in background tabs, so a timer that runs only in the browser may never fire. Proposed **AC7.4.4 (new):** "Given my phone slept or the tab was in the background for more than 30 minutes, when I return, then the server has already ended the session and the start screen is shown, whether or not the warning appeared."
- **Status checks must not count as activity.** If the page's own background checks on processing progress counted as activity, a tab forgotten on a shared phone would never expire. See Positions.

### 6. Missing foundation stories (hidden dependencies)

Several stories say "Depends on: none" or depend only on a screen, but they share three foundations that no story owns. If nobody owns them, they are built implicitly in whichever story comes first, and its size estimate is wrong. Each proposal below documents its origin, so it adds no new requirement.

- **US7.9 (new) Keep my check tied to this browser only.** Origin: FR2.1, FR2.2, FR2.8, FR1.8, NFR8. As Priya (C1), I want my check tied to this browser and never reachable by a link someone else could use, so that nobody else can open my bill. Priority: Must Have. Depends on: none. It underlies US1.1 AC1.1.4, US2.10, US6.7, US7.1, US7.3 and US7.4.
  - AC7.9.1: Given a new visit, when the journey starts, then an unguessable session identifier is set in an HttpOnly, Secure, SameSite cookie and never appears in a URL.
  - AC7.9.2: Given another browser with the same URL, when it opens a screen from my check, then it gets the start screen and no data from my check.
  - AC7.9.3: Given a session identifier that has ended or never existed, when it is sent, then it is refused and no case is shown.
- **US3.9 (new) Have my documents processed in the background while I see progress.** Origin: FR3.3, FR7.3, NFR1, NFR2, D-US1. As Priya (C1), I want my bill processed in the background with progress shown, so that I can wait without the page freezing or timing out. Priority: Must Have. A full report can take up to 15 minutes (NFR2), which is longer than a single request or serverless function can run. This needs a job runner and depends on hosting (OQ2). It underlies AC3.2.2, US3.3, AC4.14.4, US5.2 and the "processed in arrival order" rule (AC5.2.4).
  - AC3.9.1: Given an upload has completed, when processing starts, then the screen shows the current stage and updates without a reload.
  - AC3.9.2: Given processing is running, when I choose "Discard everything" or the session times out, then processing is cancelled and nothing it produces afterwards is stored.
  - AC3.9.3: Given a processing step fails, when the failure is handled, then the completed parts are kept for the session, a retry is offered (FR7.3), and the original file is still deleted (FR2.3).
- **US8.4 (new) Load the launch reference data with its sources.** Origin: FR14.1, FR14.2, FR14.5, FR8.11, PRD D22 (reference data from public sources). As the builder or founder (I1), I want NPPA ceiling prices, device caps, IRDAI Lists II–IV and the grievance timelines loaded with their source and version, so that the price checks have data on day one. US8.1 covers editing reference data but not the first load. US4.5, US4.6, US4.13 and US5.8 cannot run without this story.
  - AC8.4.1: Given the launch data files, when they are loaded, then every item carries source, effective dates, version, owner and review date, and an item missing any of these is rejected.
  - AC8.4.2: Given loaded reference data, when a report runs, then it records the reference-data version it used (FR8.17).
- **The text catalogue (NFR13, AC5.5.4).** "No user-facing string hard-coded in components" is cheap from the first screen and costly to add later. It should be set up in Bolt 1 and enforced by a lint rule (for example `react/jsx-no-literals` limited to the user routes). This is a technical task, not a story.

### 7. Separate the build dependency of each check from its release dependency (US4.1–US4.7, US8.3, US10.1)

Today every check story depends on US10.1 (promotion), which depends on US9.4 → US9.2 → US9.1 → US10.2 (sign-in, defined only in NFR Requirements) and on real labelled bills, which need the hosting and residency decision (OQ1, OQ2). Read literally, no check logic can be built or tested until all of that is done, which breaks "independently testable".

Proposals:

- Write each check criterion against a **structured synthetic bill fixture** (line items as data, no bill reading involved), so the checks become fast Jest unit tests that do not depend on US3.8's extraction. Add to "How to read these stories": "Check criteria in Group 4 are tested on structured synthetic bill data; showing a check to users additionally requires its US10.1 promotion."
- Change "Depends on" for US4.1–US4.7 to **build: US3.8 (the bill data model only) and the check runner in US8.3; release: US10.1**. Update the dependency table to match.
- **Split US8.3.** The runner obeying live, shadow and off is the engine every check runs in, not a light internal story. It can read check states from stored configuration before any admin screen exists.
  - **US8.3 (runtime):** AC8.3.1–AC8.3.4, with no dependency on US8.1 or US10.2.
  - **US8.5 (new) "Switch a check's state from the internal tools":** the admin screen, which depends on US10.2.

### 8. US3.8 is several weeks of work, not one story, and AC3.8.4 is a launch gate

US3.8 covers 13 requirements: bill reading for PDFs and photos, a 21-category classifier, and enrichment of drugs, consumables, implants, professional fees and packages with List II–IV mapping. It also depends on the outcome of the bill-reader trial (OQ8) and the data-processor check (NFR19). The draft already notes that Units Generation may split it. I recommend splitting it now, because the split changes the dependency table:

- **US3.8 Read header, lines and totals from a digital PDF** (FR5.1–FR5.3): tested on synthetic PDFs against their answer keys.
- **US3.10 (new) Read photographed and scanned bills** (FR5.1–FR5.5 on images, including Indian digit grouping, tables across pages and handwritten notes).
- **US3.11 (new) Read the discharge summary** (FR5.4).
- **US3.12 (new) Sort each line into its category and add its details** (FR6.2–FR6.8, including AC3.8.2 and AC3.8.3).
- Move **AC3.8.4 and AC3.8.5 to US9.4** as launch-gate criteria. They need about 100 real labelled bills of each kind, which cannot exist on a story's merge day. Replace them in US3.8 with: "Given synthetic bills with answer keys, when the read is compared with the key, then every planted field is extracted correctly."

US2.5 (page order and missing pages), US2.6 (legibility), US2.7 (summary-bill detection), US3.6 (unread scripts) and US3.7 (mismatches) need extracted text. They depend on US3.8 or US3.10 as well as US2.4. Please add that to their "Depends on" lines.

### 9. Smaller criterion fixes

- **US2.2 camera (AC2.2.1, AC2.2.3).** The cheap, light way to open the phone camera (`<input type="file" accept="image/*" capture="environment">`) hands over to the phone's own camera app. We cannot show tips inside it, and the browser never asks for camera permission. A custom in-page camera would add script weight and a lot of work. Proposed rewording:
  - AC2.2.1: "Given I choose 'Take a photo' on my phone, when the step opens, then three tips are shown before the camera opens: lay the page flat, avoid glare, keep all edges in the frame."
  - AC2.2.3: "Given the camera cannot be opened or I cancel it, when I return, then I am offered file upload instead and nothing already added is lost."
- **US2.3 page limit (AC2.3.3).** Counting a PDF's pages in the browser needs a PDF parser, which costs script weight, so the server should count pages after upload. Proposed **AC2.3.5 (new):** "Given a 10-page PDF that would take the session past 60 pages, when it is uploaded, then the whole file is refused with the 60-page message, it is deleted at once, and earlier files stay."
- **US4.9 AC4.9.4 determinism.** If any part of reading or explaining uses an AI model, running the same upload twice may read it slightly differently. Determinism can only be promised from the confirmed data onwards. Proposed rewording: "Given the same confirmed line items and the same rule and reference-data versions, when the checks run twice, then the findings are identical, and each finding traces to its source page and line, rule ID and reference-data version."
- **US5.5 AC5.5.2 Flesch score.** Say what the test runs on. If explanations come from templates, it scores the templates. If they are generated per bill, it scores a fixed corpus of synthetic reports. Proposed addition: "…scored over every explanation template and a fixed corpus of synthetic reports".
- **US7.1 download reminder after a regeneration.** "Fix a line" (US3.5) and "Add a document" (US6.7) make an earlier download out of date. Proposed **AC7.1.3 (new):** "Given I downloaded the report and then it was regenerated, when I try to leave, then I am reminded that my download is out of date."
- **US7.2 storage consent left unanswered.** Proposed **AC7.2.6 (new):** "Given the session ends before I answer the storage question, when it ends, then this is treated as declining and nothing is kept."
- **US7.7 "3 months".** Define it so the test has an exact boundary. Proposed AC7.7.1 addition: "…older than 3 calendar months from its session date (IST)". The deletion job is scheduled work, which depends on hosting (OQ2).
- **US6.6 AC6.6.3.** Counting a second click once needs a flag held for the session. Proposed addition: "…the flag that prevents double counting lives only in the session and is discarded when the session ends."
- **Money (AC4.11.2, AC4.10.2).** "The four parts equal the bill total" is exact only if amounts are kept as whole paise (integers), not decimals. How money is represented is left to design (`team-practices.md`). Proposed addition to AC4.11.2: "…equal the bill total to the paisa".
- **AC2.1.3 citation.** "[Q7]" refers to Requirements Analysis Q7 (WhatsApp guidance), not this stage's Q7 (Won't Have stories). Change it to "[Q7 in Requirements Analysis]", the same pattern AC2.3.1 uses.
- **Review checks, not automated tests.** AC1.3.4 (founder or counsel approval), AC6.3.1 (no outbound calls), AC10.2.5 (encryption) and AC5.10.3 (wording review) are release or launch checklist items. Marking them "(launch checklist)" or "(review)" keeps the 80% coverage and merge-blocking discussion clear.

### 10. Story sizes (aiming for one to five days each)

| Story | Estimate | Note |
|---|---|---|
| US3.8 as drafted | 15–25 days | Split per item 8 |
| US5.4 as drafted | 6–8 days | Split into US5.4 and US5.11 per item 2 |
| US10.2 | 5–8 days | Split into sign-in with roles (needs the NFR definition) and the access log, which can be built first |
| US7.2 | 5–7 days | De-identification (AC7.2.4) is its own piece; consider "US7.10 Remove identifiers before storing" |
| US2.10 | 3–5 days, after the storage choice | Blocked by OQ2 |
| US4.7 | 4–6 days, uncertain | AC4.7.2 "items the package normally includes" needs a reference list of inclusions per package type. No such data source is named, and DUP-, RX- and CON- have their reference data, but PKG does not |
| US9.4 | 3–5 days | The accuracy harness; gains AC3.8.4 and AC3.8.5 |
| US1.5–US1.7 | 1–2 days each | Fine as separate stories |
| US4.1–US4.6, US4.8–US4.14, Groups 5 and 6 (except the above) | 1–4 days | Fine |

### 11. Order (for Delivery Planning)

1. **Bolt 1 (skeleton):** US7.9 (session), a thin US1.4, US2.1 (single small file), US7.5 with the canary test, a stub US5.4 (PDF on the device), the script-size check (item 1), the text catalogue lint rule, the `use cache` guard (item 5), and the team's CI fixes. The upload in this slice can be a single small request. Chunked upload follows once storage is chosen.
2. US3.9 (processing and progress), US8.4 (reference data), US8.3 runtime, US3.8 (digital PDF reading).
3. Check families on structured fixtures (Group 4), in parallel with the consent screens (Group 1) and the report and letters (Group 5).
4. Work that hosting decides: US2.10 (resume), US3.10 (photos), US10.2, then US9.x and US10.1 (promotion). No check can go live until this chain is done, so OQ1 and OQ2 are on the critical path to launch, not only to deployment.

## Positions

- AGREE: grouping by journey step [Q2], one story per check family [Q4] and lighter internal stories [Q5]. These map cleanly onto build units and test files.
- AGREE: the PDF built on the device on both phone and desktop (D-US9, [Q15]) can be built and is the better choice for privacy, provided the details in item 2 (share after a tap, feature detection, loading the library on demand, an embedded ₹ font) become criteria.
- AGREE: dropping the urgent queue (D-US1, [Q8]). Processing in arrival order makes the job runner in US3.9 simpler.
- OBJECT (knowledge point): the 300 KB limit must be defined as compressed JavaScript on first load per route, enforced by one check in CI. Counted uncompressed, React 19 and Next.js alone nearly use it up, so AC1.1.2 and AC2.10.2 would fail on an empty screen.
- OBJECT (judgment call): whether chunks loaded on demand after a tap (the PDF library) count towards the 300 KB. If they count, the PDF library choice is tightly constrained. If they do not, they need their own limit. I recommend a separate limit (for example 200 KB compressed for the PDF chunk), but the founder should decide.
- OBJECT (knowledge point): AC2.2.1 and AC2.2.3 cannot be built as written with the phone's own camera (no tips inside the camera app, no browser permission prompt). Showing the tips before the camera opens keeps the intent at low cost.
- OBJECT (judgment call): how far upload resume goes. Resume while the page is open (recommended, cheap), or also after a reload or tab discard, which means asking the user to choose the unfinished files again or keeping copies on the device. Both are legitimate under NFR11. The second costs more and raises a privacy question.
- OBJECT (knowledge point): US3.8 is several weeks of work and must be split before Units Generation. AC3.8.4 and AC3.8.5 need about 200 real labelled bills and belong in the US9.4 launch gate, not in a story's merge criteria.
- OBJECT (knowledge point): check stories (US4.1–US4.7) should depend on the bill data model and the check runner to be built, and on US10.1 only to be released. Otherwise every check is transitively blocked by the sign-in definition and the hosting decision, and none can be tested on its own.
- OBJECT (judgment call): what "fix a rule … without a release" ([Q16], AC8.1.1) covers. Changing reference prices, thresholds, check parameters and live, shadow or off states without a release is cheap. Changing a check's logic without a release needs a rules language and is very costly. I recommend limiting it to data and parameters, with logic changes going through a normal release. The answer to [Q16] stays as it is, but the founder should confirm this reading.
- OBJECT (knowledge point): AC4.9.4 can promise identical findings only from the confirmed line items onwards, not from the raw upload, because AI-based reading need not return the same result twice.
- OBJECT (knowledge point): AC7.1.2 and US7.6 need explicit work: no prerendering or `use cache` for bill routes, `Cache-Control: no-store`, no prefetching with bill data, and the session ended on the server. Under Cache Components and partial prefetching, the browser keeps a per-session copy of the page shell, and the back button can bring back a page from memory.
- OBJECT (judgment call): whether the page's background checks on processing progress count as activity for the 30-minute idle timer. I recommend they do not, so a forgotten tab on a shared phone still expires. The cost is that a very slow report could reach the 25-minute warning while it is still being processed.
- OBJECT (knowledge point): three foundations have no story: the anonymous session (US7.9), background processing with progress (US3.9) and the first load of reference data (US8.4). Without them, the stories that rely on them are estimated too small and their dependencies are hidden. Each proposal cites the requirements it comes from.
