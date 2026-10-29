# Agent anatomy: Internship & Career Readiness ("Pip")

**Story:** `internship.RESEARCH-2-AGENT-ANATOMY` · **Domain:** Internship & Career Readiness · **Accountable human:** Dhee
**Author:** Tashima Diah (draft prepared with Claude) · **Written:** 2026-10-29
**Evidence base:** platform repo `ColaberryIntern/ColaberryEnterprise_AI_LeadershipAccelerator` at commit `825dfe3` (2026-10-29), and the domain inventory `backend/src/services/agentDomain/capabilities/internship.json` (researched 2026-10-09). All paths below are relative to `backend/src/` unless they start with `docs/`.

## Headline: 2 of 9 elements are unknown

| Status | Count | Elements |
| --- | --- | --- |
| Unknown | 2 | Memory, Coordination |
| Partly evidenced | 6 | Reasoning, Inputs, Communication, Tools, Procedures, Verification |
| Evidenced, with named gaps | 1 | Safeguards |

How I counted: an element is **unknown** when nothing in the repo answers what Pip's version of it should be. It is **partly evidenced** when the application already does a real version of it but no agent does, or when a decision that only Dhee can make is still open. This count is my judgment and Dhee can reclassify any row.

**The finding behind the number.** No agent exists for this domain: its registry entry has `agent_id: null` and `requested_capabilities: []` (`services/agentDomain/agentDomainRegistry.ts:197-207`). Most of what this document describes is the internship application that Pip would operate, not Pip itself. The only evidence that Pip exists at all is its name (`services/internship/agentNudgeState.ts:76`).

**Second finding.** Dhee described Pip's job to me as onboarding plus open house advertising and reminders. The domain inventory has 35 capabilities and none mentions advertising or open houses (a search of `internship.json` finds zero matches). Open houses do exist on the platform as an event type (`models/CommunityEvent.ts:4`, `services/openHouseTypes.ts`), but outside this domain's inventory. Whether Pip's scope includes advertising is therefore an open decision for Dhee, not something the code settles.

**Status of the inventory itself.** All 35 capabilities are `requirement_validation_state: "draft"` with the dependency "Dhee must confirm scope, exceptions and success measures" (`services/agentDomain/capabilities/internship.json`, every capability). Of the 35, 23 are implemented, 7 partial, and 5 absent.

---

## 1. Reasoning: how Pip decides what to do

**Status: partly evidenced.**

**What exists today**
- The application already separates facts from judgment. Eight hard requirements are checked with no model at all (`services/internship/internshipRequirementChecks.ts:4-11`).
- A language model (`gpt-4o-mini`, `services/internship/internshipApplicantAssessment.ts:27`) writes only the summary and posture on top of those facts. Applicant answers go in fenced as untrusted evidence (`internshipApplicantAssessment.ts:108`).
- If the model is unavailable, a deterministic fallback returns the requirement tally and marks the result `model_generated: false` (`internshipApplicantAssessment.ts:156`).
- Progress percentage and projected completion date are computed deterministically on purpose, because "a model that rounds 46.5%" is not an acceptable failure (`scripts/lib/internDeliveryCoachNote.js:18`).
- Portfolio scoring is deterministic and pure, and is forbidden from calling anyone employable (`services/career/careerReadiness.ts:11-15`).

**What is missing**
- No agent loop exists: nothing decides which capability to run, when, or why. The domain has no agent record (`services/agentDomain/agentDomainRegistry.ts:203`).
- Which decisions Pip itself would make is unconfirmed by Dhee (all 35 capabilities are draft).

**First honest version**
Pip reasons by rules first and a model second, as the application already does. It reads measured state and produces a recommendation for Dhee, with every number computed in code and the model limited to wording. It never decides admission, which the state machine reserves to a human (see Safeguards).

## 2. Memory: what Pip remembers

**Status: unknown.**

**What exists today**
- The application keeps durable records: every state change is written with its actor in one transaction (`services/internship/internshipApplicationService.ts:79-95`, `models/InternshipStatusEvent.ts:33`), and decisions record who made them (`models/InternshipDecision.ts:47`).
- The platform has a generic mechanism for agent memory: a proposal that stays `pending` until a human approves or rejects it (`services/agentMemoryProposalService.ts:32`, `:74`). It is keyed to an agent record, so it cannot be used until Pip has one.

