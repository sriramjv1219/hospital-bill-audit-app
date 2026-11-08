# Technology Stack — hospital-bill-audit-app

## Languages

| Language | Use | Notes |
|---|---|---|
| TypeScript 5.9.3 | Application code (`.tsx`), config (`next.config.ts`) | `strict: true`, `noEmit: true`, target ES2017, `moduleResolution: bundler`, `jsx: react-jsx`, `allowJs` |
| CSS | `app/globals.css` | Tailwind v4 CSS-first config (`@import "tailwindcss"`, `@theme inline`) |
| JavaScript (ESM) | `eslint.config.mjs` | Config only |

## Runtime

| Item | Value | Notes |
|---|---|---|
| Node.js (pinned) | v24.19.0 | `.nvmrc`; AGENTS.md requires `nvm use` before `npm` |
| Node.js (machine default) | v18.12.1 | Below Next 16's `engines.node >=20.9.0`; no `engines` field in `package.json` to block it |
| Package manager | npm, lockfile v3 | 437 package entries |

## Frameworks and Libraries

Declared range → installed version (from `node_modules/*/package.json`).

| Package | Declared | Installed | Role |
|---|---|---|---|
| next | 16.4.3 (exact) | 16.4.3 | App Router framework, Turbopack bundler, Cache Components, Partial Prefetching, route-aware global types |
| react | 19.3.0 (exact) | 19.3.0 | UI library |
| react-dom | 19.3.0 (exact) | 19.3.0 | DOM renderer |
| tailwindcss | ^4 | 4.3.3 | Utility CSS (dev dependency) |
| @tailwindcss/turbopack | ^4 | 4.3.3 | Turbopack loader for Tailwind v4 (used instead of `@tailwindcss/postcss`) |
| typescript | ^5 | 5.9.3 | Type checking |
| eslint | ^9 | 9.39.5 | Linting, flat config |
| eslint-config-next | 16.4.3 (exact) | 16.4.3 | `core-web-vitals` and `typescript` presets |
| @types/node | ^20 | 20.19.43 | Node 20 typings (Node 24 pinned) |
| @types/react | ^19 | 19.3.0 | React typings |
| @types/react-dom | ^19 | 19.3.0 | React DOM typings |
| next/font/google | (part of next) | — | Geist, Geist Mono, self-hosted at build |

## Build and Tooling

| Concern | Tool / config | Status |
|---|---|---|
| Dev / build / serve | `next dev`, `next build`, `next start` | Never production-built in this checkout |
| Bundler | Turbopack (Next 16 default) | CSS rule in `next.config.ts` |
| Type check | `tsc` via `tsconfig.json` | Passes (exit 0 under Node 24) after types are generated; no script |
| Lint | `eslint` (`npm run lint`) | App code clean; whole-repo run fails on `.claude/` |
| Format | none | No Prettier, no `.editorconfig` |
| Test | none | No runner, no `test` script |
| CI/CD | none | No `.github/`, no pipeline files |
| Deployment config | none | No `vercel.json`, no `Dockerfile` |

## Framework Version Notes (Next 16.4.3)

The repo's `AGENTS.md` warns that this Next.js differs from older versions; the bundled docs in `node_modules/next/dist/docs/` are the reference. Points that matter for design:

- `middleware` is deprecated and renamed `proxy` (`proxy.ts`).
- `cacheComponents` and `partialPrefetching` are opt-in here and become unconditional in the next major.
- Route types (`PageProps`, `LayoutProps`) are global and generated; `next typegen` produces them without a build.
- The bundled docs carry setup guides for Jest, Vitest, Playwright and Cypress.

## Hosting

**Not decided.** Vercel is the working assumption only (user: "Mostly Vercel. But I would like to decide at a later point."). The README's Vercel deploy link is template text, not a decision.

## Not Present

Test runner, formatter, validation library (for example zod), state or form library, data/ORM layer, auth library, i18n, analytics, upload, OCR, PDF or AI integrations.

## Sources

- Developer scan: `aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/reverse-engineering/developer-scan.md` (Build System, Frameworks & Libraries, Code Quality Indicators).
- Code: `package.json`, `tsconfig.json`, `next.config.ts`, `eslint.config.mjs`, `.nvmrc`, `AGENTS.md`.
- Hosting: `aidlc/spaces/default/memory/project.md` `## Corrections`.

## Assumptions & Open Questions

None.
