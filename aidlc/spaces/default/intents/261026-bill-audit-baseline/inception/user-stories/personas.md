# Personas — Hospital Bill Check (Project RS, Phase 1)

These personas are the actors in `stories.md`. The four customer personas come from the PRD's primary personas (`PRDs/app-baseline.md` §9.1, read through the document library, id 01a17987) and the target customers in `ideation/intent-capture/intent-statement.md`. The internal persona covers the two people who run the product's internal tools [Q1].

## Sources

- [Q1]: four customer personas plus one internal persona, in `user-stories-questions.md` in this folder.
- Designer contribution (`contributions/aidlc-design-agent.md`): the gentle wording for C4 and the large-text needs carried by C2 for Kamala.
- PRD §9.1 primary personas, §9.2 supported persona, §9.3 key stakeholder, §9.5 persona priority.
- `ideation/intent-capture/intent-statement.md`, Target Customer.
- `inception/requirements-analysis/requirements.md` (FR1–FR16, NFR1–NFR21).
- Team plan: one builder plus the founder (`ideation/feasibility/constraint-register.md`, CR-O1).

## Customer personas

### C1 — Priya, family member at discharge (primary)

| Aspect | Detail |
|---|---|
| Role | Handles the bill for her husband, admitted as an emergency for a cardiac procedure (PRD Persona 2) |
| Context | At the billing desk or the ward, on a mid-range Android phone, often on patchy hospital Wi-Fi or 4G, with little time before discharge |
| Goals | Get through discharge without being overcharged; avoid confrontation; protect the family's savings |
| Pain points | Does not understand medical terms; is pressured to pay quickly; fears that questioning the bill will affect care |
| Needs from this build | No sign-up; step-by-step guidance in simple English; two or three polite things to ask; one page to show the billing desk |
| Tech comfort | Medium |
| Frequency | Once per hospitalisation |

### C2 — Meera, remote family manager (primary)

| Aspect | Detail |
|---|---|
| Role | Manages her father's hospital bill from Singapore; her parents live in Lucknow (PRD Persona 3) |
| Context | Works from a laptop across time zones; relatives send photos of bills on WhatsApp, often blurry |
| Goals | Make sure her parents are not overcharged without travelling; act quickly |
| Pain points | Unreadable or missing pages; cannot call during Indian business hours; parents cannot argue with the billing desk |
| Needs from this build | Run the check on desktop from forwarded photos; clear notes on missing or unreadable pages, including lines in Hindi; a crisp PDF to forward |
| Tech comfort | High |
| Frequency | Once per hospitalisation; may run checks for several relatives |

Meera also stands in for the supported senior-citizen persona (PRD Persona 6, "Kamala"): a son or daughter runs the check for the parent and walks them through the PDF. That is why large text and screen-reader support sit on Meera's and Priya's stories.

### C3 — Rohit, patient paying upfront (primary)

| Aspect | Detail |
|---|---|
| Role | Pays the hospital directly: either upfront with a reimbursement claim to make later (PRD Persona 4, "Rohit") or entirely from savings with no insurer (PRD Persona 1, "Ramesh") [Q1] |
| Context | A planned procedure; a long itemised bill with internal codes; a package quote or estimate; implants or large consumable lines |
| Goals | Pay only what is fair; check the package and implant prices; for a later claim, have a clean, correct bill |
| Pain points | 40-page bills; ₹22,000 under "consumables" with no detail; the final bill above the estimate; no time to read before paying |
| Needs from this build | Line-by-line explanation; objective errors flagged before payment; letters asking for the itemised bill, the implant invoice and the package inclusions |
| Tech comfort | High |
| Frequency | Once per hospitalisation |

Reimbursement help (deadlines, documents, deductions) comes with the Insurance Check in Phase 2 and is not in this build.

### C4 — Anil, bereaved family (primary)

| Aspect | Detail |
|---|---|
| Role | Settles the ICU bill of his mother, who died after 12 days in hospital (PRD Persona 5) |
| Context | Grieving and arranging the funeral; a ₹6.8 lakh final bill with ICU, ventilator and drug charges |
| Goals | Settle the bill fairly without being exploited; get closure |
| Pain points | Pressure to clear dues; charges he cannot judge; guilt about questioning the bill at all |
| Needs from this build | A gentle, respectful tone ("has passed away", never "deceased" or "legal heir"; no wording about the patient's future care, US1.7); objective errors first; no aggressive language; reassurance that issues can be raised after payment; consent by declaration for a deceased patient; a clear way to stop at any point |
| Tech comfort | Medium |
| Frequency | Once |

## Internal persona

### I1 — The builder and the founder (internal operators)

| Aspect | Detail |
|---|---|
| Role | The only two people who run the internal tools in Phase 1: rule owners, test-set labellers and operators (FR14.6, CR-O1) |
| Context | No domain experts on the team; real bills are labelled by both people independently (FR16.5); real bills must stay out of the repository and out of the AI coding assistant's reach (FR16.13) |
| Goals | Only accurate checks reach families; rules and reference prices can be corrected without a release; contributors' bills are handled with consent and can be withdrawn |
| Pain points | Small test-set counts for some checks; a single person must be able to run promotion, labelling and incident response |
| Needs from this build | A rule library with approval before changes take effect; switching each check between live, shadow and off; promotion to live only with a recorded test pass; a quality dashboard; access logging |
| Tech comfort | High |
| Frequency | Weekly, and before every check promotion or rule change |

## Key stakeholder (not a user)

### S1 — Sunita, hospital billing desk executive

Sunita never uses the product (PRD §9.3). She reads what the family shows her, and her reaction decides whether a finding becomes a corrected bill. She acts on specific line and page references, objective errors listed first, and neutral, courteous wording. She dismisses accusations, vague claims and long documents. Her needs shape the Billing Desk Summary and letter stories (US5.3, US5.6, US5.10). She is not the actor in any story.

## Persona priority and relationships

| Priority | Persona | Why |
|---|---|---|
| 1 | C1 Priya | The hardest conditions (phone, time pressure, stress, network); if the journey works for Priya it works for most |
| 2 | C2 Meera | Desktop use, forwarded photos and unreadable pages; forwards the PDF to the family |
| 3 | C3 Rohit | The deepest use of checks, packages, implants and letters |
| 4 | C4 Anil | Sets the tone and stop-at-any-point requirements for everyone |
| — | I1 Builder and founder | Runs the tools that keep findings accurate; not part of the family journey |

C2 and C1 often work on the same hospitalisation: Meera runs the check on desktop, and Priya (or a parent) shows the Billing Desk Summary at the counter. There is no shared case access (PRD D7): they share the downloaded PDF, not a session.

Cashless insured patients are served but not targeted (`intent-statement.md`); their insurance deductions are a Phase 2 concern.
