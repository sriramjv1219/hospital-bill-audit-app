**Collaborator:** aidlc-quality-agent

## Contribution

This is the quality view of `stories.md`, `personas.md` and `traceability.json`. It covers whether each acceptance criterion can be tested, how each one would be verified (Jest, Playwright, the offline Labelled Test Set run, or a manual or launch-checklist step), and where a requirement's pass/fail test from `requirements.md` is weaker or missing in the stories. All test data referred to here is made up (synthetic). No real bill is used or implied. Items are in priority order.

### 1. Add three common test conditions (affects Groups 1, 4 and 7, and NFR20)

Add these three bullets under "How to read these stories", next to the existing "Common test conditions" bullet:

- **Checks are tested in a fixed state.** The criteria for each check family (US4.1 to US4.7) are tested with that check set to `live` in a test copy of the rule library, and the shadow-mode criteria are tested with it set to `shadow`. Whether a check is live in production is governed only by US10.1. This lets every Group 4 story be tested on its own, as the inception guardrail requires ("Stories must be independently testable"). Today US4.1 to US4.7 depend on US10.1, which in turn depends on a real Labelled Test Set run.
- **The bill reader is replaced in the code test suite.** In the Jest and Playwright suites the bill-reading service is swapped at its adapter boundary for recorded outputs of synthetic bills. The real reader is used only in the timing runs (US3.3, US5.2) and the offline accuracy run (US9.4). The code suite is then deterministic and sends nothing to an outside service, and accuracy is still measured where `team.md` places it.
- **Synthetic fixtures carry a manifest.** Every synthetic bill fixture comes from a seeded generator, carries its answer key (FR16.6) and a visible `SYNTHETIC` marker, and is listed in a fixture manifest. An automatic check fails the merge if any file under the fixture folders is missing from the manifest. This gives NFR20's "no fixture contains a real bill" a pass/fail check that can be automated.

Also add this bullet, because "by any route" (AC1.4.2) cannot be shown by tests of the screens alone:

- **Gates are enforced on the server.** The processing consent, relationship, guardian and adult-patient declarations (US1.4 to US1.7) are enforced on the server. Each gate is tested with a direct request to the upload endpoint that skips the screens, and that request must be refused.

### 2. Q10 is applied to two screens only. It must apply to every screen (US1.1, US2.10, NFR11)

Q10 says "each screen usable within 4 seconds", and NFR11's test is "a build report shows no page over 300 KB of scripts". The draft applies both only to the start screen (AC1.1.2) and the upload screen (AC2.10.2). The review, report, letters and discard screens are therefore untested. Proposed new criterion on US3.2, the shared layout story:

- AC3.2.3 Given the throttled 4G test profile and the CPU slow-down set for the 8 GB Android target, when each user screen in the journey is opened (start, explainer, privacy notice, consent, relationship, upload, case details, first look, confirm details, findings, report, letters, before you go), then its main action can be used within 4 seconds of navigation start [Q10].
- AC3.2.4 Given the production build report, when it is checked by the automatic checks, then no user route loads more than 300 KB of JavaScript (NFR11).

AC1.1.2 and AC2.10.2 can then refer to AC3.2.3 and AC3.2.4. NFR Requirements should state whether the 300 KB limit is the compressed transfer size or the uncompressed size. The test cannot pass or fail until it does. Testing approach: Playwright with Chrome DevTools Protocol network and CPU throttling on every merge, plus one manual pass on a real 8 GB Android phone before launch.

### 3. Disclaimer coverage is incomplete (FR13.1, US1.2, US5.2, US5.6)

FR13.1 requires the disclaimer on three surfaces (onboarding, reports and letters) with four elements: informational only; not legal, medical or financial advice; no guaranteed outcome; the user is responsible for their own communications.

- AC1.2.1 leaves out "informational only" and "financial". Replace with: *Given I am on the start screen, when the explainer appears, then it states that the check is informational only, is not legal, medical or financial advice, gives no guaranteed outcome, and that I am responsible for my own communications.*
- AC5.2.2 covers RP-08's limitations but not FR13.1's disclaimer. Add AC5.2.5: *Given any report, when it is generated, then its disclaimer section contains all four FR13.1 statements.*
- No criterion covers letters. Add AC5.6.4: *Given any generated letter, when it is downloaded or copied, then it carries the FR13.1 disclaimer, and the disclaimer stays in place after I edit the letter body.*

