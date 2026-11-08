# User Stories — Planning Questions

This file plans the personas and user stories for the Phase 1 Hospital Bill Check. The answers decide how the stories are grouped and sized, and how the open review findings from Requirements Analysis are handled.

## Story plan (draft)

- **Personas:** drawn from the target customers in `ideation/intent-capture/intent-statement.md`, plus the internal users of the rule library, the Labelled Test Set and the ops views (the builder and the founder).
- **Format:** "As a [persona], I want [goal], so that [benefit]", checked against INVEST (independent, negotiable, valuable, estimable, small, testable).
- **IDs:** stories `US{group}.{seq}` (for example `US1.1`); acceptance criteria `AC{group}.{seq}.{n}` (for example `AC1.1.1`), in Given/When/Then form, with at least one happy path and one error or edge case per story.
- **Priority:** MoSCoW, taken from the requirement each story covers. The MVP line is drawn later, in Delivery Planning.
- **Traceability:** every functional requirement (FR1–FR16) and non-functional requirement (NFR1–NFR21) is mapped to stories, or marked as deferred to a named later stage.

## Already settled, not asked again

- The product is free, anonymous and English-only, and is checked in one session. Launch geography and brand stance follow the PRD (D17, D25, D22).
- Abuse controls, share-in from messaging apps, urgent-situation guidance and pay-under-protest guidance are out of this build.
- The idle timeout, upload limits, accessibility standard (WCAG 2.2 AA), device target and launch checks were answered in Requirements Analysis.
- The team is one builder plus the founder.

---

## Q1. Which personas should get their own stories?

The intent lists five primary customer types: the family member handling the bill at discharge, a remote family manager (for example a child living abroad), a self-paying patient, a patient who pays upfront and claims reimbursement later, and a bereaved family. Senior citizens are served through a family member. Separate personas give sharper stories but more of them.

A. Four customer personas (the discharge family member, the remote family manager, the self-payer who will claim reimbursement later, and the bereaved family), plus one internal persona for the builder and the founder as rule owners, labellers and operators
B. Two customer personas (the discharge family member, and the remote family manager), with self-payer and bereaved-family needs written as scenarios inside their stories, plus one internal persona
C. One customer persona with scenario variations, plus one internal persona
D. Five customer personas as listed in the intent, plus separate internal personas for the rule owner, the labeller and the operator
X. Other (please specify)

[Answer]: A

## Q2. How should the stories be grouped?

The groups become the `US{group}` numbers and shape how the work is later split into build units.

A. By step in the family's journey (start and consent, upload, review and confirm, findings, report and letters, act and give feedback), with the internal tools as separate groups
B. One group per functional requirement area (FR1 consent through FR16 test set), so each group maps one-to-one to a requirement area
C. By persona
X. Other (please specify)

[Answer]: A

## Q3. How fine-grained should the stories be?

The requirements have 127 sub-requirements. Story size decides how many stories there are and how easy each is to build and test in one to five days.

A. One story per user-visible capability, usually covering several related sub-requirements (about 45–60 stories)
B. Coarser stories (about 25–30), split further when build units are planned
C. One story per sub-requirement (about 120 or more)
X. Other (please specify)

[Answer]: A

## Q4. How should the 18 launch checks be written up?

The launch checks (for example the arithmetic, duplicate-charge, length-of-stay, room-rent, drug-price, consumable, implant and package checks) each go live only after passing their test-set gate.

A. One story per family of checks (calculation, duplicates, length of stay, room, drugs, consumables and implants, packages), with each check as its own acceptance criterion
B. One story per check (18 stories)
C. One general "run the live checks" story that points to the check catalogue, with no per-check criteria
X. Other (please specify)

[Answer]: A

## Q5. How much detail should the internal-tool stories have?

The rule library (FR14), the ops views (FR15) and the Labelled Test Set (FR16) are used only by the builder and the founder.

A. The same detail as the family-facing stories, with full Given/When/Then criteria
B. Lighter stories, about one per requirement area, with criteria only for the safety-critical rules (promotion to live, approval of rule changes, sign-in, withdrawal of bills)
X. Other (please specify)

