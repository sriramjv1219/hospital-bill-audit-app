# Reverse Engineering Timestamp — hospital-bill-audit-app

| Field | Value |
|---|---|
| Date | 2026-11-04 |
| Branch | `main` |
| Commit | `818076b0defe3020874ac822bb8f46f67037fc10` ("chore: Initial setup with AIDLC harness with Claude.") |
| Parent commit | `19fa65e` ("Initial commit from Create Next App") |
| Source fingerprint (snapshot, `./`) | `git:63722944ad56aba364a9bc270e776ba4416614dc` |
| Prior store | none (first scan) |
| Breadth chosen | Full scan of the whole repo |
| Intent | `261026-bill-audit-baseline` |
| Pipeline | Developer scan (link 1) + architect synthesis (link 2) |

## Coverage Notes

- **Analyzed deeply:** all application code and its configuration (`app/`, `public/`, the root config and manifest files listed below), plus the root agent-instruction and tooling config files.
- **Skimmed only:** `.claude/` (AI-DLC tooling), `aidlc/` (AI-DLC workspace), `PRDs/` (product document), `node_modules/` (installed versions only).
- **Read but not fingerprintable:** `next-env.d.ts` and `.next/types/` are generated, gitignored and untracked. They were checked, but are recorded as shallow because the source fingerprint covers tracked content only.
- **Why `kind: partial`:** the scan covered the whole application deeply but deliberately only skimmed the `.claude/` tooling tree (91 `.ts`/`.js` files), `aidlc/` and `PRDs/`. A full deep read of the repo did not happen, so the block does not claim `./`.

## Sources

- Developer scan: `aidlc/spaces/default/intents/261026-bill-audit-baseline/inception/reverse-engineering/developer-scan.md` (Scan Coverage, header).
- `git rev-parse HEAD`; `aidlc engine workspace codekb-scope-diff --mint` over the analyzed paths below (2026-11-04).

## Assumptions & Open Questions

None.

## Scope of Analysis

```yaml
scope_version: 1
kind: partial
intent: 261026-bill-audit-baseline
fingerprint: 9697bc9f1186701b769c7f4d6f68a50b4c5338e1
analyzed:
  paths:
    - app/
    - public/
    - package.json
    - package-lock.json
    - tsconfig.json
    - next.config.ts
    - eslint.config.mjs
    - .nvmrc
    - .gitignore
    - README.md
    - AGENTS.md
    - CLAUDE.md
    - aidlc.settings.json
    - .mcp.json
  components:
    - Root Layout
    - Home Page
    - Global Stylesheet
    - Static Assets
    - Next.js Configuration
    - TypeScript Configuration
    - ESLint Configuration
    - Package Manifest
    - Agent Instruction Files
shallow:
  paths:
    - .claude/
    - aidlc/
    - PRDs/
    - node_modules/
    - .next/types/
    - next-env.d.ts
```
