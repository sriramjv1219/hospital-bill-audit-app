**Collaborator:** aidlc-devsecops-agent

## Contribution

Scope of this review: lint and format rules, static and dynamic security testing (SAST, DAST), secret scanning, dependency scanning and supply-chain controls, and the security-practice questions the founder must decide. Every finding below is labelled **[Fact]** (checked in this review) or **[Proposal]** (a suggested practice for the interview). The lead can lift each block into `team-practices.md`, `discovered-rules.md` or `evidence.md` as marked.

### 1. New evidence for `evidence.md` (security baseline)

| # | What was inspected | Path or command | Finding | Bearing on practice |
|---|---|---|---|---|
| S1 | Dependency vulnerabilities | `nvm use` (v24.19.0), then `npm audit --json` | 16 **moderate**, 0 high, 0 critical. All come from one advisory, `minimatch` GHSA-g3hv-75wg-m249 (denial of service through glob patterns), pulled in by the `eslint` / `eslint-config-next` / `typescript-eslint` chain. `npm audit --omit=dev` reports **0** vulnerabilities. | Nothing shipped to users is affected today. npm's suggested fix (`eslint@4.0.0`, a semver-major downgrade) is wrong and must not be applied with `npm audit fix --force`. A sensible gate blocks on high and critical and reports moderate. |
| S2 | Registry signatures | `npm audit signatures` | 358 packages have verified registry signatures; 86 have verified provenance attestations; no invalid signatures reported. | A cheap supply-chain check that can run in CI. |
| S3 | Lockfile provenance | `package-lock.json` | Lockfile v3; all 437 `resolved` URLs point to `https://registry.npmjs.org`; every entry has an `integrity` hash. At least two packages run install scripts (`unrs-resolver` and one other flagged `hasInstallScript`). No `.npmrc` exists. | `npm ci` from the lockfile gives reproducible installs. Install scripts are the main install-time risk; any new package with an install script deserves a look before it is added. |
| S4 | Secrets in the repository | `git ls-files`; keyword search of `git log --all -p`; `.gitignore` | No credentials in tracked files or history (the matches are documentation prose only). `.env*`, `*.pem` and `.claude/settings.local.json` are gitignored. `.mcp.json` reads `CONTEXT7_API_KEY` from the environment. | Clean starting point. A heuristic search is not a scanner; a real scan (for example Gitleaks) should run once over full history before the first push to any remote. |
| S5 | Security tooling installed | `which gitleaks trufflehog semgrep git-secrets osv-scanner` | None installed. No pre-commit hooks of the project's own (only the AI-DLC Claude Code hooks). | Every security check must be added; nothing exists to build on. |
| S6 | Next.js telemetry on the builder's machine | `next telemetry status` | **Enabled** (machine-wide setting, outside the repository). | Next.js telemetry sends anonymous usage data from `next dev` and `next build`. It is separate from `agentFeedback`, and it also runs on CI runners and hosts unless `NEXT_TELEMETRY_DISABLED=1` is set there. |
| S7 | `experimental.agentFeedback` behaviour | `next.config.ts`; `node_modules/next/dist/docs/01-app/03-api-reference/05-config/01-next-config-js/agentFeedback.md`; `AGENTS.md` | The flag acts **only under `next dev`**: it writes a managed block into `AGENTS.md` telling the AI coding agent to collect "anonymized facts" and run `npx --no-install next internal agent-feedback-instructions`, which opens a report form in the browser. The docs say "Agent feedback does not run in CI", and nothing is sent unless the person presses **Send feedback**. `AGENTS.md` has no feedback block yet. | This corrects TD-03 as worded in the lead draft: the exposure is the builder's development machine, not deployed environments. Anonymising is left to the AI agent, which is not a control a medical-data product should rely on. The managed block is also an outside party writing instructions into the agent's instruction file, which matters for agent governance. |
| S8 | AI coding agent data path | `.claude/settings.json` (modified in the working tree) | `"CLAUDE_CODE_USE_BEDROCK": "0"`, so this workflow talks to the Anthropic API directly rather than through AWS Bedrock as `CLAUDE.md` describes. `.mcp.json` declares a remote HTTP MCP server (`context7`) and four AWS MCP servers. | Anything placed in the workspace can be read by the agent and sent to the model provider. This is the main way a real bill could leave the machine during development, and it bears on data residency (CR-R9). |
| S9 | MCP server pinning | `.mcp.json` | The four AWS MCP servers launch through `uvx` with `@latest` (for example `awslabs.aws-iac-mcp-server@latest`), so each launch can fetch new, unreviewed code that runs with the builder's AWS credentials. | Unpinned tooling that runs on the builder's machine is a supply-chain risk. If hosting is not AWS, these servers may not be needed at all. |
| S10 | Security lint coverage | `eslint.config.mjs` | Only the `eslint-config-next` `core-web-vitals` and `typescript` presets. No security-focused rules (for example `eslint-plugin-security`, or `no-eval`/`no-implied-eval` enforced as errors) and no SAST tool. | Lint catches correctness and framework misuse, not security patterns. SAST must be added separately. |
| S11 | Framework security guidance | `node_modules/next/dist/docs/01-app/02-guides/` | Bundled guides exist for `data-security.md`, `content-security-policy.md`, `authentication.md`, `authentication-with-cache-components.md`, plus the `taint` API reference. | Builders should read these before writing upload, Server Action or caching code (this follows the existing `AGENTS.md` rule). |

