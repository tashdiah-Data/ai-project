# Three proposed agent tasks for Pip: boundaries and completion evidence

**Story:** `internship.RESEARCH-5-THREE-PROPOSED-TASKS` · **Domain:** Internship & Career Readiness · **Accountable human:** Dhee
**Author:** Tashima Diah (draft prepared with Claude) · **Written:** 2026-10-29
**Evidence base:** the domain inventory `backend/src/services/agentDomain/capabilities/internship.json` (35 capabilities, researched 2026-10-09), the autonomy ladder `backend/src/services/agentDomain/autonomyLadder.ts` (platform repo commit `bdcd032`), and my earlier documents `docs/discovery/02-agent-anatomy.md` (commit `825dfe3`) and `03-role-and-da-transition.md`. The story brief was not available to me, so none of its figures appear here.
**Status:** Draft. No capability is enabled, no tool is granted and no autonomy level is raised by this document. Every capability named here stays `draft` until Dhee has reviewed it.

## Headline: 3 tasks, all chosen from the 12 unbuilt or partly built capabilities

| # | Task | Inventory capability | Starts at | Ceiling | Approver |
| --- | --- | --- | --- | --- | --- |
| 1 | Prepare a manager check-in brief | `manager_check_in_brief_and_follow_up` | `autonomy:R0` | `autonomy:R5` | Dhee |
| 2 | Propose the day's reminders in shadow mode, with no sending | `routine_document_reminder_sending` | `autonomy:R0` | `autonomy:R7` (this task asks for no more than `autonomy:R5`) | Dhee |
| 3 | Report the unpaid-limbo list in shadow mode | `payment_gate_monitoring` | `autonomy:R0` | `autonomy:R5` | Dhee |

**How I chose.** The story says to choose from the domain's own draft capabilities and not to invent tasks. All 35 capabilities in the inventory are `draft`. I picked three where the inventory already states a check that can be done against something real, and where nothing needs a human decision to be taken away from a person. I left out the 10 capabilities reserved to a human. I also left out skill-gap identification, which belongs to Career Readiness as Ali defined it, because it is blocked on a definition nobody has written: no target skill profile exists and Dhee has not defined a gap. I cannot state honest completion evidence for it, so by the story's own rule it is not ready to propose.

**Where Pip stands today.** No agent exists for this domain (`agent_id: null`, `requested_capabilities: []`, per `02-agent-anatomy.md`). Under the ladder's own rule, a capability with no approved level has no effective authority (`no_approved_autonomy`), and one with no tool grants is capped at `autonomy:R2` (`autonomyLadder.ts:188-191`, `:207-211`). So all three tasks currently have no authority at all, and proposing them changes nothing.

## What the levels mean

From `autonomyLadder.ts:58-69`, in the owner's words:

| Level | Meaning |
| --- | --- |
| R0 | Define the workflow, requirements, risks and human decisions. |
| R2 | Answer questions using approved knowledge and records. |
| R4 | Execute and verify stateful, multi-step sandbox tasks. |
| R5 | Evaluate in shadow mode against approved reference outcomes. |
| R6 | Conduct a separately approved, supervised production pilot. |
| R7 | Automate only explicitly authorized action classes within limits. |

Three rules from the same file apply to every task below. Levels belong to one capability, never to the agent, so raising one cannot raise another (`:2`, `:176-184`). Values are stored as `autonomy:R5`, never a bare `R5`, because the platform's risk scale already uses bare R0 to R5 for something else (`:6-19`). Production is never reached by approval alone; a pilot at R6 or above needs its own approval (`:202-205`).

## The evidence rule

The story says completion evidence may not be: a story marked done, time having passed, a conversation that went well, or a mocked test passing. It also says the platform carries 106 approval rows whose only recorded decider is a 4.25-hour timer. I could not verify that figure myself, and it is taken from the story prompt.

Each task below therefore names evidence that is (a) a comparison against a real reference, such as a deterministic function's output or the records a person actually produced, and (b) read and recorded by Dhee by name. The platform already refuses an empty or non-human approver identity on promotion approval (`services/agentDomain/promotionIdentity.ts:56`, `:88`, per `02-agent-anatomy.md`). No task here relies on a timer, a system actor or an automatic expiry as its decider.

---

## Task 1: Prepare a manager check-in brief

**Capability:** `internship.manager_check_in_brief_and_follow_up` (partly built)

### Evidence it completed correctly (written first)

