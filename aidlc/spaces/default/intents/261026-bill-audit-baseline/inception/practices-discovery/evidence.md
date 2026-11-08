# Practices Discovery Evidence

> What each participant inspected or inferred, what the human decided in the interview, how the reviewers' objections were resolved, and what remains uncertain.

## Project Context

- Brownfield by label, effectively greenfield in application code: an unmodified Create Next App scaffold (two files in `app/`).
- Team: one builder working alone with this AI-assisted workflow, plus the founder who decides scope and priority and approves releases. No other engineers, no domain experts, no legal review before launch.
- The product handles uploaded Indian hospital bills: personal and health data under the DPDP Act 2023. Original files are deleted after reading.
- Active scope `enterprise`; Depth Comprehensive; Test Strategy Comprehensive; Change Control strict.

## Lead Inspection (aidlc-pipeline-deploy-agent)

| # | What was inspected | Path or command | Finding | Bearing on practice |
|---|---|---|---|---|
| E1 | Git history | `git log --all` | Two commits on `main`: `19fa65e` "Initial commit from Create Next App" (2026-10-26) and `818076b` "chore: Initial setup with AIDLC harness with Claude." (2026-10-27). Two author names (`Venkata Sriram Jonnavitula`, `Sriram JV`). | No established branching or commit-message habit to infer from. |
| E2 | Branches and tags | `git branch -a`, `git tag` | Only `main`; no tags. | Nothing contradicts the trunk-based default. |
| E3 | Remote | `git remote -v` | No remote configured. | No pull requests or hosted checks are possible yet. |
| E4 | Working tree | `git status --short` | `.claude/settings.json` and `memory/project.md` modified; `aidlc/spaces/default/{codekb,intents,knowledge}/` untracked. | About a week of workflow records is uncommitted. |
| E5 | CI configuration | `ls .github`, repository root | None. | CI is built from scratch; the platform depends on E3. |
| E6 | Deployment configuration | repository root | No `vercel.json`, `Dockerfile` or infrastructure code. | Deployment practice stays host-neutral. |
| E7 | Scripts | `package.json` | `dev`, `build`, `start`, `lint` only; no `test`, `typecheck` or `format`; no `engines` field. | Scripts must be added before checks can run. |
| E8 | Linter | `eslint.config.mjs` | Next `core-web-vitals` and `typescript` presets; `globalIgnores` lacks `.claude/**` and `aidlc/**`. | `npm run lint` fails on AI-DLC tooling (TD-01). |
| E9 | TypeScript | `tsconfig.json` | `strict: true`; includes `.next/types/**`. | Type-check needs `next typegen` first on a fresh clone. |
| E10 | Next config | `next.config.ts` | `experimental.agentFeedback: true`, `cacheComponents: true`, `partialPrefetching: true`. | The draft treated `agentFeedback` as a deployed-environment telemetry risk; reviewers corrected this (see R-D2, R-S2): it acts only under `next dev`. |
| E11 | Node version | `.nvmrc`, `AGENTS.md` | `.nvmrc` = v24.19.0; `nvm use` required before `npm`. Machine default v18.12.1 (from Reverse Engineering). | Basis for the `engines` guard and the `nvm use` practice. |
| E12 | Formatter | repository root | No Prettier config, no `.editorconfig`. | Formatter was an open choice (settled in Q13). |
| E13 | Agent instructions | `AGENTS.md`, `CLAUDE.md` | `nvm use` rule; read bundled Next.js docs before writing code. | Kept as practices (Q14). |
| E14 | Scope file | `.claude/scopes/aidlc-enterprise.md` | `skeleton: on`, `change_control: strict`. | Thin slice first by default (confirmed in Q4). |
| E15 | Intent plan | `aidlc-state.md` | Construction stages (`build-and-test`, `ci-pipeline`) run; all Operation stages (4.1 to 4.7) are skipped. | Performance validation has no stage of its own (resolved in Q9). |
| E16 | Framework defaults | `memory/org.md` | Trunk-based, squash-merge; skeleton per scope; test-after default with 80% floor; deploy on merge to staging; manual production gate; defer to linter config. | Used as suggested defaults only. |
| E17 | Team rules | `memory/team.md` | All sections empty. | First run; no affirmed baseline. |
| E18 | Hosting and residency | `memory/project.md` `## Corrections`; `feasibility-assessment.md` F2, F3 | Hosting not decided ("Mostly Vercel. But I would like to decide at a later point."); residency is a founder decision due before the build. | No deployment before the decision (Q4, Q10). |
| E19 | Product constraints | `constraint-register.md` CR-R1, CR-R2 | DPDP Act from day one; medical records are sensitive personal data. | Basis for the real-bill and data-safety practices. |