### 4. Reproducibility must be defined after the bill has been read (AC4.9.4, NFR6)

The bill reader is AI-based and does not always give the same output. "Same synthetic case ... findings identical" can only be guaranteed, and tested on every merge, from the extracted data onwards. Replace AC4.9.4 with:

- AC4.9.4 Given the same extracted and confirmed line items and the same rule-library and reference-data versions, when the checks are run twice, then the findings are identical (same check IDs, classes, confidence, amounts and evidence), and each finding traces to its source page and line, its rule ID and its reference-data version.
- AC4.9.5 Given a report is regenerated in the same session after the rule library has moved to a new version, when it is shown, then it uses one rule version throughout and shows the version it used (FR8.17), never a mix of two.

### 5. Missing negative and boundary cases in the check families (US4.1 to US4.7)

Every check needs a "correct bill gives no finding" case, and every amount comparison needs a stated tolerance. Without one, a rounding difference becomes a false "Likely error". That damages trust and counts against the 90% precision gate.

- AC4.1.9 (CAL-01 rounding) Given quantity × rate differs from the billed amount by no more than the rounding tolerance set in the rule library (for example 3 × ₹33.33 billed as ₹100.00), when the check runs, then no finding appears. Given the difference is one paisa more than the tolerance, then a "Likely error" appears. *The tolerance value is set in Functional Design, but the criterion must exist now.*
- AC4.2.1: state the class ("Likely error" or "Worth asking"). Add the case of a legitimate repeat, for example two units of the same item given on the same day with distinct times, if the PRD treats that as allowed.
- AC4.2.4 has only the negative case for FR8.5's "investigation repeated without a matching clinical note". Add AC4.2.6: *Given the same investigation repeated on two days with no matching clinical note, when the checks run, then the repetition is found.*
- AC4.2.5, AC4.2.6, AC4.7.2 and AC4.8.2 describe FR8.5, FR8.8 and FR8.10 behaviour that does not belong to any of the 18 launch check IDs. Name the check ID for each. Where the check is not a launch check, state that it runs in shadow mode (recorded, not shown) until it passes FR16.12, as AC8.3.4 does for FR8.6 and FR8.7.
- AC4.3.1 depends on "the hospital's day convention", which is not defined. Add a boundary case: *Given admission at 23:30 and discharge at 00:30 the next day, when LOS-01 runs, then the days of stay follow the convention recorded in the rule library and the finding states that convention.*
- Add an "exactly at the limit" case to US4.5 and US4.6: AC4.5.5 *Given a drug charged exactly at its MRP or ceiling price, when RX-01 or RX-02 runs, then no finding appears.* Use the same wording for CON-04 and IMP-02 as AC4.6.4.
- FR8.11's "no CGHS, PM-JAY or internal benchmark as the reason" is applied to drugs only (AC4.5.4). Add the same criterion to US4.6 (consumables and implants) as AC4.6.5.

### 6. Session-end and consent edge cases (US7.2, US7.3, US7.4, US7.5, US7.1)

These cover the zero-tolerance privacy guardrail. Each is a missing error path.

- AC7.2.6 Given the report has been shown and I neither agree nor decline storage, when the session ends (I close the tab, the session times out or I discard), then this counts as declining and no record is kept (FR2.4).
- AC7.3.3 Given processing is still running, when I choose "Discard everything" and confirm, then the running job stops or its output is thrown away, and no extracted data, finding or original file from the session exists afterwards.
- AC7.4.4 Given 24 minutes 59 seconds without activity, when the timer is checked, then no warning is shown. *This is the boundary case.*
- AC7.4.5 Given I close the browser tab without ending the session, when 30 minutes pass, then the server ends the session and its files and extracted data are gone. *The idle limit must be enforced by the server, not only by the page.*
- AC7.5.5 Given a file that was refused (unsupported format, over a limit, wrong password three times, or skipped), when the refusal is handled, then no copy of it remains in storage, temporary folders, logs or error reports.
- AC7.1.1 cannot pass as written when the tab is closed. Modern browsers ignore custom text in the "leave this page?" dialog, so "told it cannot be retrieved later" can only be shown on in-app exits. Split it: *AC7.1.1 Given I have not downloaded the report, when I choose "End session" or "Start a new check", then I am reminded to download it and told it cannot be retrieved later. AC7.1.3 Given I have not downloaded the report, when I close or reload the tab, then the browser's own leave-page prompt appears.*

