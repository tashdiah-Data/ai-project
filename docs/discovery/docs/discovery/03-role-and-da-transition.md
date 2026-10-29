# Dhee's role, and which DA responsibilities transition: Internship & Career Readiness ("Pip")

**Story:** `internship.RESEARCH-3-ROLE-AND-DA-TRANSITION` · **Domain:** Internship & Career Readiness · **Accountable human:** Dhee
**Author:** Tashima Diah (draft prepared with Claude) · **Written:** 2026-10-29
**Evidence base:** platform repo `ColaberryIntern/ColaberryEnterprise_AI_LeadershipAccelerator`, searched read-only at commit `bdcd032` (2026-10-29, 12,963 files), and the domain inventory `backend/src/services/agentDomain/capabilities/internship.json` (researched 2026-10-09). Paths below are relative to `backend/src/` unless they start with `docs/` or `.claude/`. Facts about Pip's agent record come from `docs/discovery/02-agent-anatomy.md`, which used commit `825dfe3`.
**Status:** Draft. Nothing here is validated by Dhee.

## Headline: no source lists Data Analyst duties, so 1 responsibility can be placed and the rest are unknown

| Status | Count | Responsibilities |
| --- | --- | --- |
| Splits (sourced, not validated) | 1 | Recurring cohort and funnel analysis |
| Unknown | all others | Every other Data Analyst responsibility, because no source exists |

How I counted: a responsibility is **sourced** only when a file in the platform repo or the inventory names it. I did not infer any duty from the job title. This count is my judgment and Dhee can reclassify any row.

**The finding behind the number.** No document in the platform repo lists human Data Analyst duties for this domain or any other. The one internship item that touches DA work is a proposal, `internship.da_cohort_analysis_automation`, whose state is `absent`, whose inputs read "unknown: which DA deliverables are in scope", and whose supporting evidence is empty (`services/agentDomain/capabilities/internship.json`). The "Data Analyst Agent" in the repo is software, not a person's job.

**Second finding.** The inventory says transitioning analyst work "must not transition the judgement about what the numbers mean." That is the only line anyone has drawn between a DA duty that could move to the AI and one that must stay human, and it is the basis for the single split below.

---

## 1. What was searched, and what was found

**Status: gap proved.**

**What was searched**

| Source | Query | Result |
| --- | --- | --- |
| Platform repo, commit `bdcd032`, cloned read-only | File names matching: data analyst, job description, role, responsibilit, duties, RACI, org chart | No document lists human Data Analyst duties. |
| Same clone | File contents matching: "data analyst", "analyst duties", "analyst responsibilities", and "DA" used as a role | Same result. Matches are listed below. |
| `internship.json` (35 capabilities) | `data analyst`, `DA`, `analyst`, `cohort analysis`, `funnel analysis`, case-insensitive | One capability matches: `internship.da_cohort_analysis_automation`. |
| `.claude/skills/loop-architect/references/execution-contract.md` (cited by that capability at line 47) | Read at the cited line | A blank template. It does not hold the owner scope amendment's wording. |
| The story brief | Not available to me | Unknown. The brief's figures and blockers do not appear in this document. |

The repo search was run in a separate session, and this table records its reported results.

**What matched, and why none of it is a DA duty list**

| File | What it is | Lists human DA duties? |
| --- | --- | --- |
| `docs/agent-catalog/assistant/data-analyst-agent.md` | Code module "Data Analyst Agent" that cleans SQL results and adds business labels | No. It is software, not a person's job. |
| `docs/training-program-2026-q3/TEAM_LEAD_JOB_DESCRIPTIONS.md` | The only job descriptions in the repo: Website Lead, Marketing Lead, AI Contract Developer, Part-Time SDR | No. There is no analyst role. |
| `docs/architecture/ai-workforce-management/employees/*/ROLE_CHARTER_*.md` | Charters for the curriculum and learner-success AI employees | No. None mentions "analyst". |
| `services/agentDomain/capabilities/internship.json`, `internship.da_cohort_analysis_automation` | The internship DA capability | `implementation_state: absent`, inputs "unknown: which DA deliverables are in scope", `supporting_evidence: []` |
| `curriculum.json`, `learner_success.json`, `executive.json`, `sales.json` (same folder) | Other domains' DA capabilities | Each states that no DA duties document exists. `executive.json` says the Data Analyst Agent is not a human role. |
| `docs/architect-mindset/02-curriculum-specification.md` and similar | Mentions "data analyst" as a stakeholder | No duties listed. |