[Answer]: B

## Q6. How should the non-functional requirements appear?

There are 21 non-functional requirements (speed, availability, accuracy, privacy, accessibility and others). The NFR Requirements stage later sets their detailed targets.

A. Add them as acceptance criteria on the stories they affect (for example the privacy rules on the upload stories), and mark the system-wide ones (availability, scalability, observability) as deferred to NFR Requirements
B. Mark all of them as deferred to NFR Requirements; stories cover functional behaviour only
C. Write a separate story for each non-functional requirement
X. Other (please specify)

[Answer]: A

## Q7. Should the stories also list what is out of this build?

The requirements already have an out-of-scope table (for example abuse controls, share-in from messaging apps and the Expert route).

A. Yes: list each out-of-scope item as a "Won't Have (this time)" story, so the boundary is visible in the story list
B. No: stories cover only what is in this build; the out-of-scope table in the requirements is enough
X. Other (please specify)

[Answer]: A

## Q8. "Discharge-urgent" cases (review finding R-01)

The speed requirement for the full report (NFR2) says cases marked discharge-urgent go first in any queue. But no requirement lets a user or the system mark a case as urgent, and the urgent-situation guidance was moved to Phase 1.5. The reviewer flagged this as untestable.

A. Drop the queue priority for this build: all cases are processed in order, and the story notes that the priority comes with the Phase 1.5 urgent-situation work
B. Ask at the start "Are you at the hospital billing desk now, about to be discharged?" and put those cases first, with no extra personal data stored
C. Treat a case as urgent when its discharge date (read from the bill or confirmed by the user) is today or missing, and put it first
X. Other (please specify)

[Answer]: A

## Q9. What may be stored with storage consent? (review finding R-02)

With consent, only certain fields are stored: line items, categories, quantities, rates, amounts, dates, hospital, city, findings and ratings (FR2.5), plus age band, gender, admission month and length of stay (FR2.6). But the feedback aggregation requirement (FR12.3) groups stored data by procedure, which is not on either list. The "what do you plan to do next" answer (FR12.2) and the "I want the Insurance Check" count (FR12.4) are also not covered.

A. Add the procedure and diagnosis from the discharge summary to the stored fields (still only with storage consent); store the "plan" answer with consent; keep the Insurance Check count as an anonymous total not linked to any session
B. Remove procedure from the aggregation; store the "plan" answer with consent; keep the Insurance Check count anonymous and unlinked
C. Store nothing beyond the current lists: drop procedure from the aggregation, keep the "plan" answer only within the session, and keep the Insurance Check count anonymous and unlinked
X. Other (please specify)

[Answer]: A

## Q10. A measurable test for the phone and network target (review finding R-03)

The target is an 8 GB RAM Android phone on typical 4G (NFR11), but there is no test network profile or load-time threshold. The reviewer also noted this is narrower than the PRD's "low and mid-range phones on patchy hospital networks".

A. Write the test into the stories now: a throttled connection of about 9 Mbps down, 1.5 Mbps up and 170 ms latency, with each screen usable within 4 seconds; record the narrower device target as an accepted risk, to be revisited if drop-off on weaker phones shows up after launch
B. Use the same profile and threshold, but test also on a slower 3G-like profile (about 1.6 Mbps down, 300 ms latency) so patchy hospital networks are covered
C. Leave the network profile and thresholds to the NFR Requirements stage; stories only state the target
X. Other (please specify)

[Answer]: A

## Q11. Sign-in for the internal tools (review finding R-04, part 1)

The internal tools need "strong sign-in" (FR15.2), but the term is not defined. These tools can reach the Labelled Test Set, which holds real (redacted) bills.

A. Two-step sign-in required for every internal user: a passkey or an authenticator-app code; no SMS codes
B. Password plus a two-step code from an authenticator app
C. Leave the definition to the NFR Requirements stage
X. Other (please specify)

[Answer]: C

## Q12. A measurable test for plain language (review finding R-04, part 2)

The report must use plain language with inline explanations of medical and insurance terms (FR10.4). The test refers to a glossary that does not exist yet, and the reading-level measure is open (OQ4). The PRD suggests "roughly school-leaver level".

