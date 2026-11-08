# Market Trends — Hospital Bill Check (Project RS, Phase 1)

This report covers the external trends that bear on the Phase 1 Hospital Bill Check described in the approved intent statement (`intent-statement.md`, in `ideation/intent-capture/`). It covers the two areas you chose: price regulation, and insurance rules as context for Phase 2 [Q9].

## Sources

- [Q1]–[Q11]: answers in `market-research-questions.md` in this folder.
- PRD §n: `PRDs/app-baseline.md`, v0.14 (PRD-v2), 4 October 2026, including its sources list (Appendix C).
- Web sources, all accessed 27 October 2026, are linked where they are used and listed again at the end.
- **[VALIDATE]** means the claim comes from a secondary source (news, law firm or compliance summaries) and must be checked against the primary notification or order before anyone relies on it.

## 1. Summary

| Trend | Direction | What it means for the MVP | Confidence |
|---|---|---|---|
| Itemised hospital bills are being standardised: a national bill-format standard (voluntary) and Karnataka's requirement from 1 Nov 2026 | Rising | **Opportunity.** More itemised bills means more lines the audit can check. It also gives users a public rule to cite when they ask for one. | Medium **[VALIDATE]** |
| NPPA ceiling prices on drugs and devices: stent prices revised for 2026; the knee-implant cap extended only to November 2026 | Stable for stents; uncertain for knee implants | Ceiling-price checks stay objective, but the reference data must follow each notification closely | Medium **[VALIDATE]** |
| Hospital rate ranges under the Clinical Establishments Rules: the Centre defends them, but most States have not set them | Slow | No legal benchmark for private patients yet, so price outliers stay a question, never an accusation (PRD §6.3) | Medium **[VALIDATE]** |
| Insurance rules (IRDAI non-payable lists, May 2024 Master Circular) | Stable | Context for Phase 2. In Phase 1 the IRDAI lists are an industry reference phrased as questions (PRD D16) | Per PRD; not refreshed |

## 2. Price regulation and billing transparency

### 2.1 Standard format for hospital bills (new since the PRD)

The PRD does not mention this trend. It came up while re-checking the direct competitors [Q1]. It is reported here because it bears on price and billing transparency, the area you chose [Q9].

