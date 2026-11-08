**Collaborator:** aidlc-quality-agent

## Contribution

Independent review of testing posture, coverage tooling, CI quality gates and test patterns. I read the lead draft (`team-practices.md`, `discovered-rules.md`, `evidence.md`), the code knowledge base (`code-quality-assessment.md`, `technology-stack.md`, `architecture.md`, `business-overview.md`), `package.json`, `tsconfig.json`, `eslint.config.mjs`, `next.config.ts`, `.nvmrc`, `.gitignore`, `.claude/scopes/aidlc-enterprise.md`, the intent's `aidlc-state.md`, the bundled testing guides in `node_modules/next/dist/docs/01-app/02-guides/testing/` (Vitest, Playwright, Jest, Cypress), and the PRD (`aidlc/spaces/default/knowledge/documents/app-baseline.md`, §8.4 quality targets, §12.14 LT-01 to LT-13, KB-04, NFR-01 to NFR-20, G-16). I confirm the lead's baseline facts: 0 tests, no runner, no `test` script, no coverage configuration, no CI, and a `npm run lint` that fails on `.claude/` tooling (TD-01).

### Finding 1: this product has two separate quality systems, and the draft merges them

The draft's Testing Posture treats "tests plus 80% coverage" and "the accuracy suite" as one posture. They are different instruments with different rules, and the team practice should name both:

| | A. Code test suite | B. Accuracy evaluation (Labelled Test Set) |
|---|---|---|
| What it proves | The code does what the requirements say (checks, arithmetic, upload, deletion, report) | Bill reading and findings are accurate on real-world bills |
| Inputs | Synthetic fixtures only, committed to the repository | Labelled Test Set: synthetic bills (LT-03) plus redacted, consented real bills (LT-02, LT-04, LT-06) |
| Determinism | Fully deterministic: the AI vendor is replaced by a stub or recorded responses | Not fully deterministic (AI reading); measured as rates |
| Target | 80% line-coverage floor (enterprise scope); all tests green | Extraction ≥ 95% on digital PDFs, ≥ 90% on good photos; finding precision ≥ 90% for "Likely error" and ≥ 75% for "Worth asking" (PRD §8.4, NFR-05) |
| When it runs | Every push and before every merge (CI gate) | Before a check leaves shadow mode (KB-04), and on every prompt, model or rule change before release (NFR-17); not on every pull request |
| Where data lives | Git | Outside git, access-controlled (see Finding 4) |

Correction to the draft: it names only the two extraction targets. The two **finding-precision** targets (≥ 90% "Likely error", ≥ 75% "Worth asking") are the ones that decide whether a check goes live, so they belong in the posture too. PRD gap G-16 (no minimum number of positive and negative examples per check) is still open and directly affects whether a precision result means anything; it should be listed for Requirements Analysis or NFR Requirements to settle.

### Finding 2: testing methodology recommendation

I support the lead's suggestion of `custom`, and suggest this concrete wording for the interview to confirm or change:

- **Methodology**: custom
- **Ordering**: For each bill check, money calculation and report rule, we write the expected-outcome tests first (from the requirement and the planted-error answer key) and then implement until they pass; for UI, upload plumbing and integration code, we implement first and then write and run that layer's tests before the Bolt can merge.

Why: the checks decide what a patient is told, a false positive harms the user (PRD RK2, risk score 16), and the synthetic bills already come with a planted answer key (LT-03, LT-08), which is a ready-made set of expected outcomes. Test-first is cheap exactly where the stakes are highest. Test-after is the pragmatic choice for UI, where design is still moving. Plain `test-after` everywhere (the org default) is a valid answer, but the human should choose it knowingly.

### Finding 3: coverage tooling and what "80%" is measured over

- The 80% line floor only means something if the denominator is defined. Recommend: measured by the unit/integration runner (Vitest with the V8 coverage provider), over application source (`app/`, `lib/` or the agreed source folders), with an **explicit, reviewed exclusion list** (generated files, `next-env.d.ts`, config files, `.claude/`, `aidlc/`, test files). Any new exclusion is a visible change in review, never a way to pass the floor.
- The threshold is enforced inside the runner's configuration (`coverage.thresholds`), so it fails locally and in CI the same way, instead of being checked only in a CI script.
- Bundled Next.js guide constraint: Vitest does not support `async` Server Components (`vitest.md`: "we recommend using E2E tests for `async` components"), and end-to-end coverage does not feed the line-coverage figure. Practical consequence for code patterns: keep `async` Server Components and route handlers thin, and put the logic (parsing, normalising line items, checks, totals, report composition) in plain TypeScript modules that unit tests can reach. Otherwise the 80% floor becomes hard to meet honestly.
- Suggested stricter floor (for the human to decide): **branch** coverage of at least 90% for the bill-check and money-calculation modules, because a missed branch there is a wrong finding. Line coverage alone can stay at 80% for the rest.

