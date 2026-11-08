# Initiative Brief — Hospital Bill Check (Project RS, Phase 1)

**Recommendation: Go, with conditions.** Proceed to detailed planning (Inception), with feasibility conditions F1–F5 tracked as gates on the build and the launch [Q8].

This brief gathers the ideation work into one page for the founder and for anyone joining the build. All decisions behind it are in `decision-log.md` in this folder.

## Sources

- [desc] Initial description: "build as per the requirements in @PRDs/app-baseline.md"
- [scope] Workflow-selected scope: `enterprise`, stopping before the operation phase (`intent-capture-questions.md`, Q13).
- [Q1]–[Q8]: answers in `approval-handoff-questions.md` in this folder.
- Ideation work, all under `ideation/`:
  - `intent-capture/intent-statement.md`, `intent-capture/stakeholder-map.md`;
  - `market-research/competitive-analysis.md`;
  - `feasibility/feasibility-assessment.md`, `feasibility/constraint-register.md`, `feasibility/raid-log.md`;
  - `scope-definition/scope-document.md`, `scope-definition/intent-backlog.md`;
  - `rough-mockups/wireframes.md`, `rough-mockups/user-flow.md`, and the design check on them (`rough-mockups/reviews/review-01.md`).
- There is no team assessment: team formation was skipped, because one builder does the work (`feasibility-questions.md`, Q1).

## 1. The problem and who it is for

Patients and their families cannot tell whether a hospital bill is correct and justified. They lack the knowledge, the time and the evidence to question it with the hospital (`intent-statement.md`).

**Who it is for.** The primary customers are:

- self-paying patients and their families;
- the family member handling the bill at discharge;
- a remote family manager working from forwarded photos;
- patients who pay upfront and claim reimbursement later;
- bereaved families settling a bill.

The hospital billing desk is not a user. It is the key stakeholder: it decides whether a finding becomes a corrected bill (`intent-statement.md`).

**What the product is.** A free, anonymous, single-session web check of Indian hospital bills. It reads the bill line by line and shows what may be worth asking the hospital about, with the evidence. It ends with a polite one-page Billing Desk Summary and ready-to-use letters (`scope-document.md`).

**How success is measured.** With the PRD's Phase 1 metric tree, treated as starting hypotheses. The headline measure is the share of completed audits with at least one "Likely error" or "Worth asking" finding. Guardrails include zero accusatory findings and zero stored records older than 3 months (`intent-statement.md`).

## 2. Market validation

- **The direct field is thin.** One live free checker (eVaakil) and one hackathon concept (CleanBill) were found on 27 October 2026 (`competitive-analysis.md`).
- **Five differentiators:**
  1. every line is read, not just category totals;
  2. objective findings a billing desk cannot easily dismiss, such as arithmetic errors, duplicates and prices above MRP;
  3. evidence for every finding;
  4. follow-through, with the Billing Desk Summary, letters and an escalation ladder;
  5. usefulness before payment.
- **A gap to watch.** The free checker shows the effect of the insurance room-rent cap, which Phase 1 does not.
- **The market is easy to enter and free to leave,** so the product must win on quality and trust.
- **Enough to proceed** [Q5]. Facts marked for checking against a primary source stay open, and customer expectations are untested hypotheses.

## 3. Feasibility and risk highlights

**Feasible with conditions** (`feasibility-assessment.md`). Two things are unproven:

- **Reading bills accurately enough.** The targets are at least 95% line-item accuracy on digital PDFs and at least 90% on good photos. No option has been tested yet.
- **One builder carrying the whole Phase 1 boundary.** This is a risk to pace and quality. There is no deadline (`intent-statement.md`).

The main risks are accepted with the responses already planned [Q3] (`raid-log.md`):

| Risk | Planned response |
|---|---|
| Bill reading misses the targets on real bills, or a synthetic-bill trial overstates accuracy (R1, R2) | Trial on synthetic bills, then repeat on real bills before deciding; users confirm unclear lines; checks below target stay hidden |
| One builder carries everything (R3) | The workflow's built-in reviews and test gates; outside security testing; the weekly founder update |
| False findings or a domain-knowledge gap (PRD RK2, RK27) | Launch with the most objective checks only; each check goes live only after it passes the Labelled Test Set; neutral wording |
| No legal review before launch (PRD RK28) | Accepted by the founder; conservative wording |
| Pay-under-protest guidance moved to Phase 1.5 leaves the patient-welfare risk partly open (PRD RK15) | The rule that nothing may encourage withholding payment in a way that blocks discharge or treatment applies to all wording (C10) |

**Fixed boundaries** (`constraint-register.md`):

- DPDP Act standards from day one;
- no accounts and a single session;
- original files deleted after reading;
- de-identified data kept for 3 months, and only with consent;
- English only;
- no legal advice, no clinical opinion and no accusations.