### 7. Stored-data criteria that cannot pass as written (AC7.2.3, AC7.2.4, AC7.7.1)

- **Contradiction to resolve, per the inception guardrail.** AC7.2.3 stores "dates" (the date of each line item) and also "admission month". Line-item dates give away the exact admission and discharge dates, so a test that checks "only the admission month is kept" cannot pass. Either store each line's date as the number of days from admission, or accept exact dates and remove "admission month" as a protection. This is a de-identification decision for Domain Design or compliance. The stories should record which one is chosen.
- **Identifiers outside the bill header.** AC7.2.4 plants identifiers only where FR2.6 expects them. Extend it: *... and the same holds when the patient's name or phone number appears inside a line-item description or in the optional reason given with a rating.* If the free-text rating reason (FR12.1) is to be stored at all, it needs this test. See Positions.
- **"3 months" needs a precise meaning and a tolerance.** It must be stated as a fixed number of days (for example 90) or as calendar months, and the test must allow for how often deletion runs. With a daily job, a record can reach 3 months plus up to 24 hours. Replace AC7.7.1 and AC7.7.2 with: *AC7.7.1 Given stored records created at known times (injected clock), when the stores are checked at any moment, then no record is older than 3 months as defined in NFR Requirements [Q15]. AC7.7.2 Given a record that reaches that age, when the next deletion run happens, then the record is removed; and given a deletion run fails, then an alert is raised.* Without the failure case the guardrail can be breached silently.

### 8. Criteria that describe design work or a review rather than behaviour (AC10.2.4, AC8.3.4, AC6.3.1, AC7.6.2, AC5.4.3)

- AC10.2.4 ("when it is designed") is not something a test can check. Replace with: *Given the strong sign-in definition set in NFR Requirements, when an internal user signs in with only part of it (for example a password alone), then sign-in is refused [Q11].*
- AC8.3.4 ("when they are built") becomes: *Given a newly added check, when it is first saved, then its state is `shadow`, and it cannot be created directly as `live`.*
- AC6.3.1 becomes automatable as: *Given the complete Playwright journey and the server's outbound request log, when they are checked, then every outbound destination is on an allow-list of our own services and the approved data processors, and none is a hospital, insurer, email or SMS service.*
- AC7.6.2 becomes: *Given every route that shows bill data, when its responses are checked, then they are marked as not cacheable and private, the build output shows those routes are rendered per request and not prebuilt, and no cached function reads bill data (static check).* Partial prefetching is on in this Next.js version (`architecture.md`), so this needs a specific test. AC7.6.1 (two concurrent Playwright browser contexts with different synthetic bills) stays as written.
- AC5.4.3 ("no request carries ... the report content") cannot pass, because the report page itself is fetched from our server. Replace with: *Given I choose "Download" or "Save or share", when the PDF or image is created, then no network request made from that moment on has the file, or the report body, as its payload, to our servers or anyone else.* Test: Playwright request interception, with `navigator.share` replaced by a stub that checks it receives a `File` of type `application/pdf` (AC5.4.2). The real phone share menu is checked once by hand on a device. FR10.3 also allows an image. Either add "or image" to AC5.4.2 or record that only a PDF is offered.

### 9. Launch-gate NFR content lost in the stories (NFR7, NFR9, NFR19, FR16.13)

