# internship.CAP.class_internship_movement — Moving a person between class and internship without losing history

A student can join the internship, or an intern can drop into a live class, with their progress, cohort and history intact and no second divergent record of who they are.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no dedicated move endpoint was identified in this pass
- Dhee has not validated the move workflow

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Moving a person between class and internship without losing history
**Domain:** Internship & Career Readiness · **Capability:** `internship.class_internship_movement`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** manager_defined
## The business outcome
A student can join the internship, or an intern can drop into a live class, with their progress, cohort and history intact and no second divergent record of who they are.
Intended users: Dhee, intern, student.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Identify the person's single enrollment
- Add or deactivate an internship cohort membership rather than creating a second enrollment
- Confirm progress, points and history are unchanged
- Record the move and the manager who made it
**Inputs**
- enrollment id
- source cohort
- target membership type
- manager identity
**Outputs**
- cohort_memberships row
- preserved enrollment and progress
- audit record of the move
**Exceptions that must be handled**
- Never create a second active enrollment for one person; the duplicate sweep would destroy one of them
- A comped person must not resolve to the wrong enrollment and receive a paywall
- A move must be idempotent at the database, not checked in application code
- Reversing a move must not delete history; deactivate rather than destroy
- An agent must never move a person between class and internship; this is the manager's act
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
**Application surfaces**
- admin Intern Console Manage drawer
- admin application detail
- POST /api/admin/internship/applications/:id/transition
- unknown (no dedicated move endpoint identified)
**Source modules**
- backend/src/services/internship/internshipActivationService.ts
- backend/src/db/ensureInternshipSchema.ts
**Dependencies**
- cohort_memberships
- enrollments
- participantService.pickBestEnrollment call sites
- contentEntitlement
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: The move touches entitlement and the enrollment identity that roughly twenty route files read. A wrong move shows a student a complete, coherent, wrong portal with nothing logged, which is why the agent is capped at explaining the model rather than operating it.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains why the internship is a membership on one enrollment rather than a second enrollment, and what breaks under the alternative

Acceptance: Explanation names the duplicate-sweep, comp-entitlement and pickBestEnrollment failures with their citations and claims no capability that does not exist
## Before you start
- no dedicated move endpoint was identified in this pass
- Dhee has not validated the move workflow
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- AI_INTERNSHIP_SPEC.md flow decision 3: moving from a class keeps the class enrollment and adds the internship on top; progress, cohort and history survive untouched
- AI_INTERNSHIP_SPEC.md: Dhee needs to review, accept, and move a student between class and internship, and this is the piece with no analogue today
- AI_INTERNSHIP_DISCOVERY.md 3.2: cohort_memberships delivers the stated outcome while keeping one enrollment per person, retiring the duplicate-sweep, comp-entitlement and pickBestEnrollment failures the audit ranked highest
- AI_INTERNSHIP_DISCOVERY.md cites duplicateAccountSweepService.ts:237 destroying a legitimate enrollment under the two-enrollment mechanism, and participantService.ts:67 pickBestEnrollment with 29 references
- internshipActivationService.ts:57 checkMembership, :43 MembershipCheck
- ensureInternshipSchema.ts:51 cohort_memberships DDL

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Explanation names the duplicate-sweep, comp-entitlement and pickBestEnrollment failures with their citations and claims no capability that does not exist

When every box above is ticked, stop and show the demo.
