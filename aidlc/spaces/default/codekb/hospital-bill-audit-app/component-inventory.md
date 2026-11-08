# Component Inventory — hospital-bill-audit-app

Health ratings: **healthy** (fit for purpose as-is), **at-risk** (works but carries a known issue to resolve), **degraded** (does not work as intended). Because the product is not built, "fit for purpose" means fit as a foundation, not as product functionality.

## Summary

| Component | Kind | Location | Health |
|---|---|---|---|
| Root Layout | Server Component | `app/layout.tsx` | at-risk |
| Home Page | Server Component (page) | `app/page.tsx` | at-risk |
| Global Stylesheet | CSS | `app/globals.css` | at-risk |
| Static Assets | Assets | `public/`, `app/favicon.ico` | healthy |
| Next.js Configuration | Config | `next.config.ts` | at-risk |
| TypeScript Configuration | Config | `tsconfig.json` | at-risk |
| ESLint Configuration | Config | `eslint.config.mjs` | degraded |
| Package Manifest | Config | `package.json`, `package-lock.json`, `.nvmrc` | at-risk |
| Agent Instruction Files | Docs / tooling config | `AGENTS.md`, `CLAUDE.md`, `README.md`, `.mcp.json`, `aidlc.settings.json`, `.gitignore` | healthy |

## Root Layout

- **Location:** `app/layout.tsx`
- **Responsibility:** document shell for every route: `<html lang="en">`, fonts (Geist, Geist Mono), global CSS, site metadata.
- **Depends on:** `next` (Metadata type), `next/font/google`, Global Stylesheet, generated `LayoutProps<"/">` type.
- **Used by:** the framework for every route.
- **Health: at-risk.** Template metadata ("Create Next App"); `lang` is a product decision (English-only is fixed for Phase 1, so `en` may stand, or `en-IN` may be chosen); type-check depends on generated types.

## Home Page

- **Location:** `app/page.tsx`
- **Responsibility:** renders `/`. Currently template content only.
- **Depends on:** `next/image`, Static Assets.
- **Used by:** the framework (route `/`).
- **Health: at-risk.** Must be replaced wholesale by the product's start screen; contains `utm_*` tracking links to third-party sites.

## Global Stylesheet

- **Location:** `app/globals.css`
- **Responsibility:** Tailwind v4 entry, colour variables with dark mode, `@theme inline` tokens.
- **Depends on:** `tailwindcss` via the `@tailwindcss/turbopack` loader.
- **Used by:** Root Layout.
- **Health: at-risk.** `body { font-family: Arial, ... }` overrides the configured Geist font.

## Static Assets

- **Location:** `public/` (5 SVGs), `app/favicon.ico`.
- **Responsibility:** images and favicon served from `/`.
- **Used by:** Home Page (`next.svg`, `vercel.svg`); `file.svg`, `globe.svg`, `window.svg` are unused.
- **Health: healthy** (template leftovers to remove later).

## Next.js Configuration

- **Location:** `next.config.ts`
- **Responsibility:** framework flags (`experimental.agentFeedback`, `cacheComponents`, `partialPrefetching`) and the Turbopack CSS rule for Tailwind.
- **Used by:** `next dev`, `next build`, `next start`.
- **Health: at-risk.** `agentFeedback` needs telemetry (privacy decision); Cache Components demands a caching rule for user data; Tailwind tied to Turbopack. See `architecture.md`.

## TypeScript Configuration

- **Location:** `tsconfig.json`
- **Responsibility:** strict type-checking (`noEmit`), `@/*` alias, Next TS plugin; includes `.next/types/**` and `next-env.d.ts`.
- **Health: at-risk.** A fresh clone has neither `next-env.d.ts` nor `.next/types/`, so `tsc` fails until `next typegen` or a build runs.

## ESLint Configuration

- **Location:** `eslint.config.mjs`
- **Responsibility:** ESLint 9 flat config with Next core-web-vitals and TypeScript presets.
- **Health: degraded.** `npm run lint` (bare `eslint`) lints `.claude/` tooling, which is not ignored, and fails with 9 errors and 25 warnings. App code alone is clean.

## Package Manifest

- **Location:** `package.json`, `package-lock.json`, `.nvmrc`
- **Responsibility:** scripts (`dev`, `build`, `start`, `lint`), dependencies, Node version pin (v24.19.0).
- **Health: at-risk.** No `test`, `typecheck` or `format` script; no `engines` field, so the machine's default Node v18.12.1 (below Next 16's minimum 20.9.0) is not blocked; `@types/node` `^20` vs Node 24.

## Agent Instruction Files

- **Location:** `AGENTS.md`, `CLAUDE.md`, `README.md`, `.mcp.json`, `aidlc.settings.json`, `.gitignore`
- **Responsibility:** instructions for AI agents (Next.js agent-rules block managed by `next dev`; `nvm use` rule), MCP server declarations, AI-DLC model preset, VCS ignores.
- **Health: healthy.** `README.md` is unmodified template text; `AGENTS.md` is partly rewritten by `next dev` when `agentFeedback` is on.

## Areas outside the application (skimmed only, not rated)

| Area | What it is | Effect on the app |
|---|---|---|
| `.claude/` | AI-DLC tooling: agents, skills, hooks, 91 `.ts`/`.js` files | Breaks `npm run lint`; not type-checked (TypeScript skips dot-directories) |
| `aidlc/` | AI-DLC workspace: memory, intents, knowledge, this code KB | None (no JS/TS) |
| `PRDs/` | Product document `app-baseline.md` (212,039 bytes) | None at build |
| `node_modules/` | Installed dependencies (287 top-level entries) | Versions confirmed only |
| `.next/types/`, `next-env.d.ts` | Generated type files, gitignored and untracked | Required for type-checking |

## Sources

- Developer scan: `aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/reverse-engineering/developer-scan.md` (Scan Coverage, file classification, Technical Debt Signals).
- Code confirmed by the architect: `app/layout.tsx`, `app/page.tsx`, `app/globals.css`, `next.config.ts`, `eslint.config.mjs`, `package.json`; `git check-ignore -v next-env.d.ts`.

## Assumptions & Open Questions

None.