| Rung | What counts as evidence | Source |
| --- | --- | --- |
| R2 | For each intern in a named sample, the brief's percentages and projected dates equal the output of `scripts/lib/internDeliveryCoachNote.js` exactly, and no figure comes from the model. Dhee reads the comparison and records the result. | `internship.json`, `release_behaviour` |
| R5 (ceiling) | Zero Basecamp writes are attributable to the agent. The drafted note, set beside the one the manager actually posted, has no em dash and no second sign-off. | Same |

Sample size: unknown. Dhee sets it.

### The task

Before a check-in, give the manager the intern's real position, what they are working on and the projected completion date. Afterwards, draft a supportive follow-up note that the manager posts as their own.

### Tools and data

- Data: the intern delivery snapshot, task completion state, and the projected completion arithmetic.
- Sandbox tools: `sandbox_database_read`, `llm_text_generation`.
- Production tools: `internship_admin_api_read`, `basecamp_read`, both read-only.
- Source for the lists: `internship.json`, `required_tools` and `permissions_by_environment`.

### Boundary: what Pip must NOT do

- Must not post anything to Basecamp. The note is the manager's to post, because an agent posting it would speak as a named human.
- Must not compute any percentage or date with the model. They stay deterministic.
- Must not claim a follow-up is tracked, because no check-in record exists.
- Must not use the Basecamp token, which acts as Ali, so an automated comment would be attributed to him.

### Starting level, ceiling and why

Starts at `autonomy:R0`. Ceiling `autonomy:R5`. The inventory says preparing a brief is safe and already deterministic, but posting the note would speak in a named person's voice through a token that acts as Ali, so shadow mode is the ceiling until a write path attributed to an agent exists.

### Approver

Dhee, as the accountable human. The manager posts any note. Who the manager is: unknown.

### What would make me switch it off

- Any figure in a brief that differs from the coach-note arithmetic.
- Any figure that came from the model.
- Any Basecamp write attributable to Pip, or any comment appearing under a person's name that Pip produced.

**Blocked today by:** no check-in record model, the Basecamp token acting as Ali, and Dhee not having validated the check-in cadence.

---

## Task 2: Propose the day's reminders in shadow mode

**Capability:** `internship.routine_document_reminder_sending` (partly built)

This task proposes reminders. It does not send them.

### Evidence it completed correctly (written first)

| Rung | What counts as evidence | Source |
| --- | --- | --- |
| R2 | For each fixture, the template Pip selects matches the precondition table, and zero send attempts are recorded. | `internship.json`, `release_behaviour` |
| R5 | There are zero `email_send_ledger` rows for real recipients. The proposed reminder list is compared with what Dhee actually sent, and the agreement is reported as proposed until Dhee accepts a threshold. | Same |

The agreement threshold: unknown. Dhee sets it.

### The task

Each day, list which interns would get which approved reminder template, and why. Put the list in front of Dhee, who sends any reminder.

### Tools and data

- Data: application id, state, and the fixed template allowlist (`quiet_check_in`, `weeks_1_3_reminder`, `project_start`).
- Sandbox tools: `sandbox_email_sink`, `email_send_ledger_idempotency_claim`, `sandbox_database_read`.
- Production tool: `kill_switch_read` only.
- Source for the lists: `internship.json`, `permissions_by_environment`; templates from `02-agent-anatomy.md`.

### Boundary: what Pip must NOT do

- Must not send any email to a real person. The output goes to a sink.
- Must not nudge an application whose state does not match the template, or one that is withdrawn, removed, rejected or completed.
- Must not send to a guessed address or put a placeholder in a greeting.
- Must not use a reminder to advance an application, verify a document or record a decision. An agent may never self-approve an applicant.

### Starting level, ceiling and why

Starts at `autonomy:R0`. The inventory ceiling is `autonomy:R7`, set because this is the owner's worked example of an authorized routine action class. **This task asks for no more than `autonomy:R5`.** R7 would need its own approval, a named approver, and a decision that live email may leave the default-denied list (`executionPolicy.ts:33`, per the inventory). None of that exists, and this document does not request it.

### Approver

Dhee. For a future R7 step, the inventory calls for "a separately approved action class and a named approver." That person: unknown.

### What would make me switch it off

- Any ledger row for a real recipient while Pip is in shadow mode.
- A proposed reminder for an application in a closed state.
- Agreement with Dhee's actual sends falling below the threshold Dhee sets.
- The global kill switch being active. It halts all sends.

**Blocked today by:** no scheduled path exists (sending is manager-triggered from the console), live email is default-denied, and Dhee has not validated which templates may ever go out without a human click.

