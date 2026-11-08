# External Dependency Map — Hospital Bill Check (Phase 1)

This document lists the decisions and outside services the build waits on, mapped to the Bolts that need them. A **Bolt** is one build pass over a small piece of the work that ends in something that runs and is merged (`bolt-plan.md`). A Bolt marked **deploy-blocked** is built and tested locally, but cannot be deployed until its dependency is in place.

## Sources

- Answers Q3 and Q4 in `delivery-planning-questions.md` in this folder (build locally until hosting is decided; GitHub with GitHub Actions).
- Open questions in `inception/requirements-analysis/requirements.md` (OQ1 residency, OQ2 hosting, OQ6 privacy wording, OQ8 reader trial) and assumption AS6 (de-identification).
- Contracts and their open questions: `inception/contract-design/contract-summary.md` (C3 reader, C4 identity provider, scheduler, chunk size).
- Team practices: `inception/practices-discovery/team-practices.md` (no real bills until hosting and residency are decided; nothing deployed until hosting is decided).
- Accepted privacy risk on consented records: `inception/domain-design/decisions.md` ADR-008. Units: `inception/units-generation/unit-of-work.md`.

## Dependencies

| # | Dependency | Owner | Needed by | Blocks | Lead time | If it slips |
|---|---|---|---|---|---|---|
| X1 | GitHub repository and GitHub Actions set up (Q4) | Builder | Before Bolt 1a | Bolt 1a and every merge after it | Hours | Bolt 1 cannot finish: the checks before merge need the remote |
| X2 | Hosting decision (OQ2; Vercel is a working assumption only) | Founder | Before Bolt 42 (first deployment) | Deployment of everything; the real worker host (Bolt 31) and scheduler (Bolts 23, 35); staging timing and load test (Bolt 42) | Founder's decision | Build continues locally with stand-ins; deploy-blocked Bolts wait; launch waits |
| X3 | Data-residency decision (OQ1: India only or India first) | Founder | Before any real bill reaches any environment, and before Bolt 42 | Choice of stores, reader service and host region; real-bill accuracy runs on hosted services | Founder's decision | Real-bill accuracy runs stay on the offline machine only (allowed, D-UG6); hosted work waits |
| X4 | Bill-reading service candidates with data-processing terms and a no-training term (NFR19) | Builder | Before Bolt 3 (trial on synthetic bills); before any real-bill run (M5) | Reader trial (Bolt 3); real-bill accuracy runs | Days to weeks per vendor contract | Bolt 3 runs on whichever candidate is contracted first; real-bill runs wait |
| X5 | Reader trial result (OQ8) | Builder | Before the reader is wired into the family journey (Bolt 31) | Choice of the production reader adapter | After Bolts 3 and 14 | Keep the best candidate behind the adapter; swap later without changing checks (ADR-005) |
| X6 | Staff identity provider and sign-in method (D-US4; set in NFR Requirements) | Builder, in NFR Requirements | Before Bolt 12 is deployed | Real staff sign-in (Bolt 12), internal tools (Bolts 40, 41) | Set during Construction design | Bolt 12 uses a local stand-in for sign-in; deployment of internal tools waits |
| X7 | Consented real bills from the personal network, with written consent (FR16.2) | Founder and builder | Before promotion decisions (Bolt 26 onward) | Real-bill measurement; promoting any check to live (FR16.12) | Weeks (collection, redaction, two labels each) | Checks stay in shadow; launch goes ahead with only the checks that pass (D-US7) |
| X8 | Counsel review of privacy wording (OQ6) and of the accepted re-identification risk (ADR-008, AS6) | Founder | Before launch (Bolt 42) | Launch | Weeks | Launch waits; any change is isolated in feedback-insights (consented record) and the wording texts |
| X9 | Public reference data sources: NPPA ceiling prices, device caps, IRDAI Lists II to IV, grievance timelines | Builder | Before Bolt 11 | Price checks on real data (Bolts 9, 11) | Days | Bolt 9 uses fixture data until Bolt 11 loads the real lists |
| X10 | Offline machine for accuracy runs, outside the AI assistant's reach (D-UG6, FR16.13) | Builder or founder | Before the first real-bill run (M5) | Real-bill accuracy runs | Hours | Only synthetic-bill runs happen until it is set up |

## Deploy-blocked Bolts

| Bolt | Why it is deploy-blocked | Unblocked by |
|---|---|---|
| 23 (3-month deletion job) | Needs a real scheduler | X2 |
| 31 (background worker) | Needs a real worker host and job runner | X2 |
| 35 (idle sweep) | Needs a scheduler that can run every 60 seconds | X2 |
| 42 (launch readiness) | Needs hosting, residency and counsel review | X2, X3, X8 |

Every other Bolt is built, tested and merged locally. None of them is deployed until hosting is decided (`team-practices.md`).

## Assumptions & Open Questions

- No dates are set yet for X2, X3 or X8. The plan treats the hosting decision as the gate before the first deployable milestone. The founder owns all three.
- X4 vendor lead times are unknown until vendors are approached; Bolt 3 is scheduled to use whichever is ready first.
