# Practices Discovery — Questions

These questions settle how the build will be run: branching and commits, a first thin slice, testing, deployment and code style. The release engineer drafted the practices from the repository, and the quality engineer, developer and security engineer each reviewed the draft separately. The questions below cover only what the evidence could not decide.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise` (80% line-coverage floor and tests in CI before merge; comprehensive test strategy; the scope turns the thin first slice on by default).
- Draft practices: `team-practices.md`, `discovered-rules.md` and `evidence.md` in this folder.
- Independent reviews: `contributions/aidlc-quality-agent.md`, `contributions/aidlc-developer-agent.md`, `contributions/aidlc-devsecops-agent.md`.
- Code knowledge base: `aidlc/spaces/default/codekb/hospital-bill-audit-app/`.

Already settled, so not asked again:

- **The team.** You build alone with this AI-assisted workflow; the founder decides scope and priority and approves releases (`approval-handoff-questions.md`, Q6).
- **Hosting is not decided.** Your words: "Mostly Vercel. But I would like to decide at a later point." Vercel stays a working assumption. Data residency, hosting and the bill-correction measure are decided by the founder before the build starts (`approval-handoff-questions.md`, Q7).
- **What exists today.** An unmodified Next.js 16 starter: no tests, no CI, no remote repository, one `main` branch with two commits, and about a week of workflow records not yet committed.

---

## Q1. Where will the code live, and which service runs the automatic checks?

There is no remote repository yet, so nothing can run checks before a merge. The first build slice needs one.

A. GitHub, with GitHub Actions running the checks
B. GitLab, with GitLab CI running the checks
C. Not decided yet; decide before the first build slice starts
X. Other (please specify)

[Answer]: C

## Q2. How does finished work get into `main`?

With one builder there is no second reviewer. The draft proposes that the review before merge is this workflow's approval step plus the automatic checks passing. Each piece of work is squashed into one commit on `main`.

A. Through pull requests once the remote exists: the checks must pass, then squash-merge
B. Merge locally after the approval step, then push; the checks run on `main` after the push
X. Other (please specify)

[Answer]: A

## Q3. When should the workflow records and code be committed?

About a week of planning records is still uncommitted, which is the biggest risk of losing work right now.

A. Commit the current work now, then commit at every stage approval
B. Commit at the end of each phase
C. I'll handle commits myself
X. Other (please specify)

[Answer]: B

## Q4. Build a thin end-to-end slice first? A walking skeleton is a minimal version that runs the whole way through, built first to prove the pieces connect before the real features go in.

The draft slice: upload a bill, see a stub report on screen and as a download, the original file deleted after reading, the test tools set up with one unit test and one end-to-end test, and the automatic checks running. It would also fix what currently blocks the checks: lint failing on the workflow tooling, type-checking needing generated route types, and no Node version guard. The founder approves the slice before the rest of the build continues.

A. Yes, as drafted, and it deploys to a test environment using made-up bills only
B. Yes, as drafted, but no deployment until hosting is decided
C. No; the first piece of work is an ordinary feature
X. Other (please specify)

[Answer]: B

## Q5. When are tests written?

The bill checks decide what a patient is told, and the made-up test bills come with a list of the errors planted in them, which gives ready-made expected results.

A. Tests first for the bill checks, money calculations and report rules; tests after the code for screens, upload handling and wiring
B. Tests after the code everywhere (the framework default)
C. Tests first everywhere
X. Other (please specify)

[Answer]: B

## Q6. How strict should the test coverage floor be?

The enterprise rules require at least 80% of lines covered, and the floor is never lowered to get a step through. A wrong branch in a bill check produces a wrong finding.

A. 80% of lines overall, plus 90% of branches in the bill-check and money-calculation code
B. 80% of lines overall only
C. A higher floor everywhere (say what under X)
X. Other (please specify)

[Answer]: B

## Q7. Which test tools should we use?

The first build slice installs them, so leaving the choice to a later stage would be too late.

A. Vitest for unit and component tests, and Playwright for end-to-end tests with automatic accessibility checks (axe)
B. Jest for unit and component tests, and Playwright for end-to-end tests
X. Other (please specify)

[Answer]: B

## Q8. How should bill-reading and finding accuracy be checked against the test set of labelled bills?

The targets are at least 95% of lines read correctly on digital PDFs and at least 90% on good photos. For findings, at least 90% of "Likely error" findings and at least 75% of "Worth asking" findings must be right. Real bills, even redacted ones, can be withdrawn by the person who gave them, and git history would keep them anyway.

A. Run it before a check goes live and on every change to prompts, models or rules, not on every merge. Real bills are kept outside git and outside the AI assistant's reach, and runs happen only on services that have passed the data-processor check
B. Run it on every merge using made-up bills only; run on real bills by hand before a check goes live
X. Other (please specify)

[Answer]: A

## Q9. Where are the speed and capacity targets checked?

The plan skips the Performance Validation stage, yet the targets are measurable: a first insight within 5 minutes for 90% of audits (10 minutes for photographed multi-page bills), the full report within 15 minutes for 90%, 99.5% availability, and handling 10 times pilot volume.

A. A basic timing and load test in Build and Test, against a test environment with made-up bills
B. Defer it and record it as a known gap
C. Add the Performance Validation stage back into the plan
X. Other (please specify)

[Answer]: A

## Q10. Which environments do we need, and who approves going live?

A. A preview for each change, one staging environment and production. Each merge deploys to staging automatically; the founder approves production after the checks pass and a quick post-deploy check succeeds. No real bills anywhere until residency and hosting are decided
B. Staging and production only, with the same approval and real-bill rule
X. Other (please specify)

[Answer]: B

## Q11. Which security checks should block a merge, and from when?

With no second reviewer, automatic security checks are the only independent check before a merge. Today: no secrets in the repository, 16 moderate warnings in development-only tools, none in what ships, and no security tools installed.

A. From the first build slice: scan for secrets (on your machine before each commit and again on the server), block on high or critical dependency problems, verify package signatures, and run a code security scan that blocks on high or critical findings. Medium findings are reported only; any accepted finding needs a written reason and expires within 90 days
B. Add these checks later, before launch
X. Other (please specify)

[Answer]: A

## Q12. How do we keep real bills away from usage reporting and the AI tools?

Next.js sends anonymous usage data from your machine, and from build servers unless turned off. A development-only setting (`experimental.agentFeedback`) asks the AI assistant to collect "anonymized facts" for feedback reports. Your project settings send workspace content straight to the AI provider (`CLAUDE_CODE_USE_BEDROCK` is `0` in `.claude/settings.json`), and four AWS helper tools load their latest version each time they start.

A. Turn off Next.js usage reporting on your machine, the build servers and every host; turn off `agentFeedback`; never put real bills anywhere the AI assistant can read; pin the helper tools to fixed versions and remove any the project does not need
B. Turn usage reporting off on build servers and hosts only, and keep the rest as it is
X. Other (please specify)

[Answer]: A

## Q13. How much code structure should we fix now?

The first build slice will set these conventions whether or not they are decided.

A. Prettier formatting plus a few security lint rules; route files only in `app/`, with check logic in `lib/audit`, server-only code in `lib/server`, shared screens in `components`, and end-to-end tests in `e2e`. Bill-check logic is plain TypeScript with no framework or I/O. Every form action and endpoint checks its input first. Expected failures are returned as results. Money is held in whole paise. No bill contents or patient details appear in logs
B. Prettier and the linter only; leave structure to the design stages
X. Other (please specify)

[Answer]: B

## Q14. Which of these should become hard rules that every later step must follow?

The candidates:

- always run `nvm use` before any `npm` command;
- always read the bundled Next.js docs before writing Next.js code;
- always delete the original uploaded file once it has been read, including when reading fails, leaving no copy in temporary folders, logs or error reports;
- never commit passwords, keys or `.env` files;
- never put a user's bill data or findings in a shared cache, a prebuilt page or prefetched output;
- never use real bills in tests, fixtures, logs, screenshots or git;
- never lower the coverage floor or skip a check to get a step through;
- always get the founder's approval, a written rollback plan and a quick post-deploy check before a production release.

A. All of them
B. All except some (name them under X)
C. Keep them as practices, not hard rules
X. Other (please specify)

[Answer]: C

## Consolidated Summary Confirmation

- Code host (Q1): not decided yet; it is decided before the first build slice starts.
- Merging (Q2): through pull requests once the remote exists; the checks must pass, then squash-merge into `main`.
- Commits (Q3): commit the workflow records and code at the end of each phase.
- First slice (Q4): yes, build a thin end-to-end slice first, as drafted, but with no deployment until hosting is decided. The founder approves it before the rest of the build continues.
- Test timing (Q5): tests are written after the code everywhere.
- Coverage (Q6): at least 80% of lines covered overall.
- Test tools (Q7): Jest for unit and component tests; Playwright for end-to-end tests.
- Accuracy (Q8): checked against the labelled bills before a check goes live and on every change to prompts, models or rules, not on every merge. Real bills stay outside git and out of the AI assistant's reach, and runs happen only on services that passed the data-processor check.
- Speed and capacity (Q9): a basic timing and load test in Build and Test, against a test environment with made-up bills.
- Environments (Q10): staging and production only. Merges deploy to staging automatically; the founder approves production after the checks pass and a quick post-deploy check. No real bills anywhere until residency and hosting are decided.
- Security checks (Q11): from the first slice, a secret scan before each commit and on the server, blocking on high or critical dependency problems, package signature checks, and a code security scan that blocks on high or critical findings. Medium findings are reported only; any accepted finding needs a written reason and expires within 90 days.
- Data safety (Q12): Next.js usage reporting off on your machine, build servers and hosts; `agentFeedback` off; real bills never placed where the AI assistant can read them; helper tools pinned to fixed versions, with unneeded ones removed.
- Code structure (Q13): Prettier and the linter only; structure is left to the design stages.
- Hard rules (Q14): none. The eight candidates stay as practices, not hard rules.

Does this all look correct before I generate the artifact?

- Looks correct
- Request changes

[Answer]: Looks correct