---

## Task 3: Report the unpaid-limbo list in shadow mode

**Capability:** `internship.payment_gate_monitoring` (partly built)

### Evidence it completed correctly (written first)

| Rung | What counts as evidence | Source |
| --- | --- | --- |
| R2 | For all four combinations of subscription and comp status, Pip's explanation of why an application is at `payment_pending` rather than `activation_pending` matches `stateAfterDocumentsVerified()`. | `internship.json`, `release_behaviour` |
| R5 | Zero state writes are attributable to Pip. The unpaid list and proposed expiry dates, set beside what the manager acted on, reconcile against the onboarding bucket with no false entries. | Same |

### The task

List accepted applicants who have not paid, with proposed expiry dates, so none stalls unnoticed and document verification never grants paid training for free.

### Tools and data

- Data: application id, the cohort's `requires_subscription` setting, subscription status, comp status.
- Sandbox tools: `sandbox_database_read`, `subscription_status_read`.
- Production tool: `internship_admin_api_read`, read-only.
- Source: `internship.json`, `required_tools` and `permissions_by_environment`.

### Boundary: what Pip must NOT do

- Must not expire or withdraw any application. Expiring withdraws a real person's place, and that act stays with the reviewer or the existing system edge.
- Must not list a comped applicant as unpaid.
- Must not change the `requires_subscription` setting.
- Must not write any application state.

### Starting level, ceiling and why

Starts at `autonomy:R0`. Ceiling `autonomy:R5`. The inventory says reading and reporting the unpaid set is safe, but expiring an application withdraws a person's place, so the ceiling is shadow mode and the expiry act stays human.

### Approver

Dhee. The person who acts on the list and runs any expiry is the reviewer or the existing system edge. Who the reviewer is: unknown.

### What would make me switch it off

- Any state write by Pip, however small.
- A false entry on the list, or a comped applicant shown as unpaid.
- A proposed expiry before Dhee has set an expiry window.

**Blocked today by:** no scheduler wires the expiry or the payment reminder, and Dhee has not set the expiry window (unknown).

---

## Open decisions for Dhee (these block moving anything out of draft)

1. Are these the right three tasks? If not, which capabilities from the inventory would Dhee pick instead?
2. What sample size and what agreement threshold count as enough evidence for each task?
3. Who is the manager for Task 1, and who is the reviewer for Task 3?
4. Who can switch each task off, and how? A global kill switch exists for sends (`services/launchSafety.ts:124`, per `02-agent-anatomy.md`). Nothing is named for the other two tasks.
5. Which reminder templates, if any, may ever go out without a human click, and what expiry window applies to the unpaid list?

## Done means: where each line is answered

| Done means | Where |
| --- | --- |
| Exactly three agent tasks are proposed, each with its boundary stated as what the agent must NOT do. | The three task sections, each with "Boundary: what Pip must NOT do." |
| Each task names the evidence that proves it completed, and that evidence is something other than the task being marked done. | Each task's "Evidence it completed correctly," plus the evidence rule above. |
| Each task names the human who approves it wherever approval is required. | Each task's "Approver." |

## Sources

All files below are in the platform repo at the commit named. Line numbers for `autonomyLadder.ts` were checked against the attached copy. Others are as reported in `02-agent-anatomy.md`, and I did not re-check them.

- `backend/src/services/agentDomain/capabilities/internship.json`: capabilities `manager_check_in_brief_and_follow_up`, `routine_document_reminder_sending`, `payment_gate_monitoring` (fields `required_tools`, `permissions_by_environment`, `max_autonomy_rationale`, `exceptions`, `release_behaviour`, `blocked_by`)
- `backend/src/services/agentDomain/autonomyLadder.ts` (commit `bdcd032`): `:2`, `:6-19`, `:58-69`, `:176-184`, `:188-191`, `:202-211`
- `docs/discovery/02-agent-anatomy.md` (commit `825dfe3`): the headline's finding on `agent_id: null`; sections 4, 8 and 9; `services/launchSafety.ts:124`; `services/agentDomain/promotionIdentity.ts:56`, `:88`; `services/delivery/execution/executionPolicy.ts`
- `docs/discovery/03-role-and-da-transition.md`: section 4, the human-reserved capabilities
- `scripts/lib/internDeliveryCoachNote.js`: the arithmetic Task 1 is compared against (named in the inventory; not read by me)
- The story prompt for `internship.RESEARCH-5-THREE-PROPOSED-TASKS`: the evidence rule and the 106-row figure, not verified by me
