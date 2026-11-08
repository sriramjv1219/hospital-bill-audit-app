# User Stories Assessment — Hospital Bill Check (Project RS, Phase 1)

## Decision

**Execute.**

## Rationale

The Phase 1 build is mostly user-facing. A family member works through one anonymous session: consent, upload, review, report, then action. The internal tools behind it have users of their own: the rule library, the Labelled Test Set and the ops views. The requirements (`inception/requirements-analysis/requirements.md`) say what the system must do. They do not say who needs each behaviour, in which situation, or how a tester can confirm it in Given/When/Then form. The PRD lists that last gap as G-21, and the requirements carry it as open question OQ7, owned by this stage.

## Factors considered

| Factor | Finding | Source |
|---|---|---|
| Project type | Brownfield in name only: the repository is an unmodified Create Next App scaffold, so every behaviour is new | `codekb/hospital-bill-audit-app/business-overview.md` |
| User-facing scope | 13 of 16 functional groups (FR1–FR13) are seen by families; FR14–FR16 are used by the builder and the founder | `requirements.md` §2 |
| Personas | Five primary customer types, from the family member at discharge to the bereaved family, plus internal rule owners and labellers | `ideation/intent-capture/intent-statement.md` (Target Customer) |
| Complexity | Health data under the DPDP Act; consent rules for minors and third parties; 18 launch checks with precision gates; strict wording guardrails | `requirements.md` FR1, FR8, FR13, FR16 |
| Open review findings | The requirements were approved with eight review findings accepted as open risks (R-01 to R-08). Four of them are about missing pass/fail tests, which acceptance criteria can supply | `inception/requirements-analysis/reviews/review-01.md` |

## Where stories add the most value

- **Consent and declarations (FR1):** each relationship path (self, minor, adult third party, incapacitated or deceased patient) needs its own testable path.
- **Data lifecycle (FR2):** deletion of the original file, the idle timeout and storage consent are the zero-tolerance privacy guardrails, and they need exact Given/When/Then tests.
- **Upload edge cases (FR4):** limits, missing pages, unreadable pages, summary bills and password-protected PDFs.
- **Findings and wording (FR8, FR9, FR13):** what a family sees for each finding class, and the banned-word guardrail.
- **Report and sharing (FR10, FR11):** the phone share sheet versus desktop download, and the one-page Billing Desk Summary.
- **Internal tools (FR14–FR16):** promoting a check to live only with a recorded test pass.
- **Closing open review findings:** acceptance criteria can give measurable tests where the requirements left them open, without renumbering any requirement.

## Story plan (confirmed through Q28)

- Personas: four customer personas and one internal persona, in `personas.md` [Q1].
- 85 stories with acceptance criteria (364 criteria) and 8 Won't Have stories, in `stories.md` [Q2], [Q3], [Q7]. They sit in 11 groups: the family's journey, then the internal tools, then the Won't Have items.
- That is more than the "about 45–60" in [Q3]. Keeping one capability per story, as Q3 asked, produced 76. The specialists' review then added foundation stories (session, background processing, reference data, the bill-stage question) and split two oversized stories (bill reading, report download).
- Judgment calls raised by the designer, developer and quality engineer were settled in Q17–Q28. Their technical corrections are folded in, and no objection remains open.
- The launch checks are written as one story per check family, with each check as its own acceptance criterion [Q4].
- Every requirement ID is mapped in `traceability.json`. The system-wide non-functional requirements are deferred to named later stages [Q6].
