# Code Quality Assessment — hospital-bill-audit-app

## Overall Verdict

The application code is two clean template files. Quality of what exists is good; **readiness as a foundation for an enterprise-scope product is low**: no tests, no CI, a failing lint script, an implicit type-generation step, and two privacy-relevant framework flags without a decision. The application layer is effectively **greenfield despite the Brownfield label**: every product capability, and every quality gate, must be built from scratch.

## Test Coverage

| Item | Finding |
|---|---|
| Test directories | None (`__tests__/`, `*.test.*`, `*.spec.*`, `e2e/`, `tests/` all absent) |
| Test frameworks | None installed or configured (no Jest, Vitest, Playwright, Cypress) |
| `test` script | Absent |
| Coverage config | Absent (`.gitignore` reserves `/coverage`, nothing produces it) |
| **Baseline** | **0 tests, 0% coverage** |

**Gap against policy:** the enterprise scope's org default requires an **80% line-coverage floor and CI execution before merge**, and the active Test Strategy is Comprehensive. Both a test toolchain and a pipeline must be added before the first product Bolt can merge. The bundled Next docs provide setup guides for all four test tools; choosing one is a later-stage decision.

## Linting

- ESLint 9 flat config (`eslint.config.mjs`) with `eslint-config-next/core-web-vitals` and `eslint-config-next/typescript`.
- App paths (`app/`, `next.config.ts`, `eslint.config.mjs`) lint clean (exit 0).
- **`npm run lint` fails.** The script is a bare `eslint`, which lints the whole working directory, and `globalIgnores` does not exclude `.claude/`. Result: 9 errors and 25 warnings in 15 of 91 `.claude/**` files (mostly `@typescript-eslint/no-unused-vars`, `no-require-imports`). Wiring this into CI unchanged would block every PR. Fix: add `.claude/**` and `aidlc/**` to `globalIgnores`, or scope the script to app paths.

## Type Safety

- `strict: true`; `tsc --noEmit` exits 0 under Node 24 **once generated types exist**.
- **Type-checking depends on generated `.next/types/`.** `app/layout.tsx:20` uses the global `LayoutProps<"/">`, and `next-env.d.ts` imports `./.next/types/routes.d.ts`. A fresh clone has neither, so CI must run `next typegen` (or a build) before a standalone `tsc`.
- TypeScript skips dot-directories, so `.claude/` is not type-checked (correct for app scope).

## Formatting

No Prettier and no `.editorconfig`. Code follows Create Next App style. The org rule defers to project formatter config, and there is none, so a formatter decision is open.

## CI/CD

None: no `.github/`, `.gitlab-ci.yml`, `vercel.json`, or `Dockerfile`. The org Deployment rule (deploy on merge to staging, gated production) has nothing to attach to yet. Hosting is a working assumption (Vercel), not a decision.

## Documentation

- `README.md`: unmodified Create Next App text.
- `AGENTS.md`: Next.js agent-rules block (rewritten by `next dev`) plus the `nvm use` rule. `CLAUDE.md` imports it.
- No doc comments in app code (normal for template code).

## Quick Assessment (application code)

| Dimension | Rating | Note |
|---|---|---|
| Naming | Good | Idiomatic |
| Function size | Good | 2 components, each under 70 lines of JSX |
| Error handling | N/A | No I/O |
| Tests | Poor | None |
| Duplication | Good | None |
| Dead code | Fair | 3 unused SVGs, template copy and links |
| Secrets | Good | None hard-coded; `.env*` ignored; `.mcp.json` uses an env reference |

## Technical Debt and Risk Register

| # | Item | Location | Severity | Recommended handling |
|---|---|---|---|---|
| TD-01 | `npm run lint` fails on AI-DLC tooling | `eslint.config.mjs`, `package.json` `lint` | High (blocks CI) | Ignore `.claude/**`, `aidlc/**` or scope the script, before CI |
| TD-02 | Zero tests, no runner, no CI vs the 80% coverage floor | repo-wide | High | Add test toolchain and CI in the first Construction Bolt |
| TD-03 | `experimental.agentFeedback: true` requires Next.js Telemetry | `next.config.ts:5-7` | High (privacy, medical-bill product, DPDP Act) | Explicit founder decision; default to off until decided |
| TD-04 | Cache Components and Partial Prefetching on; risk of user bill data in a shared `use cache` scope or prefetched | `next.config.ts:8-9` | High (privacy) if mishandled | NFR Design rule: bill data request- or session-scoped only, never in shared cache, never prerendered |
| TD-05 | Type-check needs generated `.next/types/` and `next-env.d.ts` | `app/layout.tsx:20`, `tsconfig.json` | Medium | CI step `next typegen` before `tsc`; add a `typecheck` script |
| TD-06 | Machine default Node v18.12.1 < Next 16 minimum 20.9.0; no `engines` field | `package.json`, `.nvmrc` | Medium | Add `engines.node` (for example `>=24.19.0 <25` or `>=20.9.0`); CI uses `.nvmrc` |
| TD-07 | Tailwind only via the Turbopack loader, no PostCSS config | `next.config.ts:10-17` | Low | Record; revisit only if a webpack build is ever needed |
| TD-08 | `next-env.d.ts` gitignored (see correction below) | `.gitignore:41` | Low | Keep ignored; rely on TD-05 generation step |
| TD-09 | Template placeholders: metadata, home page, `utm_*` links, README, unused SVGs, `body` font override | `app/layout.tsx:15-18,23`, `app/page.tsx`, `README.md`, `public/`, `app/globals.css` | Low (must fix before release) | Replace during the first UI Bolt |
| TD-10 | `@types/node ^20` with Node 24 pinned | `package.json` | Low | Align when adding `engines` |
| TD-11 | No foundations for the product: upload, storage, validation, error boundaries, security headers (`proxy.ts`), observability | repo-wide | High (scope, not defect) | Greenfield design in Inception and Construction |

### Correction to the developer scan (TD-08)

The developer scan states that `next-env.d.ts` is "both tracked and gitignored". The architect re-checked on 2026-11-04: `git ls-files --error-unmatch next-env.d.ts` reports it is **not known to git**, and `git check-ignore -v` matches `.gitignore:41`. The file exists only locally as a generated file. So there is no tracked-versus-ignored inconsistency; the real effect is that **a fresh clone has no `next-env.d.ts`**, which strengthens TD-05.

## Sources

- Developer scan: `aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/reverse-engineering/developer-scan.md` (Test Coverage, Code Quality Indicators, Technical Debt Signals, Handoff Summary).
- Architect verification: `git ls-files`, `git ls-files --error-unmatch next-env.d.ts`, `git check-ignore -v next-env.d.ts` (2026-11-04); `next.config.ts`, `eslint.config.mjs`, `package.json`, `app/layout.tsx`.
- Policy: `aidlc/spaces/default/memory/org.md` `## Testing Posture`, `## Deployment`, `## Code Style`; hosting status from `aidlc/spaces/default/memory/project.md` `## Corrections`.

## Assumptions & Open Questions

- Open: which test runner(s), formatter and CI platform to adopt; decided in later stages (NFR Requirements, Infrastructure Design, CI Pipeline), not here.
- Open: telemetry and `agentFeedback` stay on or off (TD-03); a founder privacy decision.
- Assumption: lint error counts (9 errors, 25 warnings) are as reported by the developer scan and were not re-run by the architect.