A. Create a glossary of medical and insurance terms as part of the build; every glossary term in a report must have an inline explanation; and report text must score at least 60 on the Flesch reading-ease scale (roughly school-leaver level), checked automatically
B. The glossary and inline-explanation check only; leave the reading-level measure to NFR Requirements
X. Other (please specify)

[Answer]: A

## Q13. Time allowed to apply a regulatory change to the rules (review finding R-04, part 3)

A process must track changes from IRDAI and NPPA and update the rules within a target window (FR14.3). The window is open (OQ5).

A. 30 days from the circular or notification being published
B. 14 days from publication
C. Leave it open with the founder as owner, to be set in NFR Requirements
X. Other (please specify)

[Answer]: X. Other: For Phase 1, lets not update the rules. Updating of the rules can be moved to phase 2.

## Q14. Launching if some of the 18 checks don't pass in time (review finding R-05)

Each launch check must be tested on at least 10 bills with the error and 30 without (FR16.12). Some checks, such as the implant-cap check (IMP-02) and the package check (PKG-01), may never get 10 real bills with the error.

A. Hold launch until all 18 checks pass
B. Launch with the checks that pass; the rest stay hidden (shadow mode), and the report lists which checks did not run
C. Launch with the checks that pass, but only if a minimum number of them pass (please say how many under Other)
X. Other (please specify)

[Answer]: B

## Q15. The three minor review findings (R-06, R-07, R-08)

- **R-06:** the closed beta keeps original bills, but there is no consent wording, storage location or blacking-out step for them.
- **R-07:** it is unclear whether the report PDF is built on the device for desktop too, and the "daily check" for 3-month deletion has no source.
- **R-08:** there is no minimum test-set size for the bill-reading accuracy targets (95% and 90%).

A. Resolve them in the stories with these defaults: beta bills need their own consent screen, go to a store separate from product data, and are blacked out before entering the test set; the PDF is built on the device on both phone and desktop; the deletion test becomes "no stored record is older than 3 months"; bill-reading accuracy is measured on at least 100 bills for PDFs and 100 for photos
B. Note them as open items for Domain Design and NFR Requirements; stories do not decide them
X. Other (please specify)

[Answer]: A

## Q16. Follow-up to Q13: what does "not update the rules in Phase 1" cover?

Your answer to Q13 was: "For Phase 1, lets not update the rules. Updating of the rules can be moved to phase 2." That could cover only the process for tracking IRDAI and NPPA changes (FR14.3), or the rule library's editing as well. NPPA ceiling prices are revised from time to time, and without editing, a stale price could produce a wrong "Likely error" finding until Phase 2.

A. Only the regulatory-change tracking process (FR14.3) moves to Phase 2. The rule library still lets the builder or founder fix a rule, change a reference price, and switch a check between live, shadow and off without a release
B. Rules and reference data are fixed at launch: editing them (FR14.1) moves to Phase 2. Checks can still be switched between live, shadow and off, so Wave 2 and Wave 3 checks can go live once they pass their test
C. Everything is fixed at launch: rules, reference data and check states. Wave 2 and Wave 3 checks also move to Phase 2
X. Other (please specify)

[Answer]: A

## Mob follow-up questions

The designer, developer and quality engineer reviewed the draft stories. The questions below are the points where both sides are reasonable and you should decide. Their technical corrections are folded into the stories directly. Full reviews: `contributions/` in this folder.

## Q17. Knowing whether the bill is interim or final, and whether it is already paid

Several stories depend on this: which interim bills are checked, the "you can still raise this after paying" wording, and refund-first ordering. The rough mockups had a "When are you checking?" screen, but no story asks it.

A. Read it from the documents (interim or final bill, receipts) and ask the user to confirm it on the case-details screen (US3.1), with no extra question screen
B. Ask it as its own short question before upload ("still in hospital", "at discharge", "already paid")
X. Other (please specify)

[Answer]: B

## Q18. What counts as "idle" for the 30-minute timeout

As written, a family waiting for a report, or standing in a queue, could lose everything at 30 minutes.