**First honest version**
State the gap as found: the source material does not exist. Section 5 says what would have to be written.

## 2. Dhee's role

**Status: partly evidenced.**

**What exists today**

| Claim | Source |
| --- | --- |
| Dhee is the accountable human for this domain. | `services/agentDomain/capabilities/internship.json`, `accountable_human` |
| Dhee is named as an intended user on all 35 capabilities. | `internship.json`, `intended_users` (counted 2026-10-29) |
| All 35 capabilities are `draft`, with the dependency "Dhee must confirm scope, exceptions and success measures". | `internship.json`, `requirement_validation_state` and `validation_dependency` |
| Dhee described Pip's job as onboarding plus open house advertising and reminders. The inventory has no advertising or open house capability. | `docs/discovery/02-agent-anatomy.md`, second finding |
| Dhee has not named which analyst work should transition. | `internship.json`, `internship.da_cohort_analysis_automation`, `blocked_by` |

**What is missing**
- Dhee's job title, reporting line and day-to-day duties. No source I read describes them.
- Who the "data analysts" listed as intended users of the DA capability are. Ali is also listed, and the file does not say in what role.

**First honest version**
Describe Dhee's role only as far as the sources go: accountable human, intended user, and the person whose validation every capability waits on. Anything more is for Dhee to write.

## 3. Responsibilities: stays human, transitions to AI, or splits

**Status: one row sourced, the rest unknown.**

| Responsibility | Decision | Reason | Source |
| --- | --- | --- | --- |
| Recurring cohort and funnel analysis | **Splits** | The inventory proposes that the AI produces the analysis a data analyst assembles by hand, with sources cited. It also says the AI must not take over the judgement about what the numbers mean. | `internship.da_cohort_analysis_automation`: `business_outcome`, `exceptions` |
| Every other Data Analyst responsibility | **Unknown** | No source lists them. The inventory says naming one "would be an invention". | Same capability, `exceptions`; search in section 1 |

**Where the line falls for the split**

| Side | What it covers | Who holds it |
| --- | --- | --- |
| Transitions to AI (proposed, not validated) | Producing the recurring cohort and funnel analysis. Every number cites its source query or dataset. A metric of unknown reliability is reported as unknown, not computed anyway. | Unknown. The inventory names no deliverable, owner or measure. |
| Stays human | Judgement about what the numbers mean | Unknown. The inventory lists Dhee, Ali and data analysts as intended users but does not say who holds the judgement. Dhee is the only accountable human named. |

**Evidence that would show the AI is ready to hold the transitioning part**

| Evidence | Source |
| --- | --- |
| Dhee names at least one specific deliverable, and the inventory lists only artifacts that were observed, with no invented reports. This is the file's own condition for the first step (R0), and it is not met. | `internship.da_cohort_analysis_automation`, `release_behaviour` |
| Any later evidence, such as agreement with analyst output or a time-saved measure | Unknown. None is defined, because no deliverable exists to measure. |

The highest autonomy the file proposes for this capability is R2 (answering questions). Its rationale: there is "nothing to validate and nothing to measure."

**First honest version**
The AI may help produce a cited cohort analysis once Dhee names the deliverable. It never states what the numbers mean. Until a deliverable is named, nothing transitions.

## 4. Responsibilities that must stay human

**Status: evidenced.**

The inventory marks ten capabilities as human decisions (`human_decision: true`). These are not DA duties. They are acts in this domain reserved to a person, so any transition has to stay outside them. Reasons are from the inventory's `max_autonomy_rationale` and `exceptions` on each capability.