- **The national standard.** The Bureau of Indian Standards issued IS 19493:2025, "Format of Hospital Bills". A legal summary gives the gazette notification date as 10 December 2025 and the effective date as 8 December 2025 ([Advocate Gandhi](https://advocategandhi.com/new-bis-standards-introduced-for-financial-services-and-healthcare-billing-december-2025-notification/)) **[VALIDATE]**. It sets out a uniform bill format with charges itemised under heads such as room, consultation, procedures, investigations and consumables, rather than lump sums.
- **Voluntary nationally.** Reporting in October 2026 says the format is still voluntary across India, so each hospital decides whether to follow it ([Outlook Money](https://www.outlookmoney.com/news/hospital-bills-still-lack-itemised-charges-83-per-cent-want-standard-made-mandatory)) **[VALIDATE]**. Some outlets describe it as a mandate; those claims conflict and should be checked against the BIS notification.
- **Karnataka.** Itemised billing becomes compulsory in Karnataka from 1 November 2026 for hospitals and other covered facilities, including outpatient services ([Outlook Money](https://www.outlookmoney.com/news/hospital-bills-still-lack-itemised-charges-83-per-cent-want-standard-made-mandatory)) **[VALIDATE]**.
- **Today's reality.** In a LocalCircles survey of 21,485 respondents whose family member had been hospitalised in the past 12 months:
  - 28% said the bill listed every charge, and another 28% said most charges were itemised;
  - 83% called it "very critical" that the government makes the standard mandatory.

  This is self-reported and directional only ([Outlook Money](https://www.outlookmoney.com/news/hospital-bills-still-lack-itemised-charges-83-per-cent-want-standard-made-mandatory)).

**What it means for the MVP**

- **Opportunity.** Getting an itemised bill is itself a hurdle (PRD §6.5). A published national format, and a State rule in Karnataka, give users something concrete to point to when the product tells them to ask for the itemised bill (PRD DU-08).
- **Opportunity.** Bills that follow a standard format are more uniform, which should make them easier to read line by line.
- **Long-term threat.** If itemised, readable bills become normal, part of the "I can't read my bill" pain goes away. The value of the product then rests more on objective findings and evidence than on simply explaining the bill (see `competitive-analysis.md`, section 3).

### 2.2 NPPA ceiling prices on drugs and devices

- **Coronary stents.** NPPA revised the ceiling prices by a notification dated 25 March 2026, effective 1 April 2026: ₹10,762.15 per unit for bare-metal stents and ₹39,186.03 per unit for drug-eluting stents, excluding GST. The rise follows the wholesale price index ([Medical Dialogues](https://medicaldialogues.in/news/industry/medical-devices/nppa-revises-coronary-stent-prices-des-at-rs-39186-and-bms-at-rs-10762-from-april-2026-168192); [TeamLease RegTech](https://teamleaseregtech.com/updates/article/54962/nppa-notified-regarding-the-revision-of-ceiling-prices-for-coronary-stents/)) **[VALIDATE]**.
- **Knee implants.** Reporting says NPPA extended the knee-implant price cap by one year, to November 2026, despite industry requests to remove or relax it ([Business Standard, NPPA topic page](https://www.business-standard.com/topic/nppa)) **[VALIDATE]**. No later notification was found. The cap may lapse, be extended or be revised next month.
- **Drug ceiling prices.** These are set under DPCO 2013 for scheduled formulations (NLEM) (PRD §4.2, §19). This report did not refresh them.

**What it means for the MVP.** MRP and NPPA ceiling-price breaches are among the objective findings that set the MVP apart (`competitive-analysis.md`, section 3). They are only objective if the reference data matches the notification in force on the bill date. The PRD plans a monthly check of NPPA prices (PRD §13.10); the knee-implant decision due in November 2026 is the first test of that.

### 2.3 Hospital rate ranges and the Supreme Court

- **The rule.** Rule 9(ii) of the Clinical Establishments (Central Government) Rules, 2012 requires establishments to charge within a range of rates the Centre sets with the States.
- **The Centre's position.** In an affidavit reported in August 2026, the Centre defended the rule as a way to curb excessive pricing. It said the rule sets a range, not a single price, and that most States and Union Territories have still not set their ranges ([LiveLaw](https://www.livelaw.in/top-stories/centre-defends-clinical-establishment-rule-in-supreme-court-says-it-curbs-excessive-pricing-of-medical-services-545337); [Bar and Bench](https://www.barandbench.com/news/rule-mandating-uniform-healthcare-rates-constitutionally-valid-centre-tells-supreme-court)) **[VALIDATE]**.
- **Earlier history.** In February 2024 the Court warned it could enforce CGHS rates if rates were not standardised ([Business Today](https://www.businesstoday.in/amp/personal-finance/news/story/standardise-hospital-treatment-charges-or-cghs-rates-will-be-enforced-supreme-court-tells-centre-419298-2024-02-28)). In August 2025 it clarified that the 2012 Rules had not been stayed, as reported in the LiveLaw coverage above **[VALIDATE]**.
- **No final ruling found.** No final ruling or hearing outcome was found **[VALIDATE]**.
- **Opposition.** Private hospitals and the IMA oppose standard rates ([The South First](https://thesouthfirst.com/news/why-private-hospitals-ima-oppose-standarised-healthcare-rates/)).

**What it means for the MVP.** Until ranges are notified, there is still no legal price benchmark for private patients. The PRD's stance holds: CGHS and PM-JAY rates are context only, and a price outlier is a question to put to the hospital, never an accusation (PRD §6.3). If a State notifies its ranges, that State's bills gain a new objective check, so this is worth watching State by State.

## 3. Insurance rules (context for Phase 2)

Insurance checks are Phase 2 and outside this build's boundary (`intent-statement.md`) [Q9]. These points come from the PRD and were not refreshed.

- **IRDAI standard non-payable lists (2019–2020 guidelines).** Four lists of items give a public, structured rule set (PRD §4.2, §13.9). In Phase 1, Lists II–IV feed the unbundling checks as an industry reference for all patients, always phrased as a question (PRD §13.10, D16).
- **IRDAI Master Circular on Health Insurance (May 2024).** Cashless pre-authorisation within 1 hour and final discharge authorisation within 3 hours; the insurer bears extra hospital charges caused by delay (PRD §4.2) **[VALIDATE]**. This shortens the time a family has to check a bill at discharge, which is why usefulness before payment matters (PRD §6.2).
- **Proportionate deduction exclusions.** Proportionate deduction cannot be applied to pharmacy, implants and diagnostics (PRD §4.2) **[VALIDATE]**. This becomes a Phase 2 check.

## 4. Trends not covered in this report

You chose not to cover these [Q9]. They are listed so that later stages pick them up where they matter.

| Trend (PRD §4.2) | Where it should be handled |
|---|---|
| Data protection: DPDP Act 2023 and Rules (consent managers from 13 Nov 2026; main obligations from 13 May 2027) | Feasibility and compliance constraints, together with data residency (see `build-vs-buy.md`) |
| ABDM and the National Health Claims Exchange | Phase 2 and later |
| Falling cost of AI document reading | The bill-reading decision in Feasibility (see `build-vs-buy.md`) |
| How Indian families find and share help (search, WhatsApp groups, consumer-rights content) | Scope Definition and later go-to-market work (PRD §6.1) |

## 5. Market size: what is still needed

This stage keeps the PRD's approach and publishes no sizing figures [Q3]. The inputs the PRD still needs are:

| Layer | Input still needed | Source the PRD names |
|---|---|---|
| Total in-patient hospitalisations in private facilities (insured and self-pay) | Yearly event count | NSSO health survey, NHA data (PRD §4.3) |
| Self-pay hospitalisations: the primary Phase 1 segment, not yet sized (gap G-15) | A self-pay sizing layer | NSSO, NHA (PRD G-15) |
| Privately insured claims | Split by segment of the roughly 32.6 million health claims settled in FY25 **[VALIDATE]** | IRDAI annual report (PRD §4.3) |
| Users reached in the first one to two years | Funnel conversion assumptions | To be modelled after the pilot (PRD §4.3) |

## Assumptions & Open Questions

- Whether IS 19493:2025 is voluntary or mandatory, and its exact dates, must be checked against the BIS gazette notification **[VALIDATE]**.
- The details of Karnataka's itemised-billing requirement (which facilities, how it is enforced) need the State's own notification **[VALIDATE]**.
- What NPPA does when the knee-implant cap reaches November 2026 is not yet known.
- The self-pay market (G-15) remains unsized by your choice [Q3].

## Web sources (accessed 27 October 2026)

- Outlook Money, "Hospital bills still lack itemised charges; 83 per cent want standard made mandatory" (updated 17 Oct 2026): https://www.outlookmoney.com/news/hospital-bills-still-lack-itemised-charges-83-per-cent-want-standard-made-mandatory
- Advocate Gandhi, "New BIS standards … healthcare billing (December 2025 notification)": https://advocategandhi.com/new-bis-standards-introduced-for-financial-services-and-healthcare-billing-december-2025-notification/
- Medical Dialogues, NPPA revises coronary stent prices from April 2026: https://medicaldialogues.in/news/industry/medical-devices/nppa-revises-coronary-stent-prices-des-at-rs-39186-and-bms-at-rs-10762-from-april-2026-168192
- TeamLease RegTech, NPPA revision of ceiling prices for coronary stents: https://teamleaseregtech.com/updates/article/54962/nppa-notified-regarding-the-revision-of-ceiling-prices-for-coronary-stents/
- Business Standard, NPPA topic page (knee-implant cap extension): https://www.business-standard.com/topic/nppa
- LiveLaw, Centre defends Clinical Establishment rule in Supreme Court: https://www.livelaw.in/top-stories/centre-defends-clinical-establishment-rule-in-supreme-court-says-it-curbs-excessive-pricing-of-medical-services-545337
- Bar and Bench, uniform healthcare rates rule valid, Centre tells Supreme Court: https://www.barandbench.com/news/rule-mandating-uniform-healthcare-rates-constitutionally-valid-centre-tells-supreme-court
- Business Today (Feb 2024), standardise charges or CGHS rates will be enforced: https://www.businesstoday.in/amp/personal-finance/news/story/standardise-hospital-treatment-charges-or-cghs-rates-will-be-enforced-supreme-court-tells-centre-419298-2024-02-28
- The South First, why private hospitals and the IMA oppose standard rates: https://thesouthfirst.com/news/why-private-hospitals-ima-oppose-standarised-healthcare-rates/