### 2. Corrections to the lead draft

- **Telemetry candidate (Forbidden, `discovered-rules.md`).** "NEVER let real patient bills reach any environment while Next.js telemetry (`experimental.agentFeedback`) is on" mixes two different things and points at the wrong place. Suggested replacement, split in two:
  - CANDIDATE (source: S6, S7, TD-03, CR-R1, CR-R2): NEVER run `next dev` against real patient bills while `experimental.agentFeedback` or Next.js telemetry is enabled on that machine.
  - CANDIDATE (source: S6): ALWAYS set `NEXT_TELEMETRY_DISABLED=1` on CI runners and every hosted environment.
  - The matching line in `team-practices.md` `## Deployment` ("privacy settings (Next.js telemetry via `experimental.agentFeedback`)") should say "Next.js telemetry and `experimental.agentFeedback`".
- **TD-03 wording in `evidence.md` E10.** Add that the flag is development-only (S7). Its risk is real but sits on the builder's machine and in `AGENTS.md`, not in production.
- **CI gate list is missing security checks.** The `## Walking Skeleton` and `## Code Style` proposals list lint, type-check, tests and coverage, but no secret, dependency or static security scan. With one builder and no second human reviewer (inference I1), automated security gates are the only independent check before merge, so they belong in the first CI pipeline, not later.

### 3. Proposed security practices (for `team-practices.md`)

The lead can place these as **[Proposal]** lines. The founder decides the practice; the exact tools can be left to the CI Pipeline and NFR Requirements stages.

**Under `## Code Style` (static checks):**
- [Proposal] ESLint stays the linter. We add a small set of security rules as errors (no `eval` or `new Function`, no `dangerouslySetInnerHTML` without a reviewed sanitiser) through config, so the linter enforces them rather than reviewers.
- [Proposal] A static security scan (SAST) runs on every change in CI. Candidates: Semgrep with its TypeScript, React/Next.js and secrets rule packs (works on any CI platform), or GitHub CodeQL if the remote is GitHub. High and critical findings block the merge; medium findings are reported.
- [Proposal] Any finding we accept is suppressed in code or in a committed suppression file, with a written reason and an expiry date of at most 90 days. No silent suppressions.

**Under `## Way of Working` (secrets and dependencies):**
- [Proposal] Secret scanning runs in a pre-commit hook on the builder's machine and again in CI (for example Gitleaks). Before the first push to a new remote, one full-history scan runs. If a secret is ever committed, it is revoked and rotated first, then removed from history.
- [Proposal] CI installs with `npm ci` from the committed lockfile. Lockfile changes are reviewed like code. CI runs `npm audit --audit-level=high` (blocking) and `npm audit signatures` (blocking on invalid signatures).
- [Proposal] Automated dependency update proposals (Dependabot or Renovate) run weekly. `next` and `eslint-config-next` are upgraded together, as `dependencies.md` already notes. Security updates for production dependencies are applied within 7 days for critical and 30 days for high.
- [Proposal] Before adding a new dependency, the builder checks its maintenance, licence and whether it runs install scripts. A package that receives bill content (for example PDF or image parsing) gets extra scrutiny because it handles untrusted files.
- [Proposal] Developer tooling is pinned: `.mcp.json` servers pinned to specific versions instead of `@latest`; servers not needed for this project removed. CI actions (if GitHub Actions) pinned to commit SHAs, with the workflow token limited to read access by default.
- [Proposal] Secrets for CI and hosting live only in the platform's secret store, never in the repository, workflow files or build logs.