- NFR7's test includes "a security test report exists before launch". AC10.2.5 keeps only encryption. Add AC10.2.6: *Given launch is prepared, when the launch checklist is run, then a third-party security test report dated before launch exists, and every high or critical finding in it is fixed or accepted with a written reason.*
- US9.1, US9.4 and US9.5 handle real bills but have no criterion tying them to NFR9 (data residency) or NFR19 (processor agreements), which are deferred. FR16.13's "accuracy runs only on services that passed the data-processor check" appears only in an INVEST note. Add AC9.4.5: *Given an accuracy run on real bills, when it starts, then it is refused unless every service it uses has a recorded data-processing agreement with a no-training term (NFR19, FR16.13).* Add AC9.1.6: *Given no recorded founder decision on data residency, when a real bill is added, then it is refused (NFR9).*

### 10. Smaller testability fixes

- **AC1.5.2:** add the missing case "no relationship chosen", not only "box not ticked".
- **AC2.3.3:** add "and the pages already accepted stay" (FR4.4). Add AC2.3.5: *Given 55 pages are accepted, when I add a 10-page PDF, then the whole file is refused with the 60-page limit named, and the 55 pages stay.* Define "25 MB" in bytes (25 × 1,048,576 or 25,000,000), so the boundary test in AC2.3.4 is exact.
- **AC2.8.3:** "three" wrong password attempts is not in FR4.9. Record where the number comes from, or mark it as set in Functional Design.
- **AC3.2.2:** "roughly how long it takes" becomes "shows an estimated wait in minutes". Add an error case: *Given processing has failed, when the screen is shown, then the next-step action is "Try again" (FR7.3).*
- **AC3.4.2:** pair it with its opposite case, so FR7.2's separation of reading errors from billing errors is tested in both directions: *Given a high-confidence reading whose arithmetic does not add up, when the checks run, then a CAL finding is raised and no confirmation is requested.*
- **AC3.8.4 and AC3.8.5:** define "read correctly" as a measurable rule (for example the amount, quantity and rate match exactly and the description matches after normalisation). Define "good-quality photo" as a capture-quality label recorded in the test set. For AC3.8.5, state a minimum number of test-set bills for each Indian bill feature, or say that accuracy is reported for each feature without its own threshold. FR6.2 (drug identification) is "measured on the drug lines" but has no target anywhere. Add one, or record that it is reported without a target.
- **US4.9:** has no error case. Add AC4.9.6: *Given a finding missing any of the eight FR8.12 fields, when the report is built, then the finding is not shown and the failure is logged without bill content.*
- **US4.10:** add AC4.10.3: *Given a grouped finding, when its amount is shown, then it equals the sum of its evidence lines, and lines it shares with another finding count once in the reconciled total.*
- **AC4.11.2:** "equal the bill total" should say "exactly, to the paisa". Add AC4.11.4: *Given lines that could not be read (US3.6), when the breakdown is shown, then their amounts are in "could not be assessed".*
- **AC5.1.2:** the ranking needs a defined formula and a tie-break before a unit test can assert the order. Name Functional Design as the place it is set, and add a tie case.
- **AC5.5.1 and AC5.5.2:** say whether an inline explanation is needed at the first use of a term only or at every use. For Flesch reading ease, say that every report in the generated synthetic corpus must score at least 60, not only the average.
- **AC5.5.3:** add boundary amounts ₹99,999, ₹1,00,000 and ₹1,00,00,000, plus a credit (negative) amount.
- **AC5.10.1:** match the banned words case-insensitively and include their forms ("fraudulent", "scammed", "cheated", "illegally").
- **AC5.10.3:** keep the recorded human review, and add an automatic phrase check ("do not pay", "refuse to pay", "withhold payment") that runs on every merge.
- **AC6.6.3:** the "increase once" rule must be enforced within the session, because the stored count is not linked to any session. Add: *the stored count has no session ID, case ID or exact timestamp* (AC6.6.2). Also say who counts as "insured" (payment mode cashless or reimbursement) and whether the button is hidden for self-pay cases.
- **US6.7 or US3.5:** add AC6.7.3: *Given I have corrected lines and marked findings "Will ask", when the report regenerates after I add a document, then my corrections and marks are kept for every finding that still exists.*
- **US7.8:** name the screen readers (TalkBack on Android; NVDA or VoiceOver on desktop) in AC7.8.2. Add AC7.8.4: *Given a 320 CSS-pixel-wide viewport, when each screen is shown, then no horizontal scrolling is needed (WCAG 1.4.10).*
- **US8.1:** add AC8.1.5: *Given a report already being generated when a rule change is approved, when it finishes, then it uses one version throughout.* This links to AC4.9.5.
- **AC9.1.2:** state the size of the redaction spot check: every real bill, or a stated sample with a re-check if any identifier is found.
- **AC9.2.1:** make "independent" testable: *the labelling tool hides each person's labels from the other until both have submitted.*
- **AC9.5.3:** make it testable as a setting check: *the beta setting is off in the production configuration, and with it off every upload path deletes the original.*
- **US10.1:** add the "exactly at the limit" boundaries: *exactly 10 and 30 bills with precision of exactly 90.0% are accepted; 29 bills without the error are refused; a "Worth asking" check at 74% is refused.* State the precision formula (correct findings ÷ findings shown, over the measured bills).