**What is missing**
- No agent record, so no memory for Pip.
- No manager check-in or 1:1 record model, so a follow-up cannot be tracked (inventory: `internship.manager_check_in_brief_and_follow_up`, `blocked_by`).
- Nothing says what Pip should remember about an intern between runs, or how long.

**First honest version**
Start with no memory. Pip recomputes from the application's own records each run. Anything it wants to retain goes through the existing pending-then-approved memory proposal, once Pip has an agent record.

## 3. Inputs: what Pip receives

**Status: partly evidenced.**

**What exists today**
- The inventory lists inputs per capability, such as enrollment id, cohort id, target state, and interview answers (`services/agentDomain/capabilities/internship.json`, `inputs` field of each capability).
- Intern delivery data comes from Basecamp through scheduled scripts outside the backend (`scripts/lib/reportingRegistry.js:108-146`).
- Interview preparation reads the view `vw_ColaberryInterviewPreparation_UpcomingInterviews` (`scripts/lib/interviewPrepData.js:7`).
- Program facts, such as weekly rolling starts and the paid subscription, are written in `docs/AI_INTERNSHIP_SPEC.md:4-12`.

**What is missing**
- No input for advertising or open house content in this domain (see the second finding above).
- No target skill profile for internship work, so skill-gap input cannot be produced (inventory: `internship.skill_gap_identification_for_interns`, `blocked_by`). The skill service exists (`services/skillGenomeService.ts:48`) but nothing under `services/internship/` imports it.
- Comp expiry has no storage location (inventory: `internship.comp_expiry_advance_warning`, `blocked_by`).

**First honest version**
Pip reads only the application's existing records and the Basecamp-derived delivery data, read-only. Inputs for advertising and open houses wait for Dhee to say whether they are in scope.

## 4. Communication: how Pip talks to people

**Status: partly evidenced.**

**What exists today**
- Managers can send an intern one message from the console, choosing only a template key. There is no parameter for a custom subject or body (`services/internship/internshipNudge.ts:7-14`, templates `quiet_check_in`, `weeks_1_3_reminder`, `project_start`).
- Every send checks the kill switch first and then goes through a ledger that blocks duplicates (`services/internship/internshipNudge.ts:136`, `:144`; `services/launchSafety.ts:124`).
- Decision emails are built from reason codes, not free text (`services/internship/internshipEmails.ts:11-28`).
- A nudge composer decides whether to say anything and what to say from measured state, never from caller text (`services/internship/agentNudgeComposer.ts:1-40`).

**What is missing**
- Live email is on the default-deny list for agent execution (`services/delivery/execution/executionPolicy.ts:37`), so Pip cannot send production email without a separately approved action class and a named approver.
- No scheduled internship send exists in the backend (searches of `services/aiOpsScheduler.ts` and `services/schedulerService.ts` find no internship job). Reminders today are manager-triggered.
- No channel for Pip to talk to Dhee other than the console and email.

**First honest version**
Pip drafts messages and puts them in front of Dhee, who sends them. It uses the existing fixed templates. No autonomous sending until the approved-action-class decision is made.

## 5. Tools: what Pip is allowed to use

**Status: partly evidenced.**

**What exists today**
- The inventory names 29 distinct tools the 35 capabilities would need, including `internship_admin_api_read`, `basecamp_read`, `ccpp_sql_read`, `kpi_aggregate_read`, `approved_template_notification_send`, `llm_text_generation`, and `sandbox_email_sink` (`services/agentDomain/capabilities/internship.json`, `required_tools`).
- Every capability starts at `autonomy:R0` and its ceiling is set per capability, not per agent (inventory: `starting_autonomy`, `max_permitted_autonomy`; ladder in `services/agentDomain/autonomyLadder.ts:2`).

**What is missing**
- No tool has been granted to Pip. `requested_capabilities` is empty (`services/agentDomain/agentDomainRegistry.ts:206`).
- None of the 29 tools is provisioned for an agent identity, and none of the eight default-denied capabilities appears in any required-tools list (inventory `notes`).

**First honest version**
Read-only tools only: `internship_admin_api_read`, `kpi_aggregate_read`, and `basecamp_read`, in a sandbox. Write and send tools wait for Dhee's approval of the capability that needs them.