**Under `## Deployment` (runtime checks):**
- [Proposal] A dynamic scan (DAST, for example the OWASP ZAP baseline scan) runs against a preview or staging deployment. It starts as report-only and becomes blocking on high findings before launch. It never runs against production and never uses real bills.
- [Proposal] Before launch, the staging deployment is checked for security headers (Content Security Policy, HSTS, `X-Content-Type-Options`, `Referrer-Policy`), following the bundled `content-security-policy.md` guide.
- [Proposal] A software bill of materials is produced for each release with `npm sbom` (CycloneDX) and kept with the release tag.
- [Proposal] Feasibility condition F5 is a launch gate: an outside security test (penetration test) of the upload, processing and report flows, plus a written incident plan that one person can run (including the CERT-In 6-hour reporting rule, CR-R3 **[VALIDATE]**), are both done before real users upload bills.

**Under `## Testing Posture`:**
- [Proposal] Security tests are part of the normal suite: upload validation (file type, size, malformed and malicious files), confirmation that the original file is deleted on success and on failure, and confirmation that no bill content appears in logs.
- [Proposal] The accuracy suite that uses real (redacted) bills from the Labelled Test Set runs only on the builder's machine or on a CI service the founder has approved as a data processor (CR-D7). It does not run on a hosted CI runner by default.

### 4. Candidate rules to add to `discovered-rules.md`

Each is a candidate for the founder to confirm, reword or drop.

**Mandated**
- CANDIDATE (source: S4, S5, `phases/construction.md` `## Security`): ALWAYS run secret scanning before every commit and in CI; if a secret is committed, revoke and rotate it before cleaning history.
- CANDIDATE (source: S1, S10, inference I1): ALWAYS block a merge on high or critical findings from dependency or static security scans, unless a suppression with a written reason and an expiry date exists.
- CANDIDATE (source: S6): ALWAYS set `NEXT_TELEMETRY_DISABLED=1` on CI runners and every hosted environment.
- CANDIDATE (source: F5 in `feasibility-assessment.md`, CR-R3): ALWAYS complete an outside security test and a written incident plan before real users upload bills.

**Forbidden**
- CANDIDATE (source: S6, S7, replaces the current telemetry candidate): NEVER run `next dev` against real patient bills while `experimental.agentFeedback` or Next.js telemetry is enabled on that machine.
- CANDIDATE (source: S8, CR-R1, CR-R2, CR-D6): NEVER store real patient bills, including redacted Labelled Test Set originals, inside the repository working tree or any folder the AI coding agent or its MCP servers can read; keep them in a separate, encrypted location.
- CANDIDATE (source: CR-D3, CR-D4, NFR-20 via CR-D7): NEVER write bill content or patient identifiers to logs, error reports, analytics or telemetry.
- CANDIDATE (source: CR-D7, `build-vs-buy.md` pass/fail conditions): NEVER send bill content to a third-party service (bill reading, AI, hosting, CI, error tracking) that has not passed the data-processing terms check. This answers the lead's open question about which services may receive bill content.
- CANDIDATE (source: S1): NEVER apply `npm audit fix --force` or accept a semver-major change suggested by an audit tool without reviewing it.

The existing candidates on committed secrets, shared caching of bill data, real bills in tests and gate bypassing are sound and should stay (see Positions). For the "delete the original" candidate, I suggest adding "and leave no copy in temporary folders, logs or error reports".

### 5. Questions the founder must decide in the interview

Practice-level decisions (needed now):
1. **Telemetry.** Turn off `experimental.agentFeedback` in `next.config.ts` and disable Next.js telemetry on the builder's machine (`next telemetry disable`) and in CI and hosting? Recommended: yes, both, now. Telemetry is currently enabled on this machine (S6).
2. **Real bills and the AI agent.** May the AI coding agent ever see a real bill (even a redacted one)? This workflow currently sends workspace content to the Anthropic API directly, not through Bedrock (S8). Recommended: no; Labelled Test Set originals live outside the repository in an encrypted location, and only synthetic bills enter the workspace.
3. **Blocking threshold.** Block merges on high and critical findings from dependency and static scans, and report medium and below? Or stricter (block medium too)?
4. **Suppression policy.** Allow suppressions only with a written reason and a maximum 90-day expiry?
5. **Patch timelines.** Critical within 7 days and high within 30 days for production dependencies: accept, tighten or loosen?
6. **Outside security test and incident plan (F5).** Confirm both are a hard gate before real users upload bills, and say who will do the test (a paid firm, a freelance tester, or a bug-bounty style arrangement) and roughly when.
7. **Developer tooling.** Pin the MCP servers in `.mcp.json` to fixed versions and remove the AWS servers if hosting will not be AWS?