### 11. Verification method for each criterion (for Build and Test planning)

Proposed tag for each criterion. The lead can add these to `stories.md` as a suffix, or keep them in Build and Test.

**Key:**
- **J** = Jest unit or component test.
- **I** = Jest integration test against storage, PDF or adapter fakes.
- **P** = Playwright end-to-end test. This includes async Server Components, which Jest cannot test (`team.md`).
- **C** = static, build or configuration check in the automatic checks.
- **L** = offline Labelled Test Set run. Not run on every merge. Real bills, approved processors only.
- **T** = timing or load test in the test environment, during Build and Test.
- **M** = manual step or launch-checklist evidence with a recorded sign-off.

| Story | Verification |
|---|---|
| US1.1 | 1.1.1 P; 1.1.2 P (throttled) and M (real device); 1.1.3 C and P; 1.1.4 P |
| US1.2 to US1.7 | J and P; gate criteria (1.4.2, 1.5.2, 1.6.2, 1.7.3) also I (direct request); 1.3.4 M; 1.7.2 wording M (US5.10 review) |
| US2.1 to US2.10 | J and I with the reader stub; recognition and legibility accuracy (2.4.1, 2.4.3, 2.6.1) also L; 2.2.x P with fake camera and M; 2.10.1 P (offline toggle) and I; 2.10.2 P and C |
| US3.1, US3.2, US3.4, US3.5, US3.7 | J and P; 3.5.2 P |
| US3.3 | 3.3.1 J; 3.3.2 and 3.3.3 T |
| US3.6, US3.8 | J with the reader stub; 3.8.4 and 3.8.5 L |
| US4.1 to US4.14 | J (check engine on fixture readings); 4.13.3 I; 4.14.4 I with a fault injected; precision before going live is L through US10.1 |
| US5.1, US5.3, US5.5 to US5.9 | J; 5.3.1 I (render the PDF, count pages); 5.5.4 C (lint rule for literal strings); 5.6.2 P |
| US5.2 | 5.2.1 to 5.2.3 J and P; 5.2.4 T |
| US5.4 | P (download event, `navigator.share` stub, request interception) and M (real phone) |
| US5.10 | 5.10.1 and 5.10.2 J on every merge; 5.10.3 M and J (phrase check); 5.10.4 C |
| US6.1 to US6.7 | J, I and P; 6.3.1 C and P (outbound allow-list) |
| US7.1 to US7.7 | I (storage inspection, injected clock) and P; 7.4.x J (fake timers) and P (`page.clock`); 7.6.x P (two browser contexts) and C (headers, build output) |
| US7.8 | 7.8.1 P with axe-core; 7.8.2 M; 7.8.3 P |
| US8.1 to US8.3 | I |
| US9.1 to US9.5 | I; 9.1.2 M; 9.1.4 C (fixture manifest) and M; 9.4.1 L; 9.4.2 C or M (release record) |
| US10.1 | J and I; 10.1.5 M (launch checklist) |
| US10.2 | I and P (role-by-route matrix); 10.2.5 and 10.2.6 M |
| US10.3, US10.4 | J and P; 10.4.1 M; 10.4.2 C (route inventory) |

