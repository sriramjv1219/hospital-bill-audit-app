# Intent Statement — Hospital Bill Check (Project RS, Phase 1)

This build delivers the product that the PRD at `PRDs/app-baseline.md` describes [desc], bounded to its Phase 1 Hospital Bill Check MVP and the internal tooling that Phase 1 needs [Q12].

## Problem Statement

Patients and their families cannot tell whether a hospital bill is correct and justified, and they lack the knowledge, time and evidence to question it with the hospital [Q1].

The pains this build addresses are [Q3]:

- The bill cannot be read, or the hospital has handed over only a summary bill instead of an itemised one [Q3].
- There is no time to review the bill at discharge, and the family is under pressure to pay and leave [Q3].
- The family does not know whom to approach at the hospital, what to say, or how to escalate [Q3].
- Fear and emotional stress, especially for families of very ill or deceased patients [Q3].
- Managing a hospitalisation remotely, for example an NRI family member working from forwarded photos of the bill [Q3].
- Not knowing whether a price is reasonable; this is always treated as a question to put to the hospital, never as an accusation [Q3].

Insurer-side problems (policy and settlement opacity) are not part of this build's problem statement [Q1] [Q12].

## Target Customer

| Customer | Role in this build | Source |
|---|---|---|
| Self-paying patient and family | Primary customer | [Q2] |
| Family member handling the bill at discharge | Primary customer | [Q2] |
| Remote family manager (e.g. an NRI child abroad) | Primary customer | [Q2] |
| Patient paying upfront who will claim reimbursement later | Primary customer | [Q2] |
| Bereaved family settling the bill of a patient who died in hospital | Primary customer | [Q2] |
| Senior citizen | Supported, usually through a family member | [Q2] |
| Cashless insured patient | Served, but not targeted | [Q2] |
| Hospital billing desk staff | Not a user; the key stakeholder whose acceptance decides whether a finding becomes a corrected bill | [Q2] |

The customer benefits by being able to see what is questionable on a hospital bill and to raise it with the hospital with evidence, before or after paying [Q1] [Q3].

## Success Metrics

Success is measured with the PRD's Phase 1 metric tree and guardrail metrics, adopted as written and treated as initial hypotheses to recalibrate after the first 500 completed audits [Q4].

| Area | Metric | Initial target | Source |
|---|---|---|---|
| North Star | Actionable Audit Rate: share of completed audits with at least one "Likely error" or "Worth asking" finding | Baseline in the first month | [Q4] |
| Value | Objective value flagged: median rupee value of "Likely error" findings per audit (money flagged, never "saved") | Baseline in the first month | [Q4] |
| Activation | Audit completion rate | At least 60% | [Q4] |
| Activation | Report download rate | At least 70% | [Q4] |
| Speed | Time to first insight, from upload complete to summary shown, for typed or PDF bills | At most 5 minutes at the 90th percentile | [Q4] |
| Speed | Time to full report, from upload complete | At most 15 minutes at the 90th percentile | [Q4] |
| Engagement | Action intent: sessions with findings marked "Will ask" or a template downloaded | At least 40% | [Q4] |
| Quality | Line-item extraction accuracy against the Labelled Test Set | At least 95% on digital PDFs; at least 90% on good-quality photos | [Q4] |
| Quality | "Likely error" finding precision against the Labelled Test Set; a check goes live only when it meets the target | At least 90% | [Q4] |
| Quality | "Worth asking" finding precision against the Labelled Test Set | At least 75% | [Q4] |
| Quality | Finding usefulness: share of findings rated "helpful" in the session | Tracked | [Q4] |
| Coverage | Unreadable-line rate, including lines in Hindi or regional scripts, by State | Tracked | [Q4] |
| Satisfaction | Report usefulness: "This report helped me" rated 4 or 5 out of 5 | At least 80% | [Q4] |
| Satisfaction | Net Promoter Score at the end of the session | At least 40 | [Q4] |
| Growth | Referral share from self-reported "how did you hear about us" | At least 20% | [Q4] |
| Trust | Personal-data incidents | Zero | [Q4] |
| Guardrail | "Likely error" findings later shown to be wrong | Near zero, each instance root-caused | [Q4] |
| Guardrail | Findings phrased as accusations | Zero | [Q4] |
| Guardrail | Stored records older than 3 months | Zero | [Q4] |

Whether billing desks actually correct bills is not measured in this metric tree; that gap (PRD finding G-02) stays open for the founder to decide [Q16].

## Initiative Trigger

The trigger is a market gap: today's options are generic free bill checkers, grievance firms that step in only after a claim is rejected, and broker or employer claim desks that do not audit the hospital [Q5].

## Initial Scope Signal

| Signal | Value | Source |
|---|---|---|
| Workflow-selected scope | `enterprise` (workflow-selected) | [scope] |
| Process the user confirmed | The enterprise process, stopping before the operation phase; right after this stage is approved, the exact plan change is proposed for approval | [Q13] [Q18] |
| Product boundary the user confirmed | The Phase 1 Hospital Bill Check MVP, including its internal modules and the P0 tooling it needs (rule library, Labelled Test Set, ops/admin) | [Q12] |
| Out of the confirmed boundary | Phase 1.5 (Expert Review) and every later phase (Insurance Check, Scale and B2B2C, Ecosystem) | [Q12] |
| Timeline | No fixed timeline; the PRD's founder decision D21 (launch about 3–4 months after the team is in place) does not apply to this build | [Q11] [Q17] |
| PRD open items | The PRD's working defaults and suggested resolutions are used as decisions; where a suggestion lists alternatives, that item is resolved at the stage that first needs it | [Q10] [Q15] |
| PRD coverage | The unread end of the PRD (rest of decision D30, glossary, sources, version history) is accepted for intent capture | [Q14] |

## Assumptions & Open Questions

None.