## 6. Procedures: the steps Pip follows

**Status: partly evidenced.**

**What exists today**
- The admission process is a 22-state machine whose transitions are the procedure (`services/internship/internshipStateMachine.ts:33-63`, `:271`).
- Onboarding has a defined checklist (`services/internship/internshipOnboarding.ts:58`).
- Each capability in the inventory carries a written `workflow` and a list of `exceptions` (`services/agentDomain/capabilities/internship.json`).

**What is missing**
- These are the application's procedures, not Pip's. Nobody has written Pip's own runbook.
- Dhee has not validated the workflows, the check-in cadence, or what "required attendance" means (inventory `blocked_by` on several capabilities).

**First honest version**
Pip's first procedure is a single weekly watch routine: read the console, list interns and applicants needing attention, and give Dhee the list with the evidence. It begins from the two capabilities the inventory notes as strongest candidates, `career_support_handoff` and `manager_check_in_brief_and_follow_up`, if Dhee agrees. For the handoff, Pip only lists who may need one and why. The handoff itself is a human act (`human_decision: true`, capped at R2), and Dhee has not yet named the receiving person or the trigger.

## 7. Coordination: how Pip works with other agents and people

**Status: unknown.**

**What exists today**
- Nothing for agent-to-agent handoff. The platform's own research brief states it does not exist (`services/agentDomain/agentDomainStoryTemplates.ts:160`), and the autonomy ladder places it at R8, "coordinate necessary, authorized handoffs with other agents" (`services/agentDomain/autonomyLadder.ts:67`), which no internship capability reaches (maximum is R7, inventory `max_permitted_autonomy`).
- Agent-to-human handoff exists elsewhere on the platform as ranked queue rows (`services/growthJourney/handoffs/handoffService.ts:1-30`). It belongs to the growth journey, not to this domain.
- `services/internship/internshipWeek3Handoff.ts` is an internal project handoff at week 3 and is not a career handoff (inventory `notes`).

**What it would require (not available today)**
1. A defined receiving party and trigger for the hand-off from internship to placement. Dhee has not named either (inventory: `internship.career_support_handoff`, `blocked_by`).
2. A link between a completed internship application and the placement pipeline. No such join exists in code.
3. A message format that carries the intern's evidence with it, plus an owner who accepts it.
4. An agent identity for Pip, an approved R8 capability, and an agreed protocol for agent-to-agent requests. None exists.
5. Attribution: the Basecamp token acts as Ali, so an agent's actions cannot be attributed to it (inventory: `internship.manager_check_in_brief_and_follow_up`, `blocked_by`).

**First honest version**
No coordination. Pip's output is a note to Dhee, who forwards it to the right person.

## 8. Safeguards: what stops Pip doing harm

**Status: evidenced, with named gaps.**

**What exists today**
- Admission is human only. Approve, reject, and waitlist can only be made by a reviewer. The system actor "may never decide admission" (`services/internship/internshipStateMachine.ts:64-77`, `:154-160`, `:177-178`), and `canTransition` has no override parameter (`:262-281`).
- Ten of the 35 capabilities are marked as reserved to a human (inventory `human_decision`).
- Personal data is a first-class constraint: 31 capabilities are applicant-identifiable, and no agent is granted a production read of the review queue (inventory `pii_exposure`, `notes`).
- Live email, and seven other capabilities, are default-denied for agent runs (`services/delivery/execution/executionPolicy.ts:22-37`).
- A global kill switch is checked before any send (`services/launchSafety.ts:124`).
- Production is never reached by approval alone; a supervised pilot at R6 is separately approved (`services/agentDomain/autonomyLadder.ts:202`).

**Named gaps**
- One coarse permission guards all internship routes: anyone who holds the `internship` section can reach every route, including decide, verify, activate, and raw document download (`routes/admin/internshipRoutes.ts:38`, 27 routes). The inventory records the lack of a finer split and of second-person approval.
- No read audit exists over the personal-data routes. A state-change audit exists; reads are not logged (inventory: `internship.applicant_pii_access_audit`, absent).
- The inventory says `requireAgentManagerOrAdmin` is "proposed and not built". The middleware does now exist (`middlewares/agentManagerAuthMiddleware.ts:50`), but I found no use of it on the internship routes (zero matches in `routes/admin/internshipRoutes.ts`). The inventory statement is partly out of date. Dhee or Ali should confirm the current position.

