# internship.CAP.comp_expiry_advance_warning — Comped-access expiry advance warning to the manager

Dhee is warned before a comped intern's free access lapses, so a past student is never dropped to a paywall without anyone intending it.

**Built today:** absent. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- not implemented
- comp expiry storage location unknown
- Dhee has not set the warning lead time or the recipient

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Comped-access expiry advance warning to the manager
**Domain:** Internship & Career Readiness · **Capability:** `internship.comp_expiry_advance_warning`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** absent · **Source:** manager_defined
## The business outcome
Dhee is warned before a comped intern's free access lapses, so a past student is never dropped to a paywall without anyone intending it.
Intended users: Dhee.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the comped set and each person's expiry date
- Select the comps falling inside the warning lead time
- Warn the manager with the person, the date and what the lapse will change
- Record that the warning fired so it is not repeated for the same comp and window
**Inputs**
- comped enrollment ids
- comp expiry date per person
- warning lead time (unknown; Dhee must set it)
**Outputs**
- advance warning to the manager
- list of comps expiring in the window
**Exceptions that must be handled**
- The warning goes to the manager, never to the intern, until Dhee decides otherwise
- A warning must fire once per comp per window, idempotently; a daily re-run must not re-warn
- A comp with no expiry date is a data gap to surface, not a comp to treat as permanent
- An agent must never extend, shorten or revoke a comp; granting free access is the manager's act
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- approved_template_notification_send
- email_send_ledger_idempotency_claim
- sandbox_email_sink
**Application surfaces**
- unknown (no surface exists today)
- unknown (no endpoint exists today)
**Source modules**
- unknown (not implemented)
**Dependencies**
- activeCompEnrollmentIds
- contentEntitlement
- email_send_ledger
- unknown comp expiry storage location
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Nothing is built, so no evidence exists for any level. A manager-facing warning is low blast radius, but the comp data model location is unknown and the lead time is unset, so shadow mode is the honest ceiling until both are resolved.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R0)**
Defines the workflow, the risks and the human decisions for comp expiry warning

Acceptance: Dhee accepts the lead time, the recipient and the once-per-window rule in writing
## Before you start
- not implemented
- comp expiry storage location unknown
- Dhee has not set the warning lead time or the recipient
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- AI_INTERNSHIP_SPEC.md flow decision 4: Dhee can grant free access with an end date she sets; on lapse access converts to requiring a subscription, and she is warned before it expires; comp is time-boxed per person
- AI_INTERNSHIP_SPEC.md revised build order item 5: comp expiry, with the advance warning to Dhee
- AI_INTERNSHIP_DISCOVERY.md: activeCompEnrollmentIds() exists and the entitlement layer already distinguishes comp from paid
- internshipStateMachine.ts:354 hasActiveComp is read at the payment gate, so comp state is already an input to the lifecycle
- no advance-warning job, cron or service was found under backend/src/services/internship/ or in aiOpsScheduler.ts in this pass

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Dhee accepts the lead time, the recipient and the once-per-window rule in writing

When every box above is ticked, stop and show the demo.
