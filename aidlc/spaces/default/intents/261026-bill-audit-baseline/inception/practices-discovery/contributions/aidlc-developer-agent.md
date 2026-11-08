**Collaborator:** aidlc-developer-agent

## Contribution

Scope of this review: naming, layer boundaries, error handling, file organization and code style, plus the way-of-working points that touch code. I read the lead draft (`team-practices.md`, `discovered-rules.md`, `evidence.md`), the code knowledge base, the repository configuration, the two `app/` files and the bundled Next.js 16.4.3 docs under `node_modules/next/dist/docs/`. I ran no `npm` command and changed no file other than this one.

### 1. The facts in the lead draft check out

I re-checked every Code Style fact against the repository and found no errors: ESLint 9 flat config with the `core-web-vitals` and `typescript` presets, `globalIgnores` limited to `.next/**`, `out/**`, `build/**`, `next-env.d.ts`; bare `"lint": "eslint"`; TypeScript `strict: true` with `@/*` mapped to `./*`; no formatter and no `.editorconfig`; `.nvmrc` set to `v24.19.0`; no `engines` field. Two facts can be made more precise:

- [Fact] Next.js 16 removed `next lint` (bundled doc `03-api-reference/05-config/03-eslint.md`, v16.0.0). The ESLint CLI is the only lint entry point, so the lint fix has to go in `eslint.config.mjs` or the script. It cannot go in `next.config.ts`.
- [Fact] The bundled CLI doc (`06-cli/next.md`, `next typegen`) gives `next typegen && tsc --noEmit` as the CI type-check command. This confirms the lead's proposed `typecheck` script, and the script should be exactly that.

### 2. Correction: what `experimental.agentFeedback` actually does

The lead's Forbidden candidate ("NEVER let real patient bills reach any environment while Next.js telemetry (`experimental.agentFeedback`) is on") rests on a misreading of the flag. The bundled doc `05-config/01-next-config-js/agentFeedback.md` (added in v16.4.0) says:

- It only controls whether `next dev` writes a managed "Report Next.js feedback" block into `AGENTS.md` when it detects an AI coding agent. **It does not run in CI**, and it has no effect on a built or deployed app. A staging or production deployment receives bills whether this flag is on or off.
- It needs Next.js Telemetry to be on. Feedback reports are opened in a browser form for the developer to review, and nothing is sent until the developer selects **Send feedback**.
- Today `AGENTS.md` has no `nextjs-agent-feedback` block, so `next dev` has not yet run with an agent attached.

The real exposure is on the **development machine**: during a dev session, an agent with this instruction might keep details of a bill it saw in a feedback draft. The lead's other candidate (NEVER use real patient bills in tests, fixtures, logs or screenshots) already covers that, as long as it also covers local development. Suggested replacement wording for the lead to offer the founder:

- CANDIDATE: NEVER open or process a real patient bill on a development machine or in a dev session. Local development uses synthetic bills only, and consented, de-identified Labelled Test Set bills only for the accuracy suite.
- Separate question for the founder (a choice, not a rule): keep `experimental.agentFeedback: true` or turn it off. Also decide whether CI and deployed builds set `NEXT_TELEMETRY_DISABLED=1`. Telemetry sends anonymous build and usage data, not request contents, but for a health-data product turning it off is the cautious default.

TD-03 in `code-quality-assessment.md` uses the same "privacy flag on any environment" framing. The lead may want to note that it applies to development, not deployment.

### 3. Refine the shared-cache Forbidden candidate

`cacheComponents: true` makes three cache directives available, and they behave differently (`01-directives/use-cache-private.md`, `use-cache-remote.md`):

- `'use cache'` and `'use cache: remote'` keep results across requests, and across users. Bill data must never go in either.
- `'use cache: private'` is not kept in a server cache across production requests. However, in `next dev` it is kept in memory, keyed by cookies and headers, and the client router may keep the output in browser memory for the `cacheLife` stale time.

Suggested wording: NEVER put a user's bill data, extracted line items or findings inside a `'use cache'` or `'use cache: remote'` scope, in a prerendered static shell, or in prefetched output. Use `'use cache: private'` for bill data only with a design decision recorded in NFR Design. This keeps the lead's intent and avoids banning a directive that may legitimately be needed later.

### 4. Gap: the Code Style section does not yet cover layer boundaries, file organization or error handling

The app has no structure yet, so the first Bolt will set these conventions whether or not anyone decides them. The proposals below are drawn from the bundled Next.js 16 docs. Each one is marked [Proposal] and needs the founder's confirmation.

**File organization**

