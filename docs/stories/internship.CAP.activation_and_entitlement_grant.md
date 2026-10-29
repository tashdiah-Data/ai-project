# internship.CAP.activation_and_entitlement_grant — Internship activation and curriculum entitlement grant

An intern gains full training access only when every blocker is cleared, and activation is an explicit human act rather than a side effect of a file arriving.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant
- Dhee has not validated what the agent may surface about activation readiness

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Internship activation and curriculum entitlement grant
**Domain:** Internship & Career Readiness · **Capability:** `internship.activation_and_entitlement_grant`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern gains full training access only when every blocker is cleared, and activation is an explicit human act rather than a side effect of a file arriving.
Intended users: Dhee, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Build the activation checklist and compute blockers
- Refuse activation while any blocker remains
- Create or confirm the internship cohort membership
- Transition the application to active and record the activation timestamp
- Add the intern to the standup meeting room
**Inputs**
- application id
- checklist blockers
- cohort membership state
- activator identity from the session
**Outputs**
- active application
- cohort_memberships row
- standup room membership
- ActiveInternView
**Exceptions that must be handled**
- ACTIVATION GRANTS PAID TRAINING. It must not be reachable by any automated path that has not cleared the payment or comp gate
- Membership creation must be idempotent at the database; a retry must not create a second active cohort_memberships row
- A blocker present means refuse, not warn and proceed
- Ruth Kumera must not be activated and her onboarding requirements must not be bypassed
- An agent must never activate an intern
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- sandbox_database_write
**Application surfaces**
- admin application detail activation panel
- POST /api/admin/internship/applications/:id/activate
- GET /api/admin/internship/applications/:id/onboarding
**Source modules**
- backend/src/services/internship/internshipActivationService.ts
- backend/src/services/internship/internshipCohortService.ts
- backend/src/services/internship/internshipMeetingRooms.ts
**Dependencies**
- internshipOnboarding
- cohort_memberships
- internshipCohortService
- internshipMeetingRooms
- contentEntitlement
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: Activation grants paid curriculum entitlement to a named person and is the most expensive act in this domain. The agent is capped at answering questions about blockers from approved knowledge; even shadow mode is not proposed, because a shadow activation list invites a one-click bulk apply.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Answers which blockers stand between an application and activation, citing the checklist

Acceptance: Blocker list matches activationBlockers() exactly; the output never contains an activation instruction or a ready-to-activate verdict
## Before you start
- human-decision invariant
- Dhee has not validated what the agent may surface about activation readiness
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipActivationService.ts:135 activate(), :88 advanceAfterDocumentsVerified, :57 checkMembership, :265 buildChecklist, :120 ActivationResult
- internshipStateMachine.ts:216 activation_pending to active, :218 activation_pending to removed (reviewer only)
- internshipRoutes.ts:847 POST /applications/:id/activate requireSection('internship')
- AI_INTERNSHIP_DISCOVERY.md risk R1: built literally, document verification would grant hasFullCurriculumAccess(), meaning full paid training free forever
- internshipCohortService.ts:31 INTERNSHIP_COHORT_TYPE ai_internship, :129 ensureInternshipCohort
- internshipMeetingRooms.ts:130 ensureInternInStandupRoom
- cohort_memberships DDL at ensureInternshipSchema.ts:51; AI_INTERNSHIP_DISCOVERY.md risk R3 requires a DB-enforced unique partial index on active membership

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Blocker list matches activationBlockers() exactly; the output never contains an activation instruction or a ready-to-activate verdict

When every box above is ticked, stop and show the demo.