Tool choices (can be deferred to the CI Pipeline stage once the remote and CI platform are known):
8. Static scan: Semgrep, CodeQL (GitHub only) or SonarCloud.
9. Secret scanner: Gitleaks (pre-commit and CI) plus the platform's own scanning if available.
10. Dependency updates: Dependabot or Renovate, and whether patch-level updates may merge automatically when CI is green.
11. Dynamic scan: OWASP ZAP baseline on staging, and the date it switches from report-only to blocking.
12. Software bill of materials per release: yes or no.

Suggested walking-skeleton additions (if the founder agrees): Gitleaks pre-commit hook and CI step, `npm audit --audit-level=high` and `npm audit signatures` in CI, `NEXT_TELEMETRY_DISABLED=1` in CI, and `agentFeedback` turned off. Each is small and much cheaper to add before features exist.

### Sources

- Commands run in this review: `nvm use` then `node -v` (v24.19.0); `npm audit --json`; `npm audit --omit=dev`; `npm audit signatures`; `next telemetry status`; `git ls-files`; keyword search of `git log --all -p`; `which gitleaks trufflehog semgrep git-secrets osv-scanner`.
- Files: `package.json`, `package-lock.json`, `eslint.config.mjs`, `next.config.ts`, `.gitignore`, `.mcp.json`, `.claude/settings.json`, `AGENTS.md`; `node_modules/next/dist/docs/01-app/03-api-reference/05-config/01-next-config-js/agentFeedback.md`, `node_modules/next/dist/docs/01-app/02-guides/ai-agents.md`.
- Code knowledge base: `aidlc/spaces/default/codekb/hospital-bill-audit-app/code-quality-assessment.md` (TD-01 to TD-06), `dependencies.md`, `technology-stack.md`, `architecture.md`, `business-overview.md`.
- Ideation: `aidlc/spaces/default/intents/261026-bill-audit-baseline/ideation/feasibility/constraint-register.md` (CR-R1 to CR-R3, CR-R9, CR-D3 to CR-D7), `feasibility-assessment.md` (F2, F3, F5).
- Lead draft: `team-practices.md`, `discovered-rules.md`, `evidence.md` in this folder.

### Assumptions & Open Questions

- Assumption: the keyword search of git history (S4) is a heuristic; a full-history scan with a real secret scanner has not been run because none is installed.
- Assumption: patch timelines (7 and 30 days) and the 90-day suppression expiry are common industry defaults offered as starting points, not requirements from any source.
- Open: the CI platform, and therefore the exact SAST, secret-scanning and dependency-update tools, depend on where the remote repository is hosted (lead's E3, I3).
- Open: CERT-In 6-hour reporting is marked **[VALIDATE]** in the constraint register and was not re-checked here.

## Positions

- AGREE: Trunk-based development with squash-merge, and the AI-DLC approval gate plus green CI as the review before merge — workable for one builder, provided the CI gates include automated security checks, which then act as the independent reviewer.
- AGREE: Forbidden candidate on committing credentials, keys, secrets or `.env` files — matches the clean baseline (S4) and the construction security guardrail.
- AGREE: Forbidden candidate on bill data in shared caches or prerendered or prefetched output — Cache Components and Partial Prefetching are on, so this is a real leak path between users.
- AGREE: Forbidden candidate on real bills in tests, fixtures, logs or screenshots — required by CR-R1, CR-R2 and CR-D4.
- AGREE: Mandated candidate to delete the original upload, including when reading fails — add "and leave no copy in temporary folders, logs or error reports".
- AGREE: Founder approval, written rollback and smoke check for every production deployment — sound for a two-person team.
- OBJECT: Telemetry Forbidden candidate as worded — `experimental.agentFeedback` only acts under `next dev` and never in CI or deployed environments (S7), while general Next.js telemetry also runs on builds and is enabled on this machine (S6); replace it with the two rules in section 2.
- OBJECT: CI gate lists in `## Walking Skeleton` and `## Code Style` — they omit secret scanning, dependency scanning and static security scanning; with no second human reviewer these must be in the first pipeline.
- OBJECT: Accuracy suite "against the Labelled Test Set" in `## Testing Posture` with no location stated — redacted real bills must not go to a hosted CI runner or into the AI agent's workspace unless that service passes the CR-D7 processor check.
- OBJECT: No practice covering the AI coding agent and MCP servers as a data path — the workflow sends workspace content to the Anthropic API directly (S8) and runs unpinned `@latest` MCP servers (S9); the draft should include the real-bill storage rule and tooling pinning.