**First honest version**
Pip starts at `autonomy:R0` on every capability, read-only, with outputs going to Dhee. It cannot send, decide, or touch personal data until the read audit exists and Dhee approves each step.

## 9. Verification: how anyone checks Pip is working

**Status: partly evidenced.**

**What exists today**
- The application has a test suite per module, such as `services/internship/__tests__/internshipStateMachine.test.ts` and `internshipRequirementChecks.test.ts` (34 test files in `services/internship/__tests__/`).
- The platform defines acceptance criteria for research stories (`scripts/lib/researchStoryAcceptance.js:1-25`) and names what does not count as evidence: a story marked complete, elapsed time, a conversation that went well (`services/agentDomain/agentDomainResearchPrompts.ts:74`).
- Promotion approval refuses an empty or non-human approver identity (`services/agentDomain/promotionIdentity.ts:56`, `:88`).

**What is missing**
- Success measures. The inventory leaves them open on every capability, and Dhee has not stated them.
- No test or evaluation exists for Pip, because Pip does not exist.
- Nothing separates what Pip does from what has been approved, since there is no agent to compare.

**First honest version**
Before Pip does anything, Dhee writes down one measurable success statement per capability Pip will touch. The first verification is a read-only run in a sandbox compared against what Dhee would have flagged by hand.

---

## Open decisions for Dhee (these block moving anything out of draft)

1. Is advertising and open house work part of Pip's scope? It is not in the inventory today.
2. Which of the 12 unbuilt or partial capabilities does Pip start with?
3. What are the success measures for each?
4. Who receives a career-support handoff, and what triggers it?
5. Is a read-access audit over applicant personal data wanted?

## Sources

All files below were read directly in the platform repo at commit `825dfe3`.

- `services/agentDomain/capabilities/internship.json` (35 capabilities; `counts`, `notes`, `human_decision`, `required_tools`, `blocked_by`)
- `services/agentDomain/agentDomainRegistry.ts:197-207`: internship domain entry, `agent_id: null` (`:203`), `requested_capabilities: []` (`:206`)
- `services/internship/agentNudgeState.ts:76`: `internship: 'Pip'`
- `services/internship/internshipRequirementChecks.ts:4-11`
- `services/internship/internshipApplicantAssessment.ts:27`, `:108`, `:156`
- `services/internship/internshipStateMachine.ts:33-63`, `:64-77`, `:154-160`, `:177-178`, `:262-281`
- `services/internship/internshipApplicationService.ts:79-95`
- `services/internship/internshipOnboarding.ts:58`
- `services/internship/internshipNudge.ts:7-14`, `:136`, `:144`
- `services/internship/internshipEmails.ts:11-28`
- `services/internship/agentNudgeComposer.ts:1-40`
- `services/internship/internshipWeek3Handoff.ts:2-6`
- `models/InternshipStatusEvent.ts:33`, `models/InternshipDecision.ts:47`
- `models/CommunityEvent.ts:4`, `services/openHouseTypes.ts`
- `services/agentMemoryProposalService.ts:32`, `:74`
- `services/skillGenomeService.ts:48`
- `services/career/careerReadiness.ts:11-15`
- `services/delivery/execution/executionPolicy.ts:22-37`
- `services/launchSafety.ts:124`
- `services/agentDomain/autonomyLadder.ts:2`, `:67`, `:202`
- `services/agentDomain/agentDomainStoryTemplates.ts:160`
- `services/agentDomain/agentDomainResearchPrompts.ts:74`
- `services/agentDomain/promotionIdentity.ts:56`, `:88`
- `services/growthJourney/handoffs/handoffService.ts:1-30`
- `routes/admin/internshipRoutes.ts:38`
- `middlewares/agentManagerAuthMiddleware.ts:50`
- `scripts/lib/internDeliveryCoachNote.js:18`
- `scripts/lib/interviewPrepData.js:7`
- `scripts/lib/reportingRegistry.js:108-146`
- `scripts/lib/researchStoryAcceptance.js`
- `docs/AI_INTERNSHIP_SPEC.md:4-12`