Lead inferences: I1 the only review before merge is the approval step plus automatic checks; I2 squash-merge fits a solo builder; I3 the remote and CI platform are a prerequisite of the first slice; I4 the lint, typecheck and `engines` fixes belong in the first slice; I5 a test-first cadence might suit the bill checks (not adopted, see Q5).

## Quality Review (aidlc-quality-agent)

Inspected the lead draft, the code knowledge base, workspace config, the bundled testing guides (`node_modules/next/dist/docs/01-app/02-guides/testing/`) and the PRD (§8.4, §12.14, KB-04, NFR-01 to NFR-20, G-16). Confirmed the baseline (0 tests, no runner, no coverage, no CI, failing lint). Key inferences:

- R-Q1: the code test suite and the accuracy evaluation are two different quality systems with different inputs, targets and cadences.
- R-Q2: the finding-precision targets (≥ 90% "Likely error", ≥ 75% "Worth asking") were missing from the draft.
- R-Q3: recommended `custom` methodology (test-first for checks, money and report rules) and a 90% branch floor for check and money modules.
- R-Q4: real Labelled Test Set bills must stay out of git (withdrawal under LT-11 is impossible once in history); end-to-end tests use synthetic bills only; no live AI-vendor calls in the per-merge suite.
- R-Q5: performance NFRs (NFR-01, NFR-02, NFR-04, NFR-15) have no validation home because Performance Validation is skipped.
- R-Q6: the runner must be chosen now because Bolt 1 installs it.
- R-Q7: PRD gap G-16 (minimum positive and negative examples per check) is still open.

## Developer Review (aidlc-developer-agent)

Inspected the lead draft, the code knowledge base, repository config, `app/` files and bundled Next.js 16.4.3 docs. Confirmed all Code Style facts. Key findings:

- R-D1: `next lint` was removed in Next.js 16; the lint fix belongs in `eslint.config.mjs`. The bundled CLI doc prescribes `next typegen && tsc --noEmit` for CI type-checking.
- R-D2: `experimental.agentFeedback` acts only under `next dev` (writes a managed block into `AGENTS.md`); it does not run in CI or deployed builds.
- R-D3: the shared-cache wording should name `'use cache'` and `'use cache: remote'` explicitly and treat `'use cache: private'` as needing a recorded NFR Design decision.
- R-D4: Prettier 3 defaults match the scaffold style; the bundled ESLint doc recommends `eslint-config-prettier/flat`.
- R-D5: proposed conventions for file layout, layer boundaries, error handling and money (see Deferred Design Input below).

## Security Review (aidlc-devsecops-agent)

Ran `nvm use`, `npm audit --json`, `npm audit --omit=dev`, `npm audit signatures`, `next telemetry status`, a keyword search of git history and a check for installed security tools. Key findings:

- R-S1: 16 moderate advisories, all from one `minimatch` advisory in the dev-only lint chain; 0 in production dependencies. `npm audit fix --force` would wrongly downgrade `eslint`.
- R-S2: Next.js telemetry is enabled on the builder's machine and would also run on build servers and hosts unless `NEXT_TELEMETRY_DISABLED=1` is set; `agentFeedback` is a separate, dev-only setting (agrees with R-D2).
- R-S3: no secrets in tracked files or history (heuristic search); no security tools installed.
- R-S4: `.claude/settings.json` has `CLAUDE_CODE_USE_BEDROCK` set to `0`, so workspace content goes to the AI provider directly; anything in the workspace can leave the machine.
- R-S5: the four AWS MCP servers in `.mcp.json` launch with `@latest`, an unpinned supply-chain risk.
- R-S6: with no second reviewer, automated secret, dependency and static security scans must be in the first pipeline.
- R-S7: proposed a DAST baseline on staging, security headers check, SBOM per release, dependency-update automation, patch timelines (7 days critical, 30 days high), and F5 (outside security test and incident plan) as a launch gate.

## Interview Decisions