Criteria tagged **T** (AC3.3.2, AC3.3.3, AC5.2.4) cannot be checked until a test environment exists, and that waits on the hosting decision (`team-practices.md`, Open). Build and Test should state the sample size behind each 90th-percentile figure (proposed: at least 30 timed runs per bill category) and when the clock starts (the server receiving the last file).

### 12. Traceability check (`traceability.json`)

Every FR and NFR ID has a row. The `OK` targets exist in `stories.md`, and FR14.3 `N/A` has a justification. Two notes:

- Q6 deferred only availability, scalability and observability (NFR3, NFR14, NFR18). The draft also defers NFR9, NFR10 and NFR19. That is acceptable as long as the launch-gate parts are tied to stories as proposed in item 9. Otherwise US9.1 and US9.4 can pass with no residency or processor check.
- NFR7 is marked `OK` against US10.2, but its security-testing part has no criterion until AC10.2.6 is added.

## Positions

- AGREE: The shared throttled 4G profile, the boundary criteria in US2.3 and US10.1, and D-US11's minimum of 100 PDF bills and 100 photographed bills give NFR11, FR4.4, FR16.12 and NFR4 clear pass/fail tests.
- AGREE: Measuring accuracy offline (US3.8, US9.4) rather than on every merge matches `team.md` Testing Posture.
- AGREE: The "Won't Have (this time)" stories with no acceptance criteria are correct. There is nothing to test in this build.
- OBJECT: The 4-second and 300 KB criteria cover only two screens, but Q10 says "each screen" and NFR11's test covers every page. Add AC3.2.3 and AC3.2.4 (knowledge point).
- OBJECT: Group 4 stories depend on US10.1 and a real Labelled Test Set run, so they cannot be tested on their own. A test copy of the rule library with checks set to `live` or `shadow` fixes this (knowledge point).
- OBJECT: AC4.9.4 promises identical findings from "the same case". The AI bill reader does not always give the same output, so reproducibility must be defined from the confirmed readings onwards (knowledge point).
- OBJECT: FR13.1's disclaimer is incomplete at onboarding ("informational only", "financial" missing) and has no criterion for letters (knowledge point).
- OBJECT: NFR7's pre-launch third-party security test is missing from the stories (knowledge point).
- OBJECT: Without a rounding tolerance, CAL-01 to CAL-07 will raise false "Likely error" findings on correct bills. A tolerance criterion is needed now, with its value set in Functional Design (knowledge point).
- OBJECT: AC7.1.1, AC5.4.3, AC10.2.4 and AC8.3.4 cannot pass as written (browser limits on the leave-page dialog, the report page itself being fetched from our server, and two that describe design work rather than behaviour). Use the replacement wording in item 8 (knowledge point).
- OBJECT: "No stored record older than 3 months" needs a precise length, a tolerance for how often deletion runs, and an alert when deletion fails. Otherwise the guardrail can be breached without anyone noticing (knowledge point).
- OBJECT: Storing exact line-item dates contradicts keeping only the admission month (AC7.2.3). The contradiction must be resolved, not carried forward (knowledge point for Domain Design or compliance).
- OBJECT: Leaving the storage question unanswered must count as declining (AC7.2.6), and Discard must also cover processing that is still running (AC7.3.3). Both are untested error paths on a zero-tolerance guardrail (knowledge point).
- OBJECT: AC8.2.4 lets the person who made a rule change approve it themselves. With two people, a second-person approval is possible, which I prefer, with self-approval only as a logged emergency exception. Keeping self-approval is also legitimate for speed (judgment call: the human should decide).
- OBJECT: The optional free-text reason given with a rating (FR12.1) may contain names or phone numbers, and would be stored with consent. The choice is between not storing it, offering only fixed reasons, or storing it with an automatic identifier scrub and a test (judgment call: the human should decide).
- OBJECT: The stories do not say whether low-confidence findings, which are folded away under FR8.19, can appear in the Top 3, in the Billing Desk Summary or in the "likely errors" amount of the Bill Breakdown. Either answer can be tested, but one must be chosen (judgment call: the human should decide).
