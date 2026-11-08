# Project-Level Rules

> Project-specific specialisation and corrections. Loaded after `org.md` and
> `team.md` as strict-additive guidance; contradictions with broader policy
> are rejected. Populated by practices-discovery and the self-learning loop.
>
> Use sparingly: most teams don't need a project layer. Reach for it
> only when this specific project needs stable, durable guidance beyond the
> team practice (for example, package-specific release checks or an additional
> regression suite for a legacy component).

## Way of Working

<!-- Project-specific specialisation. Example: -->
<!-- This monorepo requires package-scoped branch names and a package owner -->
<!-- review in addition to the team's normal merge policy. -->

## Walking Skeleton

<!-- Project-specific specialisation. Example: -->
<!-- The walking skeleton must exercise the legacy service adapter as well -->
<!-- as the new service boundary. -->

## Testing Posture

<!-- Project-specific specialisation. -->

## Change Control

<!-- Project-specific. Mode: strict or relaxed. Strict here holds for every intent and cannot be changed from chat. -->

## Deployment

<!-- Project-specific specialisation. -->

## Code Style

<!-- Project-specific specialisation. -->

## Tech Stack

<!-- Technology choices locked for this project. -->

- UI is built with Tailwind for styling and shadcn/ui components copied into the repo (components/ui/); prefer the native element listed in design-system-mapping.md when a shadcn component pushes a route over the 300 KB first-load limit (user: "use tailwind and shadcn"). (learned 2026-11-07) <!-- cid:261026-bill-audit-baseline:refined-mockups:c65603d85b63f03d39a52e805bf38a837cc7620de4c0fdbc9501a05e8b36fe1d -->

## Decided

<!-- Decisions made in earlier stages that should not be re-asked. -->
<!-- Format: DECIDED: [decision] (Stage [slug], [date]) -->

## Scope Overrides

<!-- Custom scope rules for this project. -->

## Forbidden

<!-- Populated by practices-discovery affirmation gate. -->
<!-- Format: NEVER [behavior] (affirmed [date]) -->
<!-- Example: NEVER throw exceptions across service layer boundaries (affirmed 2026-05-17) -->

## Mandated

<!-- Populated by practices-discovery affirmation gate. -->
<!-- Format: ALWAYS [behavior] (affirmed [date]) -->
<!-- Example: ALWAYS use Result<T,E> for fallible operations in service layer (affirmed 2026-05-17) -->

None. (affirmed 2026-11-04)

## Corrections