| Q | Topic | Answer | Effect on practice |
|---|---|---|---|
| Q1 | Code host and CI service | C | Not decided; decided before the first build slice starts. |
| Q2 | Merging into `main` | A | Pull requests once the remote exists; checks must pass; squash-merge. |
| Q3 | Commit cadence | B | Commit workflow records and code at the end of each phase. |
| Q4 | Thin first slice | B | Yes, as drafted, but no deployment until hosting is decided; founder approves before the rest. |
| Q5 | Test timing | B | `test-after` everywhere. |
| Q6 | Coverage floor | B | 80% of lines overall; no extra branch floor. |
| Q7 | Test tools | B | Jest for unit and component tests; Playwright for end-to-end. |
| Q8 | Accuracy evaluation | A | Before a check goes live and on every prompt, model or rule change; real bills outside git and out of the AI assistant's reach; only on services that passed the data-processor check. |
| Q9 | Speed and capacity | A | Basic timing and load test in Build and Test, on a test environment with made-up bills. |
| Q10 | Environments and approval | B | Staging and production only; merges deploy to staging; founder approves production after checks and a quick post-deploy check; no real bills until residency and hosting are decided. |
| Q11 | Security checks | A | From the first slice: secret scan (pre-commit and CI), block on high or critical `npm audit`, verify package signatures, SAST blocking on high or critical; medium reported only; accepted findings need a reason and expire within 90 days. Tools chosen in CI Pipeline. |
| Q12 | Data safety | A | Telemetry off on the builder's machine, build servers and hosts; `agentFeedback` off; real bills never where the AI assistant can read; helper tools pinned, unneeded ones removed. |
| Q13 | Code structure | B | Prettier (with `eslint-config-prettier`) and the linter only; structure left to the design stages. |
| Q14 | Hard rules | C | No hard rules; the eight candidates stay as practices in `team-practices.md`. |

Consolidated summary: confirmed by the human ("Looks correct").

## Reviewer Objections and Their Resolution

| Objection | Raised by | Resolution |
|---|---|---|
| Finding-precision targets missing; G-16 not mentioned | Quality | Precision targets added to Testing Posture (Q8). G-16 remains open (see Unresolved Uncertainty). |
| Accuracy suite placement and cadence unstated | Quality, Security | Resolved by Q8: not per merge; before go-live and on every prompt, model or rule change; real bills outside git and the AI assistant's reach; approved processors only. |
| Runner choice deferred too late | Quality | Resolved by Q7: Jest and Playwright affirmed now. The quality reviewer's Vitest recommendation was not chosen. |
| Performance NFRs had no validation home | Quality | Resolved by Q9: basic timing and load test in Build and Test. |
| `custom` methodology and 90% branch floor | Quality | Considered and declined: Q5 chose `test-after` everywhere, Q6 chose 80% lines only. |
| Telemetry candidate pointed at the wrong place | Developer, Security | Corrected framing adopted: `agentFeedback` is dev-only and is turned off; Next.js telemetry is turned off on the machine, build servers and hosts (Q12). |
| Shared-cache wording too coarse | Developer | Kept as a general practice ("never in a shared cache, prebuilt page or prefetched output"). The distinction between `'use cache'`, `'use cache: remote'` and `'use cache: private'` is deferred input for NFR Design. |
| Code Style omits structure, error handling, money | Developer | Q13 chose to leave these to the design stages; proposals recorded below as deferred input. |
| CI gate lists omit security checks | Security | Resolved by Q11: blocking security checks from the first slice. |
| No practice for the AI assistant and MCP servers as a data path | Security | Resolved by Q12: real bills never where the assistant can read; helper tools pinned and unneeded ones removed. |

## Deferred Design Input (not team practice)

Per Q13, these developer and security proposals are recorded for Functional Design and NFR Design to consider, and are not team practices:

- File layout: routing files only in `app/`; `lib/audit/` for bill-check logic, `lib/server/` for server-only code, `components/` for shared UI, `e2e/` for Playwright; unit tests next to the code; root vs `src/` and file-name case still open.
- Layer boundaries: bill-check logic framework-free and I/O-free; `import 'server-only'` on modules touching uploads, bill content, secrets or environment variables; a Data Access Layer returning minimal DTOs; `"use client"` only on interactive leaf components; Server Actions and Route Handlers treated as untrusted public endpoints with schema validation (library open; `zod` is only transitive today); `proxy.ts`, not `middleware.ts`.
- Error handling: expected failures returned as typed results; `error.tsx` per segment plus `global-error.tsx`; `catchError` from `next/error`; `retry()` rather than `reset()`; plain-language patient messages; log `error.digest`; deletion of the original in `finally` or `after()`, with failed deletions logged.
- Caching: `'use cache'` and `'use cache: remote'` never hold bill data; `'use cache: private'` only with a recorded NFR Design decision.
- Money: integer paise or a decimal library, never floating-point rupees; display with `Intl.NumberFormat('en-IN', { style: 'currency', currency: 'INR' })`.
- Logging: no bill contents, patient names, patient-linked hospital identifiers or uploaded file names in logs, error reports or analytics.
- Stricter TypeScript options (`noUncheckedIndexedAccess`, `noImplicitOverride`), `.editorconfig`, `prettier-plugin-tailwindcss`, security lint rules, and a commit-message format (`<type>(<bolt-slug>): <summary>`).
- Security tooling for the CI Pipeline stage: SAST (Semgrep, CodeQL or SonarCloud), secret scanner (for example Gitleaks, with one full-history scan before the first push), Dependabot or Renovate, DAST baseline on staging, security headers check, SBOM per release, patch timelines.
- Test design for Build and Test: AI vendor behind an interface with a stub or recorded responses in the per-merge suite; synthetic-bill builders; property-based tests for money invariants; content-safety wording checks (C10); an explicit, reviewed coverage exclusion list.

## Tool Note (Q7)

- The bundled Next.js 16 Jest guide (`node_modules/next/dist/docs/01-app/02-guides/testing/jest.md`) configures Jest through `next/jest`.
- The same guide states that Jest does not support `async` Server Components and recommends end-to-end tests for them, so those components need Playwright coverage. End-to-end coverage does not count towards the 80% line figure, so logic is best kept where Jest can reach it.

## Unresolved Uncertainty

- Code host and CI platform (Q1): decided before the first build slice; exact security tools follow from it.
- Hosting and data residency (feasibility F2, F3): founder decisions due before the build. Until then nothing is deployed and no real bills reach any environment.
- Q9 requires a test environment for the timing and load test, which depends on the hosting decision.
- CERT-In 6-hour incident reporting (CR-R3) is marked **[VALIDATE]** in the constraint register and was not re-checked.
- Feasibility F5 (outside security test and a written incident plan before launch) was raised by the security reviewer but not asked in this interview; it remains a launch condition from feasibility.
- PRD gap G-16 (minimum examples per check before promotion) is open for Requirements Analysis or NFR Requirements.
- `.claude/settings.json` has `CLAUDE_CODE_USE_BEDROCK` set to `0`, which differs from `CLAUDE.md`; the founder may want to review it in light of data residency.
- The heuristic secret search is not a full scan; a real scanner has not yet been run over history.
- The two git author names; set a consistent identity before Construction if attribution matters.

## Sources

- Lead commands: `git log --all`, `git branch -a`, `git remote -v`, `git tag`, `git status --short`, `git diff --stat`, `git rev-parse HEAD`, repository root listing, `ls .github`.
- Files: `package.json`, `eslint.config.mjs`, `tsconfig.json`, `next.config.ts`, `.nvmrc`, `.gitignore`, `.mcp.json`, `AGENTS.md`, `CLAUDE.md`, `.claude/scopes/aidlc-enterprise.md`, `aidlc/spaces/default/intents/261026-bill-audit-baseline/aidlc-state.md`, `node_modules/next/dist/docs/01-app/02-guides/testing/jest.md`.
- Code knowledge base: `aidlc/spaces/default/codekb/hospital-bill-audit-app/` (`technology-stack.md`, `code-quality-assessment.md`, `architecture.md`, `business-overview.md`, `code-structure.md`, `dependencies.md`).
- Reviews: `contributions/aidlc-quality-agent.md`, `contributions/aidlc-developer-agent.md`, `contributions/aidlc-devsecops-agent.md` in this folder.
- Interview: `practices-discovery-questions.md` in this folder.
- Memory: `aidlc/spaces/default/memory/org.md`, `team.md`, `project.md`.
- Ideation: `aidlc/spaces/default/intents/261026-bill-audit-baseline/ideation/feasibility/feasibility-assessment.md`, `constraint-register.md`.

## Assumptions & Open Questions

- Assumption: the machine default Node version, lint error counts and `npm audit` results are as reported by Reverse Engineering and the security reviewer; the lead did not re-run them.
- Assumption: the two git author names belong to the same person (the builder).
- Open: the items under Unresolved Uncertainty above.