| Reserved act | Reason it is reserved | Holder named in the file |
| --- | --- | --- |
| Recording an admission decision | Internship acceptance is always a human decision, enforced by actor and not by convention. | Reviewer |
| Verifying signed documents | It is the gate between a submitted file and a granted entitlement. No automated actor exists in the state machine. | Human reviewer |
| Activation and curriculum entitlement grant | It grants paid training to a named person and is the most expensive act in the domain. | Not stated |
| Moving a person between class and internship | A wrong move shows a student a coherent but wrong portal with nothing logged. | Not stated |
| Publishing generated build projects | Publication puts a plan in front of a named intern. | Reviewer |
| Inactivity exit | Removal is irreversible in practice, and the repo documents false positives on named interns. | A person who runs the confirmation step |
| Publishing a portfolio | Publication makes a named person's work public. | Career reviewer |
| Career support handoff | A handoff transfers accountability for a person. | A receiving human, not yet named by Dhee |
| Committing an existing-intern conversion | It changes the lifecycle state of real people in bulk. | A human, after reading the plan |
| Verifying the manager can reach the agent surface | Granting or changing a role is an authorization change that no agent may make. | Not stated |

## 5. The document that would have to be written

**Status: does not exist.**

No source for Data Analyst duties in this domain exists. To fill the gap, a document called "Data Analyst responsibilities for Internship & Career Readiness" needs to be written. Outline:

1. The recurring analyses, reports or dashboards a data analyst produces for this domain today. Each is named and observed, with a sample, because the inventory lists only artifacts that were seen.
2. For each: who requests it, how often, and from what data.
3. For each: the decision it supports, and who makes that decision.
4. Which of these Dhee proposes to move to the AI first, and the measure that would show it works.
5. What stays human, and the reason.

**Who writes it:** Dhee, as the accountable human, with the data analysts the inventory lists as intended users. I do not know who those analysts are.

---

## Open decisions for Dhee (these block moving anything out of draft)

1. Which specific Data Analyst deliverable, if any, should move to the AI first? The inventory needs at least one named deliverable.
2. Who holds the judgement about what the numbers mean?
3. Should `internship.da_cohort_analysis_automation` stay in Pip's scope at all, given that no deliverable is identified?
4. Who are the data analysts the inventory lists as intended users, and what is Ali's role in this?
5. Will Dhee write the Data Analyst duties document in section 5, or name someone who will?

## Done means: where each line is answered

| Done means | Where |
| --- | --- |
| The document names the accountable human and the specific responsibilities proposed to transition to the agent. | Section 2 names Dhee. Section 3 lists the one proposed responsibility, cohort and funnel analysis. |
| Each transitioning responsibility states what evidence would show the agent ready to hold it. | Section 3, "Evidence that would show the AI is ready". |
| Responsibilities that must stay human are listed separately, each with the reason. | Section 4. |

## Sources

All repo files below were found in the platform repo at commit `bdcd032`, searched read-only on 2026-10-29. The search was run in a separate session. Line numbers are those that session or `02-agent-anatomy.md` reported, and I did not re-check them.

- `services/agentDomain/capabilities/internship.json` (35 capabilities; `accountable_human`, `intended_users`, `requirement_validation_state`, `validation_dependency`, `human_decision`, `max_autonomy_rationale`, `exceptions`, `blocked_by`, `release_behaviour`). Capability: `internship.da_cohort_analysis_automation`
- `services/agentDomain/capabilities/curriculum.json`, `learner_success.json`, `executive.json`, `sales.json`
- `.claude/skills/loop-architect/references/execution-contract.md` (line 47 cited by the inventory)
- `docs/agent-catalog/assistant/data-analyst-agent.md`
- `docs/training-program-2026-q3/TEAM_LEAD_JOB_DESCRIPTIONS.md`
- `docs/architecture/ai-workforce-management/employees/*/ROLE_CHARTER_*.md`
- `docs/architect-mindset/02-curriculum-specification.md`
- `docs/discovery/02-agent-anatomy.md` (in `tashdiah-Data/ai-project`)