**Running cost.** No budget is set [Q4]. The bill-reading trial records cost per audit, and a ceiling is proposed to the founder once hosting is chosen.

## 4. Scope boundary

| In this build | Out of this build |
|---|---|
| Every Must Have and Should Have feature in the PRD, except urgent-situation guidance | Could Have items (a later build) |
| The 18 most objective checks at launch; the 8 document checks as guidance; the rest going live one by one as each passes | Urgent-situation and pay-under-protest guidance (Phase 1.5) |
| Internal tools in their leanest form: the rule library, the Labelled Test Set and labelling, and ops/admin, all behind their own strong sign-in | Everything on the PRD's Won't Have list, including insurance checks, accounts, Hindi and regional languages, and a native app |
| Mobile web and desktop web | Phase 1.5 and every later phase |

Source: `scope-document.md`.

**Order of work** (`scope-document.md`, `intent-backlog.md`):

1. Start the bill-reading trial.
2. In parallel, build the features that do not depend on which bill reader wins: consent and upload, the checks, the report, and the letters.
3. Connect the chosen bill reader.
4. Build the remaining features, then the internal tools.
5. Launch when every Must Have is in and the 18 launch checks pass.

## 5. Concept visuals

The rough sketches are in `wireframes.md`, with the flow in `user-flow.md` (both in `ideation/rough-mockups/`). There are 11 screens in one session: start, privacy and consent, the moment of checking, upload with a checklist, a quick summary, confirming unclear lines, the bill check with its breakdown and findings, finding detail, the Billing Desk Summary, next steps and letters, and "before you go".

**Changes carried forward.** A design check raised eight points. A change was agreed for each, and the sketches were approved without them, so all eight go forward into Requirements Analysis and Refined Mockups [Q1]:

| ID | Agreed change | Matters before launch |
|---|---|---|
| R-01 | Add "Add a document" and "Fix a line" after the report appears, with the report refreshing; show the return path in the flow (PRD DU-11, RP-06, both Must Have) | Yes |
| R-02 | Keep the current wording, but the privacy wording on the start screen ("Nothing about you is saved") and on "before you go" ("anonymous") needs a founder or counsel check before launch. Optional 3-month retention means data is de-identified, not anonymous | Yes |
| R-03 | Mark the quick summary "First look — may change after you confirm a few details"; rename its button "Confirm a few details" | No |
| R-04 | One rule for interim bills. In hospital: the current interim bill only, and nothing carries over. At discharge: the checklist asks for earlier interim bills with the final bill | No |
| R-05 | "Share" becomes "Save or share". It makes a PDF or image on the device and opens the phone's own share menu, with nothing sent through our servers; desktop offers a download | No |
| R-06 | Add three edge states: a file that is not a bill, a check that fails partway, and declined consent | No |
| R-07 | Keep desktop as written notes for now and draw it in Refined Mockups; use the ₹ sign | No |
| R-08 | Explain "live / shadow / off", "precision targets" and "Labelled Test Set" in plain words; write "PRD scope item S8" | No |

## 6. Team plan

Team formation was skipped. The plan is confirmed [Q6]:

- **You:** build alone with this AI-assisted workflow, and label real bills.
- **The founder:** makes the decisions, receives a weekly status update, and is the second, independent labeller of real bills (`stakeholder-map.md`).
- **No domain experts**, and no legal review before launch.

## 7. Go/no-go

**Go** [Q8]. The founder has already agreed to the intent and scope [Q2].

**Conditions tracked as gates** (`feasibility-assessment.md`, section 7):

| # | Condition | Needed before |
|---|---|---|
| F1 | The bill-reading trial runs on synthetic bills and then real bills, and at least one option meets the accuracy, speed and pass/fail targets | Building anything that depends on bill reading |
| F2 | The founder decides data residency: India only, or India first | The build starts [Q7] |
| F3 | Hosting is confirmed against that residency decision, including where stored data and logs live | The build starts [Q7] |
| F4 | Consented real bills start coming in, so the Labelled Test Set can grow beyond synthetic bills | Any check goes live |
| F5 | An incident plan and outside security testing are arranged | Launch |

**Founder decisions due before the build starts** [Q7]:

- data residency;
- hosting;
- how to measure whether billing desks actually correct bills (PRD G-02). Until now this was open for the founder with no date (`intent-statement.md`).

## Assumptions & Open Questions

- Facts marked [VALIDATE] in the market and feasibility work remain unchecked against primary sources [Q5].
- Customer expectations are hypotheses; none has been tested with customers (`competitive-analysis.md`).
- The session-timeout value (PRD G-10) and the threshold for collapsing low-confidence findings are set in Requirements Analysis (`wireframes.md`).
- The minimum number of test examples per check (PRD G-16) is set in Requirements Analysis (`scope-document.md`).
- Whether a public source of typical quantities exists is not yet known (`scope-document.md`).
- Whether a cloud account exists for the product is not known (`feasibility-assessment.md`).
