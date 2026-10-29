# internship.CAP.routine_document_reminder_sending — Approved routine document and activity reminder sending

An intern who has gone quiet or has an outstanding document gets a timely reminder from an approved template, exactly once per day per template, without a human having to notice first.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- live email is a default-denied capability (executionPolicy.ts:33), so production sending requires a separately approved action class and a named approver, not this matrix
- no cron wires internshipNudge today; sending is manager-triggered from the console, so the scheduled path does not exist yet
- Dhee has not validated which templates may ever fire without a human click

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Approved routine document and activity reminder sending
**Domain:** Internship & Career Readiness · **Capability:** `internship.routine_document_reminder_sending`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** existing_application
## The business outcome
An intern who has gone quiet or has an outstanding document gets a timely reminder from an approved template, exactly once per day per template, without a human having to notice first.
Intended users: intern, Dhee.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Check the global kill switch; if active, send nothing
- Resolve the template from the fixed allowlist; reject any unknown key
- Claim the idempotency key (template, application, day) in email_send_ledger
- Render the approved copy plus the escaped, length-capped manager note
- Send and record the outcome; a failed send does not consume the claim silently
**Inputs**
- application id
- template key from a fixed allowlist
- optional short note
- sender identity from the session
**Outputs**
- one sent email
- email_send_ledger claim row
- send outcome recorded against the application
**Exceptions that must be handled**
- DUPLICATE PREVENTION: the business event is template plus application plus day. A double-clicked button or a retried request is the same nudge; tomorrow's is genuinely a different one. Without the day an intern could be nudged once ever; with a timestamp the ledger would protect nobody
- INCORRECT RECORDS: an application whose state does not match the template precondition must not be nudged. A weeks_1_3_reminder to an intern who has cleared the gate is a wrong statement to a real person and must be refused at precondition, not apologised for afterwards
- MISSING RECORDS: no enrollment email, no intake first name, or no application row means skip with a recorded reason, never send to a guessed address and never substitute a placeholder into the greeting
- CONTACT RESTRICTIONS: the kill switch halts all sends; a withdrawn, removed, rejected or completed application is never nudged; students are never bulk-emailed; the caller may supply only a template key and a short escaped note, so no request can place unreviewed text in front of a student
- ESCALATION: a send failure, a template precondition mismatch, a kill switch encountered mid-batch, or any ledger-claim conflict that is not an exact replay escalates to Dhee with the application id and the reason, and the agent stops rather than retrying unboundedly
- AN AGENT MAY NEVER SELF-APPROVE AN APPLICANT. Sending a reminder is an authorized routine action class; moving an application toward approval, verifying a document, or recording a decision is not, and no reminder path may be used to advance state. Internship acceptance remains a human decision
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- approved_template_notification_send
- email_send_ledger_idempotency_claim
- internship_admin_api_read
- kill_switch_read
- sandbox_email_sink
**Application surfaces**
- admin Intern Console Manage drawer nudge action
- POST /api/admin/internship/applications/:id/nudge
**Source modules**
- backend/src/services/internship/internshipNudge.ts
- backend/src/services/email/idempotentSend.ts
- backend/src/db/ensureEmailSendLedgerSchema.ts
- backend/src/scripts/dailyInternNudges.js
**Dependencies**
- email_send_ledger
- email/idempotentSend sendOnce
- emailService guardedSendMail
- launchSafety kill switch
- internship_applications state
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R7**.
Why that ceiling: This is the owner's worked example: an internship agent may eventually send approved routine document reminders while internship acceptance remains a human decision. R7 automates only that explicitly authorized action class, bounded to the fixed template allowlist and the day-scoped idempotency key. R8 and R9 are not proposed, because a handoff or an ownership claim would widen the action class beyond reminders.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains which template applies to an intern's current state and why, and sends nothing

Acceptance: Template selection matches the precondition table for every fixture; zero send attempts recorded
## Before you start
- live email is a default-denied capability (executionPolicy.ts:33), so production sending requires a separately approved action class and a named approver, not this matrix
- no cron wires internshipNudge today; sending is manager-triggered from the console, so the scheduled path does not exist yet
- Dhee has not validated which templates may ever fire without a human click
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipNudge.ts:5-16: the caller chooses a template and never the words; there is no parameter for a subject, body, HTML or custom message
- internshipNudge.ts:18-30: every send goes through sendOnce, whose claim is enforced by unique indexes on email_send_ledger, with business event internship-nudge-<template>-<applicationId>-<day>
- internshipNudge.ts:31: the global kill switch is checked first, because a stop that does not stop outbound mail is a flag, not a stop
- internshipNudge.ts:33 NudgeTemplate (quiet_check_in, weeks_1_3_reminder, project_start), :52 NUDGE_TEMPLATES, :90 renderNudge with a 400-character escaped note cap
- internshipRoutes.ts:273 POST /applications/:id/nudge requireSection('internship'), :263-266 the caller can never supply the words
- db/ensureEmailSendLedgerSchema.ts implements the (recipient, subject, business_event_id) key with two unique indexes
- services/launchSafety.ts isKillSwitchActive
- backend/src/scripts/dailyInternNudges.js plus reportingRegistry.js:120-132 (a separate Basecamp-sourced daily nudge digest at 17:00 UTC with a GREEN to BLACK escalation ladder)

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Template selection matches the precondition table for every fixture; zero send attempts recorded

When every box above is ticked, stop and show the demo.