### Finding 4: test data and privacy, additions to the candidate rules

- The draft's candidate Forbidden rule on real bills is right; I would add two concrete edges:
  1. Real (even redacted) bills from the Labelled Test Set are never committed to git. LT-11 says contributors can withdraw a bill at any time and it must be deleted; git history makes that deletion practically impossible. Synthetic bills may be committed.
  2. End-to-end tests use only synthetic bills, because Playwright traces, screenshots and videos (normally saved on failure and uploaded as CI artifacts) capture whatever was on screen.
- No real AI-vendor calls in the per-merge suite: the vendor sits behind an interface and tests use a stub or recorded responses. This keeps the suite deterministic, free, and free of bill content leaving the machine. Vendor keys are then needed only where the accuracy evaluation runs.
- Test data patterns: a synthetic-bill builder or factory (line items, totals, room days, MRP, implants) with sensible defaults overridden per test; committed fixture PDFs and photos for upload and reading paths; each test owns its data.

### Finding 5: test types the Comprehensive strategy and the PRD call for

Test Strategy Comprehensive means roughly 10 to 15 tests per component across unit, integration, end-to-end, performance and security. Mapped to this product:

| Type | What it covers here | Source |
|---|---|---|
| Unit (most tests) | Each check (LOS, RM, RX, CON, IMP, PKG), totals and arithmetic, Indian number formats (lakh/crore, ₹, IST), report wording rules | PRD §13, NFR-14 |
| Property-based (fast-check) | Arithmetic and money invariants (sum of lines equals total; amounts in integer paise, never floating point); number-format round trips | NFR-07, NFR-14 |
| Reproducibility | Same inputs and rule versions give identical findings (golden-output test per check) | NFR-07 |
| Integration | Upload to reading to checks to report, with the AI vendor stubbed; **original file deleted after reading, including when reading fails** | NFR-09, candidate Mandated rule |
| Privacy guards | No bill data in a shared `use cache` scope or prerendered output; no personal data in logs | TD-04, NFR-09 |
| Content safety | Automated check of report and template wording against a banned-phrase list (no accusatory language, nothing that encourages withholding payment in a way that blocks discharge, C10) | NFR-18, C10 |
| End-to-end (Playwright, few) | Journey J1 on a phone viewport and a desktop viewport, on the built app (`next build` then `next start`) | J1, NFR-12 |
| Accessibility | `@axe-core/playwright` scan inside the end-to-end tests; failures block merge | NFR-13 |
| Security | Delegated to devsecops (dependency and secret scanning, SAST) | — |
| Performance | See gap below | NFR-01, NFR-02, NFR-04, NFR-15 |

**Gap to surface:** this intent skips Performance Validation (4.6) and all other Operation stages, yet NFR-01 (first insight within 5 minutes P90; 10 minutes for photographed multi-page bills), NFR-02 (full report within 15 minutes P90), NFR-04 (99.5% availability) and NFR-15 (10x pilot volume) are measurable targets. Under the Comprehensive strategy, someone must say where they are validated. Options: a basic timing and load test in Build and Test against a non-production environment using synthetic bills, or an explicit deferral recorded as a known gap. This is a human decision.

### Finding 6: CI quality gates (order and blocking status)

Proposed per-merge gate, all blocking, in this order so the cheap checks fail first: `nvm`-pinned Node from `.nvmrc` → `npm ci` → `next typegen` → type-check → lint (with `.claude/**` and `aidlc/**` ignored, TD-01) → format check (if a formatter is adopted) → unit and integration tests with coverage thresholds → `next build` → end-to-end and accessibility tests against the built app. Target total run time under 10 minutes.

Separate, non-per-merge gates:
- **Accuracy evaluation**: required before a check is promoted out of shadow mode and before any prompt, model or rule change is released (NFR-17); its report records the per-check precision, the number of examples behind it (G-16), and the set's coverage (LT-12).
- **Flaky tests**: a flaky test is fixed or quarantined with a tracked reason within the same or the next Bolt; it is never silently retried until green.
- **Defects**: every defect found gets a failing test that reproduces it before the fix; every wrong finding reported in the beta becomes a Labelled Test Set case.