<!-- Project-specific corrections from human feedback. -->
<!-- Format: NEVER/ALWAYS [behavior] (learned [date]) -->
- PRD read via the document library, not directly: PRDs/app-baseline.md exceeds the 200,000-character direct-read limit, so it is read from aidlc/spaces/default/knowledge/documents/app-baseline.md (document id 01a17987); that extraction is also truncated at 200,000 characters, ending partway through decision D30. (learned 2026-10-26) <!-- cid:261026-bill-audit-baseline:intent-capture:a84b42921d9fa487c3d6feebdefb814956fb25c290c4cf9f37bbf68f07ac42a2 -->
- Do not re-ask decisions the PRD already settles: launch geography (D17), Phase 1 free (D25), reference data built from public sources (D22) and the patient-side brand stance are listed as settled in a stage's questions file instead of being asked again. (learned 2026-10-27) <!-- cid:261026-bill-audit-baseline:market-research:f98b6f349cbb9f02b28fa5bfd3f90f36517a4e476cb1aa39e4c3213125353c57 -->
- Hosting is not decided: the user's answer was "Mostly Vercel. But I would like to decide at a later point." Treat Vercel as the working assumption, not a decision, and record free-text answers verbatim as Other answers. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:feasibility:c430bcf8153dd972a253f5156deaed4e76cf02bf6cbdf7cac65f8b311c298fb9 -->
- Pay-under-protest guidance and urgent-situation guidance (TS-03, PRD gap G-12) are moved to Phase 1.5 and are out of this build (user: "Move this scenario to phase 1.5"); the hard constraint against encouraging payment withholding that blocks discharge (C10) still applies to all report wording. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:scope-definition:a5d51639ad3c035618259cddad514f83a39dfe7e7f4f0a9d8d11cb8797b9eec9 -->
- For PRD review gaps (G-01 to G-23), ask the user only about gaps whose suggested resolution offers a real choice; apply gaps with a single suggested resolution directly as decisions and list them in the stage artifact. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:scope-definition:65402fe67936fea72906a2f472b9bc8c8db3ddb9f3bfc0c4ccf5e41d320d1cdf -->
- Take the screen list and screen order straight from the PRD's journey J1 instead of asking about entry points and the happy path; the PRD already specifies both. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:rough-mockups:3ce8fd280128f1989160f679df565d4bd16147a994a9f15373b7ae731b097cba -->
- In text sketches, draw the phone layout of each screen and add a written note on the desktop layout rather than drawing both; this honours 'phone and desktop designed equally' while keeping the sketches readable. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:rough-mockups:0daddb2e6c6a708094499cdf3c443a2eba2181739339cec8841d0816b14280a3 -->
- When a stage is approved with agreed but unapplied changes (for example design-check findings accepted at the gate), carry them into the next stage as an explicit item and ask how to handle them first, so they are not lost. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:approval-handoff:0c4bd17e3c2b2927f5ba003e3486b5047de0f0ab88066d1982bee39312b45a90 -->
- Team formation was skipped because the team is one builder plus the founder; do not ask whether mobs are staffed, and instead confirm that team plan. (learned 2026-11-01) <!-- cid:261026-bill-audit-baseline:approval-handoff:f7c409f57d6dda8c16b109a3abfbf07008acc347b84506ac70ed3443bf113e4f -->
- Keep the primary action colour (deep teal) distinct from the finding-class colours (amber Likely error, blue Worth asking, grey Explained), so a button is never mistaken for a finding class; red is reserved for system errors and Discard. (learned 2026-11-07) <!-- cid:261026-bill-audit-baseline:refined-mockups:b3495878a65ab9fc2182fc7af2731942c91b07ea8501fe774c8c5ddfcc84b206 -->
- Clickable prototypes are a single self-contained HTML file in the stage folder (no external scripts, fonts or network), using made-up data and mirroring the design tokens, so they open offline and can be sent as a file. (learned 2026-11-07) <!-- cid:261026-bill-audit-baseline:refined-mockups:7718d3c38d053e77299b8303e5e7ea7e5232795762222b84a3e0c0c7699cf39f -->
- For every question put to the user, also state the recommended option and a one-line reason, and mark that option "(Recommended)" (user: "For any questions also tell me your receommended approach.") (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:units-generation:d4cbc525d4f9228a352c32efa9dbb5ebaae714695f034a5053fba2060ff1930c -->
- Every state-changing REST endpoint (family and internal) checks the Origin header against our own site, on top of the same-site session cookie, to block cross-site requests (Contract Design D-CD8). (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:contract-design:d7d3074791225a93ed6549e46646a2b1fab3981dc219d0c39174909e43f12c74 -->
- Browser-to-server calls are REST route handlers described with OpenAPI, not Server Actions, on both the family side and the internal tools (user chose REST over the recommended Server Actions, Contract Design Q2 B and Q2a). (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:contract-design:e97421dc5eecef9882aee805decbda837ff3804e0b00e749d9dbaaffcee23aad -->
- When a new cross-unit call would add or reverse a unit dependency, prefer an interface declared in the contracts unit and implemented by the provider (as with ScheduledJob and BetaHandoverSink), so the approved unit dependency map stays unchanged. (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:contract-design:10da5b5d7775f7839a37d6f63c84a7f8fd68c7f6aa69c4a4888759d5d0c85f12 -->
- Construction is built unit by unit; the walking skeleton (Bolt 1) is built with the contracts unit plus thin stand-ins behind each contract for the other units on its path, and each later unit replaces its stand-in (document-intake's stand-in deletes originals for real). (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:delivery-planning:cd431754b4821f7cb47e993a63d664c30c217aeb247947375d089f395cc9dc05 -->
- Unit-by-unit building was chosen over parallel batch builds: early working code in risk-first order matters more than parallelism for one builder, so independent units are not built side by side. (learned 2026-11-08) <!-- cid:261026-bill-audit-baseline:delivery-planning:2c41ab80db4d43ce4de6a004202dc56952f4df54b6472f05c91a93a8689d91b4 -->
