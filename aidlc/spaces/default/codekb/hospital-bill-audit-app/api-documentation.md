# API Documentation — hospital-bill-audit-app

## Summary

The application exposes **one HTTP page route and no programmatic API**. There are no Route Handlers, Server Actions, REST, GraphQL or gRPC endpoints, and no internal shared modules with a public interface.

## External API Surface

### HTTP pages (App Router)

| Method | Path | Source | Response | Auth | Params |
|---|---|---|---|---|---|
| GET | `/` | `app/page.tsx` (wrapped by `app/layout.tsx`) | HTML, static template home page | None | None |

Confirmed by generated `.next/types/routes.d.ts` (`AppRoutes = "/"`): no dynamic segments, no redirects, no rewrites.

Framework-served static paths (not application endpoints): `/favicon.ico`, `/next.svg`, `/vercel.svg`, `/file.svg`, `/globe.svg`, `/window.svg`, plus Next.js internal `/_next/*` assets (self-hosted fonts, CSS, JS).

### Route Handlers, Server Actions, RPC

None. No `route.ts` files and no `"use server"` directives exist.

### Request interception

None. No `proxy.ts` (the Next 16 name for `middleware`) and no `instrumentation.ts`.

## Outbound Calls

| Kind | Target | When | Notes |
|---|---|---|---|
| Build-time download | Google Fonts (Geist, Geist Mono) via `next/font/google` | `next build` / `next dev` | Self-hosted afterwards; no runtime call to Google. |
| User navigation links | vercel.com, nextjs.org (with `utm_*` parameters) | User click | Plain `<a>` links in `app/page.tsx`; template content to be removed. |
| Telemetry | Next.js Telemetry (required by `experimental.agentFeedback`) | Dev tooling | Not an app API, but data leaves the machine; see `architecture.md`. |

No runtime server-to-server calls from application code.

## Internal APIs

None. The two modules (`RootLayout`, `Home`) are framework entry points, not reusable interfaces.

## Contracts and Data Shapes

None defined. There are no TypeScript domain types, schemas (for example zod), or OpenAPI documents. Contract Design starts from nothing.

## Sources

- Developer scan: `aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/reverse-engineering/developer-scan.md` (APIs Discovered).
- Code: `app/page.tsx`, `app/layout.tsx`, `next.config.ts`; generated `.next/types/routes.d.ts`.

## Assumptions & Open Questions

None.
