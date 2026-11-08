# Team-Level Rules

> This team's affirmed practices and corrections. Loaded after `org.md` as
> strict-additive guidance; contradictions with broader policy are rejected.
> Populated by the practices-discovery affirmation gate. Edit at the gate,
> not directly.

## Way of Working

- We use trunk-based development. `main` is the only long-lived branch. Each Bolt runs on a short-lived branch in its own worktree, based on `main` and merged back into `main`, normally within one to two days.
- The code host and the service that runs our automatic checks are not chosen yet. We choose them before the first build slice starts, because checks before merge need a remote repository.
- Once the remote exists, finished work reaches `main` only through a pull request. The automatic checks must pass first, then we squash-merge, so each Bolt becomes one commit on `main` named by its Bolt slug. The full Bolt history stays on the source branch until the worktree is discarded.
- With one builder there is no second human reviewer. The review before merge is the workflow's approval step plus the automatic checks, including the security checks, which act as our independent reviewer.
- We commit the workflow records (the `aidlc/` tree) and the code at the end of each phase.
- Secrets are scanned on the builder's machine before each commit and again by the automatic checks. If a secret is ever committed, we revoke and rotate it first, then remove it from history. We never commit passwords, API keys, `.env` files or other credentials; secrets for CI and hosting live only in the platform's secret store.
- The automatic checks install dependencies with `npm ci` from the committed lockfile, and we review lockfile changes like code.
- Before running any `npm` command we check `.nvmrc` and run `nvm use`, so the pinned Node version (v24.19.0) is used.
- Before writing Next.js code we read the relevant guide in `node_modules/next/dist/docs/`, because this Next.js version (16.4.3) differs from older versions.
- Real patient bills, including redacted ones, are never placed anywhere the AI coding assistant or its helper tools can read: not in the repository working tree and not in any folder the assistant can open.
- Next.js usage reporting is turned off on the builder's machine, on the build servers and on every host (`NEXT_TELEMETRY_DISABLED=1`). The development-only setting `experimental.agentFeedback` is turned off.
- The AI assistant's helper tools (MCP servers) are pinned to fixed versions, and any the project does not need are removed.

## Walking Skeleton

- Yes. We build a thin end-to-end slice first: a minimal version that runs the whole way through, built to prove the pieces connect before the real features go in.
- Bolt 1 is solo and gated. The founder approves it before the remaining Bolts run.
- The slice covers: uploading a bill; a stub report shown on screen and offered as a download; deletion of the original file after reading; the test tools (Jest and Playwright) set up with one unit test and one end-to-end test; and the automatic checks running.
- The slice also fixes what currently blocks the checks: `.claude/**` and `aidlc/**` are added to the ESLint ignores, a `typecheck` script runs `next typegen && tsc --noEmit`, and a Node `engines` field guards against the wrong Node version.
- The security checks are part of the slice from the start: the secret scan, the dependency and package-signature checks, and the code security scan (see Deployment and Code Style).
- The slice is not deployed anywhere until hosting is decided.
- After Bolt 1 ships, we decide how the remaining Bolts run (continue on their own, or gate every Bolt).

## Testing Posture

- **Methodology**: test-after
- **Ordering**: For every testable layer, we implement the code first, then write and run that layer's tests, and the Bolt cannot merge until they pass.
- Tests are a first-class deliverable in every Bolt. Test Strategy is Comprehensive.
- Coverage floor: at least 80% of lines covered overall, measured on application source. The automatic checks run the tests before every merge. We never lower the floor or skip a check to get a step through.
- Tools: Jest (configured through `next/jest`) for unit and component tests; Playwright for end-to-end tests.
- `async` Server Components cannot be unit-tested with Jest, so they are covered by end-to-end tests.
- Tests, fixtures, logs, screenshots and git never contain real bills. The code test suite uses made-up (synthetic) bills only.
- Tests confirm that the original uploaded file is deleted once it has been read, including when reading fails, and that no copy is left in temporary folders, logs or error reports.
- Accuracy is checked separately from the code test suite, against the Labelled Test Set. Targets: at least 95% of lines read correctly on digital PDFs and at least 90% on good photos; at least 90% of "Likely error" findings and at least 75% of "Worth asking" findings correct.
- The accuracy check runs before a check goes live and on every change to prompts, models or rules, not on every merge. Real bills in the test set are kept outside git and out of the AI assistant's reach, and accuracy runs happen only on services that have passed the data-processor check.
- Speed and capacity get a basic timing and load test in Build and Test, against a test environment with made-up bills (first insight, full report, availability and 10x pilot volume, at a basic level).

## Change Control

<!-- Affirmed by the team. Mode: strict or relaxed. Strict here holds for every intent and cannot be changed from chat. -->

## Deployment

- We have two environments: staging and production.
- Every merge into `main` deploys to staging automatically.
- Production releases need the founder's approval, given after the checks pass and a quick post-deploy check succeeds. Every deployment has a written rollback plan.
- No real bills go to any environment until the founder has decided data residency and hosting.
- Hosting is not decided. Vercel is a working assumption only, so our deployment practice does not depend on any particular host.
- Nothing is deployed until hosting is decided, including the first build slice.
- We mark releases with git tags on `main`, not with release branches.
- Before every merge, the automatic checks block on high or critical dependency problems (`npm audit`) and on invalid package signatures. Medium findings are reported but do not block.

## Code Style

- We use Prettier for formatting, with `eslint-config-prettier` so ESLint and Prettier do not conflict. Formatting is checked by the automatic checks.
- ESLint 9 (flat config, `eslint-config-next` `core-web-vitals` and `typescript` presets) is our linter, and we follow its configuration. Lint, type-check and format failures block the merge.
- TypeScript runs in `strict` mode with the `@/*` path alias.
- From the first build slice, a code security scan runs before every merge and blocks on high or critical findings. Medium findings are reported only.
- Any security finding we accept is recorded with a written reason and expires within 90 days.
- Naming follows TypeScript idiom (camelCase for variables and functions, PascalCase for React components and types) and Next.js file conventions for routes.
- A user's bill data or findings never go into a shared cache, a prebuilt page or prefetched output.
- File layout, layer boundaries, error handling and how money is represented are left to the design stages.

## Forbidden

<!-- Team-specific forbidden patterns -->

## Mandated

<!-- Team-specific mandates -->

## Corrections

<!-- Self-learning loop appends here. -->