A. The timer pauses while a report is being prepared. Once the report is ready, only real taps and typing count as activity; the page checking on progress in the background never keeps a session alive
B. A strict 30 minutes from the last tap, always, even during processing (privacy first); the 25-minute warning still offers "Keep going"
X. Other (please specify)

[Answer]: A

## Q19. What the first-look summary shows before details are confirmed

The first look appears before the user confirms uncertain fields, so a misread digit could show an alarming finding that later disappears.

A. Show the number of things worth looking at and their topics, but no "Likely error" label and no rupee total until details are confirmed
B. Show findings with their class and rupee amounts straight away, marked "may change"
X. Other (please specify)

[Answer]: A

## Q20. How many screens from the start to the upload

The consent stories (explainer, privacy notice, consent, relationship, minor or adult declarations) could become five or more screens. The rough mockups used two.

A. At most 2 screens before upload, as in the rough mockups: (1) what this does plus the privacy notice; (2) consent, relationship and any declaration
B. At most 3 screens before upload
C. No cap; one step per screen
X. Other (please specify)

[Answer]: A

## Q21. Script limit for the report PDF tool

Each page may load at most 300 KB of JavaScript. The tool that builds the PDF on the device loads only when the user taps "Save or share".

A. The PDF tool has its own limit (200 KB compressed), loaded only after the tap; the 300 KB limit covers what each page loads at first
B. The PDF tool counts within each page's 300 KB limit
X. Other (please specify)

[Answer]: A

## Q22. How far upload resume goes

The requirement says uploads resume after a dropped connection.

A. Resume while the page stays open; if the page is reloaded or the phone closes the tab, the user picks the unfinished files again, and finished files are not re-sent
B. Also survive a reload by keeping copies of unfinished files on the device until they are sent
X. Other (please specify)

[Answer]: A

## Q23. What "fix a rule without a release" covers

Your answer to Q16 keeps rule fixes without a release. Changing data (reference prices, thresholds, check settings, live/shadow/off) without a release is cheap; changing how a check works without a release needs a rules language and is costly.

A. Data and settings only: prices, thresholds, check settings and live/shadow/off change without a release; changes to how a check works go through a normal release
B. Both data and check logic change without a release
X. Other (please specify)

[Answer]: A

## Q24. Who approves a rule change

There are two of you. The draft lets the person who made a change approve it, with the self-approval logged.

A. The other person approves; self-approval only as a logged emergency exception, reviewed by the other person afterwards
B. Self-approval allowed and logged, for speed
X. Other (please specify)

[Answer]: B

## Q25. The optional reason given with a finding rating

Users can rate each finding and add a reason. Free text may contain names or phone numbers, and it would be stored with consent.

A. Offer only fixed reasons (for example "the hospital already explained this", "the amount is wrong"); no free text
B. Allow free text but never store it; it is used only within the session
C. Allow free text and store it, after removing names, phone numbers and other identifiers automatically
X. Other (please specify)

[Answer]: A

## Q26. Low-confidence findings outside the folded-away group

Low-confidence findings are folded away under "Other things you may ask about". The stories do not say whether they can also appear elsewhere.

A. Never in the Top 3 or the Billing Desk Summary, and not counted in the "likely errors" amount of the Bill Breakdown (they count under "worth asking about")
B. Allowed in the Top 3 and the summary when nothing stronger exists, still marked low confidence
X. Other (please specify)

[Answer]: A

## Q27. Dates on stored line items

With storage consent, the requirements keep only the admission month, but they also keep "all line items", which carry exact dates. The two conflict.

A. Store each line's date as the day of the stay (day 1, day 2, ...) rather than the calendar date; checks and grouping still work
B. Store the exact dates; the admission month rule applies only to the case header
C. Store line items without dates
X. Other (please specify)

[Answer]: A

## Q28. Follow-up: where the "When are you checking?" question goes

Your answers conflict slightly: Q17 asks the bill-stage question up front ("still in hospital", "at discharge", "already paid"), and Q20 allows at most 2 screens before upload.