### Finding 7: on the walking skeleton and tooling timing

- I agree the skeleton must include the test runner, one unit test, one end-to-end test and CI. I would add: the coverage threshold is configured and enforced from Bolt 1 (not added later), the AI-vendor stub interface exists from Bolt 1, and the end-to-end test runs on a phone viewport.
- The draft defers the runner choice to NFR Requirements or Build and Test. Build and Test runs after code generation, which is too late: Bolt 1 installs the runner. I recommend affirming the team default here (Vitest plus Playwright, both with bundled Next.js 16 guides), with NFR Requirements free to revisit it with a reason.
- Optional: an evaluation-harness skeleton that runs the accuracy measurement on a handful of synthetic bills. The Labelled Test Set is on the launch path (scope document, item 5), so proving the harness early lowers risk. Human decision whether it belongs in Bolt 1 or later.

### Testing questions the human must decide

1. **Methodology and ordering**: `custom` as worded in Finding 2 (expected-outcome tests first for checks, money calculations and report rules; test-after for UI and plumbing), or plain test-after everywhere, or TDD everywhere?
2. **Coverage**: keep the 80% line floor everywhere, or add a stricter floor (for example 90% branch coverage) for the bill-check and money-calculation modules?
3. **Test runners**: affirm Vitest (unit, integration, coverage) and Playwright (end-to-end, accessibility) now, or leave the choice to NFR Requirements?
4. **Accuracy evaluation cadence**: does it gate (a) check promotion out of shadow mode and (b) every prompt, model or rule change before release, and should it also run on a schedule (for example weekly) to catch drift (NFR-17)?
5. **Where the Labelled Test Set lives** (outside git), and whether the evaluation may run on a hosted CI runner with redacted real bills or only on a controlled machine. Linked to the open hosting and residency decisions (F2, F3).
6. **Performance targets** (NFR-01, NFR-02, NFR-04, NFR-15): validated in Build and Test on synthetic bills, or recorded as a known gap because Performance Validation is skipped in this intent?
7. **Minimum examples per check** before promotion (G-16): decide now as a practice, or leave it to Requirements Analysis?
8. **Candidate rules**: confirm the additions in Finding 4 (no Labelled Test Set bills in git; synthetic bills only in end-to-end tests; no live AI-vendor calls in the per-merge suite).
9. **Evaluation harness in the walking skeleton**: yes, or later?

## Positions

- AGREE: Baseline facts — 0 tests, no runner, no coverage configuration, no CI and failing lint (TD-01) match the evidence I re-read.
- AGREE: Enterprise floor — 80% line coverage plus tests in CI before merge, never lowered to pass a step, matches `org.md` and Build and Test policy.
- AGREE: `custom` as the credible methodology alternative — test-first fits checks with an existing answer key and high false-positive cost; I supply concrete Ordering wording in Finding 2.
- AGREE: Vitest plus Playwright as tooling — both have bundled Next.js 16 guides; Vitest's lack of `async` Server Component support is covered by end-to-end tests.
- AGREE: Skeleton includes test runner, one unit test, one end-to-end test and the CI fixes (lint ignores, `typecheck` with `next typegen`, `engines`) — CI cannot be green without them.
- AGREE: Candidate Forbidden rule on real patient bills in tests — required by DPDP constraints CR-R1 and CR-R2; I add two concrete edges in Finding 4.
- OBJECT: Accuracy targets as stated — the draft lists only extraction accuracy; the finding-precision targets (≥ 90% "Likely error", ≥ 75% "Worth asking") that decide go-live are missing, and G-16 is not mentioned.
- OBJECT: Accuracy suite placement — the draft says only that it runs "in addition to the coverage floor"; it must state that it is not a per-merge gate but gates shadow-mode promotion and every prompt, model or rule change (NFR-17), with data kept outside git.
- OBJECT: Deferring runner choice to NFR Requirements or Build and Test — Bolt 1 installs the runner, and Build and Test runs after code generation; the team default should be affirmed at this stage.
- OBJECT: Silence on performance NFRs — Performance Validation is skipped, so NFR-01, NFR-02, NFR-04 and NFR-15 have no validation home; the interview should place or explicitly defer them.
