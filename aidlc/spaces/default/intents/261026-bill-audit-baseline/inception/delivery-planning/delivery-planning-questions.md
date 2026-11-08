# Delivery Planning — Questions

This stage plans the order of the build. The build runs as a series of Bolts: a Bolt is one build pass over a piece of the work that ends in something that runs and is merged. The 12 units and their dependencies are fixed (`inception/units-generation/`). This stage chooses a path through them: what goes first, how big each Bolt is, and what outside decisions could hold it up.

Each question marks the option I recommend, with a one-line reason (project practice).

## Already settled, not asked again

- **Bolt 1 is the walking skeleton**: the thinnest slice that runs end to end, built to prove the parts connect before real features go in. It covers upload, a stub report shown and offered as a download, deletion of the original after reading, Jest and Playwright with one test each, the automatic checks including the security checks, the ESLint ignores, the `typecheck` script and the Node `engines` field. The founder approves it before the next Bolt starts (`team.md`, Walking Skeleton).
- **After Bolt 1, we decide how the rest runs**, either on their own or gated one by one (`team.md`). That is asked after Bolt 1, not here.
- **One builder plus the founder.** Every Bolt is built by the builder working with the AI assistant, and the founder approves. There are no other teams (project rule; Team Formation was skipped).
- **Units with no dependency between them may be built side by side** (Units Generation D-UG7).
- **Bolt branches are short-lived and merged within one to two days** (`team.md`, Way of Working).
- **Carried from Units Generation (R-04):** the first build slice cuts across several units and also owns repository scaffolding, test tooling and the automatic checks. This is planned into Bolt 1 below.

---

## Q1. What to build after the walking skeleton

The biggest unknown is whether bill reading and the checks reach their accuracy targets (95% of lines on PDFs, 90% on photos; 90% and 75% finding precision). The reader is not chosen yet (requirements OQ8).

A. (Recommended) Risk first, then value: right after the skeleton, the bill-reading adapter, the reader trial on synthetic bills and the first check family; then the family journey to a first useful report; then the remaining checks, letters and feedback; the internal tools and test-set workflow before launch. This tests the riskiest assumption before the most code depends on it
B. Value first: complete the family journey end to end with simple checks, then deepen reading and checks
C. Score every piece with a formal model (WSJF: value plus urgency plus risk reduction, divided by size) and follow the scores
X. Other (please specify)

[Answer]: A

## Q2. How big one Bolt is

Most units are large (several are 1 to 2 weeks or more of work), but team practice merges each Bolt branch within 1 to 2 days.

A. (Recommended) Thin slices: each Bolt is 1 to 3 stories cutting across whichever units they need, small enough to merge in 1 to 2 days; Bolts are grouped into a handful of milestones so progress stays readable. This is the only option that keeps to the 1-to-2-day branch rule
B. One unit per Bolt, even when that keeps a branch open for weeks
C. Several related units per Bolt
X. Other (please specify)

[Answer]: A

## Q3. Building while hosting and data residency are undecided

Nothing is deployed and no real bill reaches any environment until the founder decides hosting and residency (requirements OQ1, OQ2). The background worker, scheduler, stores and the test set's real bills all depend on that decision.

A. (Recommended) Build and test everything locally with synthetic bills and local stand-ins for the stores, job runner and scheduler, behind the interfaces already in the contracts; mark Bolts that need a real host as "deploy-blocked", and set a target to decide hosting before the first deployable milestone. Work keeps moving, and the stand-ins are swapped without code changes elsewhere
B. Pause Construction until hosting is decided
C. Take Vercel as decided now, to unblock everything
X. Other (please specify)

[Answer]: A

## Q4. Code host and automatic-check service

Team practice needs these chosen before Bolt 1 starts, because the checks before merge need a remote repository.

A. (Recommended) GitHub with GitHub Actions: free for a private repository at this size, built-in secret scanning, dependency alerts and code scanning, and widely supported by the tools we plan to use
B. GitLab with GitLab CI
C. Decide later (Bolt 1 then runs the checks only on the builder's machine until this is chosen)
X. Other (please specify)

[Answer]: A

## Q5. What worries you most about this build (select all that apply)

Whatever you pick is tackled early and watched closely.

A. (Recommended) That bill reading and the checks will not reach their accuracy targets; this decides whether checks can go live at launch
B. That privacy guarantees (deletion, no shared caching, de-identification) are broken by a bug
C. That the build takes too long for one builder
D. That hosting or the data-residency decision arrives late
E. That families find the report hard to understand or act on
X. Other (please specify)

[Answer]: A

## Q6. Building one unit fully, or one design stage across all units

Construction has five steps per unit: Functional Design, NFR Requirements, NFR Design, Infrastructure Design and Code Generation.

A. (Recommended) Unit by unit: each unit goes through all five steps before the next unit starts, following the Bolt order. Working code arrives early and in the planned order, which suits a skeleton-first, risk-first plan; the trade-off is no parallel batch building of independent units
B. Stage by stage: Functional Design for all 12 units, then NFR Requirements for all, and so on, with code last. Designs are consistent across units, and independent units can be built in parallel, but no code is written until every design is done
X. Other (please specify)

[Answer]: A

## Q6a. Follow-up: fitting the walking skeleton into unit-by-unit building

Two answers pull against each other. Unit by unit (Q6 A) builds each unit fully before the next, but the walking skeleton (Bolt 1) runs through seven units: contracts, session-privacy, document-intake, bill-analysis, case-processing, findings-report and family-ui. Building all seven fully first would push the first end-to-end run late. Unit by unit also means the earlier "build independent units side by side" (Units Generation Q8) will not be used.

A. (Recommended) Bolt 1 is built with the contracts unit: the real interfaces, plus thin stand-in versions of the six other units on the skeleton path, each behind its contract (document-intake's stand-in already deletes originals for real). The skeleton runs end to end in Bolt 1. Units are then built one by one in risk-then-value order, each replacing its stand-in. The contracts make the swap safe
B. Switch to stage by stage after all: all designs first, then code, with independent units built in parallel
C. Keep unit by unit and build the seven skeleton units fully first; the end-to-end run comes when the seventh is done
X. Other (please specify)

[Answer]: A

## Consolidated Summary Confirmation

- Q1: risk first, then value: after the walking skeleton, the bill-reading adapter, the reader trial on synthetic bills and the first check family; then the family journey to a first useful report; then the remaining checks, letters and feedback; the internal tools and test-set workflow before launch (A).
- Q2: thin slices: each Bolt is 1 to 3 stories across the units they need, mergeable in 1 to 2 days, grouped into milestones (A).
- Q3: build and test locally with synthetic bills and local stand-ins for stores, job runner and scheduler; Bolts needing a real host are marked deploy-blocked; target a hosting decision before the first deployable milestone (A).
- Q4: GitHub with GitHub Actions as code host and automatic-check service (A).
- Q5: the biggest worry is that bill reading and the checks miss their accuracy targets (A); no other worry selected.
- Q6 and Q6a: unit by unit: each unit goes through all five Construction steps before the next. Bolt 1 is built with the contracts unit, with thin stand-ins for the six other skeleton units (document-intake's stand-in deletes originals for real), so the skeleton runs end to end; units are then built one by one in risk-then-value order, each replacing its stand-in; independent units are not built in parallel (A, A).
- Confirmed team plan (not asked): every Bolt is built by the builder with the AI assistant and approved by the founder; no other teams.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