A. Put it on screen 2, alongside consent, relationship and any declaration; still 2 screens
B. Put it at the top of the upload screen, so it is answered before the first file is added; still 2 screens before upload
C. Allow it as a third short screen before upload
X. Other (please specify)

[Answer]: B

## Consolidated Summary Confirmation

- Personas: four customer personas (the family member at discharge, the remote family manager, the self-payer who will claim reimbursement later, and the bereaved family) plus one internal persona for the builder and the founder [Q1].
- Stories are grouped by step in the family's journey (start and consent, upload, review and confirm, findings, report and letters, act and give feedback), with the internal tools as separate groups [Q2].
- One story per user-visible capability, about 45–60 stories [Q3].
- The 18 launch checks are written as one story per check family, with each check as its own acceptance criterion [Q4].
- Internal-tool stories are lighter: about one per area, with full criteria only for promotion to live, rule-change approval, sign-in and bill withdrawal [Q5].
- Non-functional requirements appear as criteria on the stories they affect; availability, scalability and observability are deferred to NFR Requirements [Q6].
- Out-of-scope items are listed as "Won't Have (this time)" stories [Q7].
- R-01: the discharge-urgent queue priority is dropped for this build; all cases are processed in order, and the priority returns with the Phase 1.5 urgent-situation work [Q8].
- R-02: with storage consent, procedure and diagnosis from the discharge summary and the "what do you plan to do next" answer are also stored; the "I want the Insurance Check" count stays an anonymous total not linked to any session [Q9].
- R-03: phone and network tests use a throttled connection of about 9 Mbps down, 1.5 Mbps up and 170 ms latency, with each screen usable within 4 seconds; the narrower device target is recorded as an accepted risk, to be revisited if drop-off on weaker phones shows up after launch [Q10].
- R-04: the meaning of "strong sign-in" for internal tools is left to NFR Requirements [Q11].
- R-04: a glossary of medical and insurance terms is built; every glossary term in a report has an inline explanation, and report text scores at least 60 on Flesch reading ease, checked automatically [Q12].
- R-04: the process for tracking IRDAI and NPPA changes and updating rules within a window moves to Phase 2. The rule library still lets the builder or founder fix a rule, change a reference price, and switch a check between live, shadow and off without a release [Q13], [Q16].
- R-05: launch goes ahead with the launch checks that pass their test; the rest stay hidden (shadow mode), and the report lists which checks did not run [Q14].
- R-06 to R-08: beta bills have their own consent screen, go to a store separate from product data, and are blacked out before entering the test set; the report PDF is built on the device on both phone and desktop; the deletion test becomes "no stored record is older than 3 months"; bill-reading accuracy is measured on at least 100 PDF bills and 100 photographed bills [Q15].

- Bill stage (still in hospital, at discharge, already paid) is asked up front, at the top of the upload screen before the first file is added, so there are still at most 2 screens before upload [Q17], [Q28].
- The 30-minute idle timer pauses while a report is being prepared; after that only real taps and typing count as activity, and background progress checks never keep a session alive [Q18].
- The first look shows the number of things worth looking at and their topics, with no "Likely error" label and no rupee total until details are confirmed [Q19].
- At most 2 screens before upload: (1) what this does plus the privacy notice; (2) consent, relationship and any declaration [Q20].
- The on-device PDF tool has its own limit of 200 KB compressed and loads only after the tap; the 300 KB limit covers what each page loads at first [Q21].
- Upload resumes while the page stays open; after a reload the user picks only the unfinished files again, and finished files are not re-sent [Q22].
- "Fix a rule without a release" covers data and settings only (prices, thresholds, check settings, live/shadow/off); changes to how a check works go through a normal release [Q23].
- The person who makes a rule change may approve it, and the self-approval is logged [Q24].
- Ratings offer fixed reasons only, with no free text [Q25].
- Low-confidence findings never appear in the Top 3 or the Billing Desk Summary, and count under "worth asking about", not "likely errors", in the Bill Breakdown [Q26].
- Stored line items carry the day of the stay (day 1, day 2, ...) instead of the calendar date [Q27].

Does this all look correct before I generate the user stories?

- Looks correct
- Request changes

[Answer]: Looks correct