- [Proposal] Keep `app/` at the repository root, where the scaffold and the `@/*` → `./*` alias already put it. Use `app/` only for routing files (`page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`, `route.ts`) and small UI pieces used by a single route. The docs (`01-getting-started/02-project-structure.md`) leave this choice to the team and only ask that it be consistent. The alternative is moving everything under `src/`, which needs the alias changed to `./src/*`. That is cheap now and expensive later.
- [Proposal] Put non-route code in top-level folders by responsibility:
  - `lib/audit/`: the bill-check domain logic (parsing results, check rules, findings).
  - `lib/server/`: server-only code (file intake, deletion of the original file, reading bills, data access).
  - `components/`: shared UI.
  - `e2e/`: Playwright specs.
- [Proposal] Put unit tests next to the code they test as `*.test.ts` or `*.test.tsx`, and keep end-to-end tests in `e2e/`. Coverage is then measured on `lib/` and `components/`, which is where the 80% floor matters.

**Layer boundaries**

- [Proposal] Bill-check logic is plain, framework-free TypeScript. It does no I/O, does not import `next/*` or React, and does not read `process.env`. Data goes in and findings come out. This lets the accuracy suite and unit tests run against the checks without a Next.js runtime. The checks are the part where correctness matters most, and this is what makes a test-first methodology for them practical (see the lead's I5).
- [Proposal] Every module that touches uploaded files, bill contents, secrets or environment variables starts with `import 'server-only'`. The data-security guide (`02-guides/data-security.md`) recommends this together with a Data Access Layer that alone reads `process.env` and returns minimal data transfer objects (DTOs). Client components receive DTOs, never raw bill objects or file buffers.
- [Proposal] Mark `"use client"` only on the leaf components that need interactivity. Pages and layouts stay Server Components, as in the scaffold.
- [Proposal] Treat every Server Action and Route Handler as an untrusted public POST endpoint. The bundled `02-guides/server-actions.md` says so directly. Each one validates its input (file type, size, page count, form fields) with a schema before calling anything else. This puts the construction-phase rule "validate and sanitize all inputs at system boundaries" into practice. Which schema library to use is a question below. `zod` is present in `node_modules` only as a transitive dependency, not a direct one.
- [Proposal] Request-level concerns such as security headers and redirects go in `proxy.ts`. In Next.js 16, `middleware.ts` is deprecated and renamed to `proxy.ts` (`03-file-conventions/middleware.md`), so no `middleware.ts` should be created.

**Error handling** (`01-getting-started/10-error-handling.md`, `03-file-conventions/error.md`, `04-functions/catchError.md`)

- [Proposal] Return expected failures as values, for example an unreadable photo, an unsupported file, a bill with no line items, or a check that cannot decide. Server Actions return a typed result object that `useActionState` displays. The bundled docs advise against throwing for expected errors.
- [Proposal] Unexpected failures go to an `error.tsx` boundary per route segment, plus `global-error.tsx`. Use `catchError` from `next/error` for boundaries at component level. In this version, error components receive `retry()`, which has been stable since v16.3.0, and the docs say to use it in most cases instead of `reset()`. Training-era examples that use `reset` are out of date.
- [Proposal] Patient-facing messages are plain-language and written for the product. They never show `error.message`, stack traces or raw extraction output. The server logs record `error.digest` so a report can be matched to the log entry.
- [Proposal] Logs, error reports and analytics never contain bill contents, patient names, hospital identifiers tied to a patient, or uploaded file names. Logs record identifiers and outcome codes only. This could also be a Forbidden candidate. It goes beyond the lead's test-data rule, because it covers production logs.
- [Proposal] Deleting the original file goes in a `finally` path, or in `after()` from `next/server`, so it also runs when reading fails. This matches the lead's Mandated candidate on deletion. A failed deletion must be logged and surfaced, never swallowed.

**Domain-specific coding convention worth deciding now**

- [Proposal] Represent money as integer paise, or with a decimal library, and never as JavaScript floating-point rupees. The product compares billed amounts with reference rates and adds up line items. Floating-point rounding would produce false "overcharge" findings, which directly affects what a patient is told. Format amounts for display with `Intl.NumberFormat('en-IN', { style: 'currency', currency: 'INR' })`. This could be a Mandated candidate if the founder agrees.

### 5. Formatter and linter details for the lead's Prettier proposal

- [Fact] The scaffold style (double quotes, semicolons, 2-space indent, trailing commas) is identical to Prettier 3 defaults, so adding Prettier with no config would cause almost no changes to existing code.
- [Fact] The bundled ESLint doc (`03-eslint.md`, "With Prettier") recommends adding `eslint-config-prettier/flat` so that ESLint and Prettier do not conflict. The Prettier proposal should include it.
- [Note] The bundled Next.js doc examples use single quotes and no semicolons. Generated code must follow the project formatter, not the style of copied doc snippets.
- [Proposal] Fix the lint failure by adding `.claude/**` and `aidlc/**` to `globalIgnores` in `eslint.config.mjs`, not by narrowing the `lint` script. Then the editor, the script and CI all lint the same files.
- [Proposal] Add `.editorconfig` (UTF-8, LF, 2 spaces, final newline) so that editors other than the AI tooling follow the same basics.

### 6. Way of working: code-adjacent points

- [Fact] Commit messages: `818076b` uses a Conventional Commits prefix (`chore:`) and `19fa65e` is the tool-generated message. Under the org squash-merge rule, the trunk commit is named by the Bolt slug.
- [Proposal] Squash commits on `main` use `<type>(<bolt-slug>): <summary>`, for example `feat(bolt-02-upload): ...`, so that `main` stays readable and the Bolt mapping is kept. This needs confirmation. The org rule only says "named by the Bolt slug".
- [Fact] The two commits use two author names (`Venkata Sriram Jonnavitula`, `Sriram JV`). If that matters for attribution, set `git config user.name` consistently before Construction starts.

### 7. Code-style questions the human must decide

1. **Formatter**: add Prettier (defaults plus `eslint-config-prettier/flat`) and a `format:check` CI step? Also add `prettier-plugin-tailwindcss` to sort Tailwind classes automatically?
2. **Source root**: keep `app/` at the root with top-level `lib/` and `components/` (current alias), or move to `src/` now?
3. **File naming for non-route modules**: kebab-case (`bill-parser.ts`, `upload-form.tsx`, as in the bundled Next.js docs) or PascalCase files for components (`UploadForm.tsx`)? Exported components stay PascalCase in both cases.
4. **Test placement**: put unit tests next to the code they test (`*.test.ts`) or in a separate `tests/` tree? End-to-end tests go in `e2e/` either way.
5. **Domain isolation**: confirm that bill-check logic stays framework-free and I/O-free, so it can be tested on its own and test-first if that methodology is chosen.
6. **Expected-error style**: return typed result objects for expected failures (as the Next.js docs recommend), or use exceptions plus boundaries everywhere?
7. **Input validation library**: add `zod` as a direct dependency for Server Action and Route Handler input schemas, use another library, or defer to NFR Design?
8. **Stricter TypeScript**: turn on `noUncheckedIndexedAccess` (catches unchecked access to bill line items by index) and `noImplicitOverride` now, while there is almost no code to fix?
9. **Money representation**: confirm integer paise (or a named decimal library) as a rule, and decide whether it becomes a Mandated rule.
10. **Logging hygiene**: confirm "no bill contents or patient identifiers in logs or error reports" as a Forbidden rule.
11. **Telemetry and `agentFeedback`**: keep `experimental.agentFeedback` on or off, and set `NEXT_TELEMETRY_DISABLED=1` in CI and deployed builds or not?
12. **Commit message format**: Conventional Commits with the Bolt slug as scope, or the bare Bolt slug?

## Positions

- AGREE: Lint fix through `globalIgnores` for `.claude/**` and `aidlc/**` in the walking-skeleton Bolt — CI cannot be green without it, and `next lint` no longer exists to work around it.
- AGREE: `typecheck` script — the bundled CLI doc prescribes `next typegen && tsc --noEmit` for CI.
- AGREE: Adding Prettier, with the addition of `eslint-config-prettier/flat` — the defaults already match the scaffold style, so adoption costs almost nothing.
- AGREE: Naming per TypeScript idiom and Next.js file conventions, including `proxy.ts` — it matches the scaffold and the Next.js 16 renaming of `middleware.ts`; the file-name case for non-route modules is still open.
- AGREE: `nvm use` and read-the-bundled-docs Mandated candidates — both are verified against `AGENTS.md`, and the bundled docs differ from training-era Next.js in ways that matter here (`retry` replacing `reset`, `proxy.ts`, `catchError`, `next lint` removed).
- AGREE: Committing the `aidlc/` tree at every stage approval and a commit cadence decided in the interview — a week of uncommitted work is the single largest risk of losing work.
- OBJECT: The Forbidden candidate linking `experimental.agentFeedback` to real bills reaching "any environment" — the bundled doc shows the flag only affects `next dev` agent instructions and does not run in CI or deployed builds; restate it as a development-machine rule plus a separate telemetry choice (section 2).
- OBJECT: The shared-cache Forbidden candidate as worded — it should name `'use cache'` and `'use cache: remote'` explicitly and treat `'use cache: private'` as needing a recorded NFR Design decision, because the three directives keep data differently (section 3).
- OBJECT: The Code Style section as drafted — it covers only linter, formatter and naming, and leaves out layer boundaries, file organization, error handling, input validation and money representation, which the first Bolt will otherwise settle by accident (sections 4 and 7).
