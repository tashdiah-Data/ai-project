# internship.CAP.application_intake_and_state_transitions.L1-BY-HAND — Internship application intake and single-writer state transitions · 1 · do it by hand

**Understand the work by doing it yourself, before anything automates it.**

Do this capability yourself, by hand, once, end to end. Not a simulation and not a description -- the real task, with the real inputs, producing the real outcome.

**The outcome you are producing**
Every applicant has exactly one live application whose state moves only through one audited, lock-protected writer, so no two reviewers and no retry can produce divergent records.

**The workflow as it was recorded during research.** Treat this as a claim to be tested, not as instructions. Where it is wrong or incomplete, that difference is the single most valuable thing this story produces.
- Applicant starts an application; the open-application unique index makes a second start idempotent
- Administrative intake (Group A) is upserted against the application
- Applicant selects form or phone interview channel
- Any state change calls transition(), which locks the row, asserts the move for that actor, and writes state plus audit event in one transaction
- Terminal states (completed, withdrawn, removed, rejected) have empty outbound rows and refuse all further moves

**What goes in**
- enrollment id
- cohort id
- administrative intake fields
- interview channel choice
- target state
- actor and actor id

**What comes out**
- internship_applications row
- internship_status_events audit row
- InternshipStatusView for the Today card

**Exceptions the research expects you to hit**
- Concurrent reviewers: the row lock serialises them; the second write is an idempotent no-op or an InvalidInternshipTransitionError
- Illegal transition raises InvalidInternshipTransitionError with error_class ContractViolation (internshipStateMachine.ts:245)
- Terminal record: the console route pre-checks isTerminal() and returns 409 (internshipRoutes.ts:213-221)
- A system actor may never create approval; it can only move an already-approved application (internshipStateMachine.ts:177-178)
- Ruth Kumera sits at signed_documents_uploaded and must not be advanced by anything in this programme

**Write a run log.** It is the deliverable of this story and every later step cites it. Record the date, how long it actually took, each step you really performed, every judgment call you made and what you based it on, and at least one thing that was harder, slower or more ambiguous than the workflow above implies.

While you work, keep asking which parts are mechanical and which need judgment. That split is what the tool request in step 3 will be argued from.

The four steps of this capability run in order. A later step opened before its predecessor is complete is the thing this ladder exists to prevent -- say so and go back rather than working ahead.

**Release:** autonomy:R0 · Legacy migration plan (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Internship application intake and single-writer state transitions
**Domain:** Internship & Career Readiness · **Capability:** `internship.application_intake_and_state_transitions`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Every applicant has exactly one live application whose state moves only through one audited, lock-protected writer, so no two reviewers and no retry can produce divergent records.
Intended users: applicant, Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Applicant starts an application; the open-application unique index makes a second start idempotent
- Administrative intake (Group A) is upserted against the application
- Applicant selects form or phone interview channel
- Any state change calls transition(), which locks the row, asserts the move for that actor, and writes state plus audit event in one transaction
- Terminal states (completed, withdrawn, removed, rejected) have empty outbound rows and refuse all further moves
**Inputs**
- enrollment id
- cohort id
- administrative intake fields
- interview channel choice
- target state
- actor and actor id
**Outputs**
- internship_applications row
- internship_status_events audit row
- InternshipStatusView for the Today card
**Exceptions that must be handled**
- Concurrent reviewers: the row lock serialises them; the second write is an idempotent no-op or an InvalidInternshipTransitionError
- Illegal transition raises InvalidInternshipTransitionError with error_class ContractViolation (internshipStateMachine.ts:245)
- Terminal record: the console route pre-checks isTerminal() and returns 409 (internshipRoutes.ts:213-221)
- A system actor may never create approval; it can only move an already-approved application (internshipStateMachine.ts:177-178)
- Ruth Kumera sits at signed_documents_uploaded and must not be advanced by anything in this programme
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- sandbox_database_write
- internship_admin_api_read
- audit_event_write
**Application surfaces**
- student Today card
- admin internship review queue
- admin Intern Console Manage drawer
- POST /api/admin/internship/applications/:id/transition
**Source modules**
- backend/src/services/internship/internshipApplicationService.ts
- backend/src/services/internship/internshipStateMachine.ts
- backend/src/models/InternshipApplication.ts
- backend/src/models/InternshipStatusEvent.ts
- backend/src/db/ensureInternshipSchema.ts
**Dependencies**
- enrollments
- cohort_memberships
- internshipStateMachine (pure)
- internshipAnalytics event emitter
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: The writer is the gate for every admission outcome. An agent may exercise it against sandbox fixtures, but production writes to internship_applications.state would let an agent move a real applicant, so no level above R4 is proposed.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Answers which states an application can legally reach next, and for which actor, citing the state machine

Acceptance: Named scenarios across all 22 states answered with zero illegal transitions asserted as legal; evaluator is a human reviewer against internshipStateMachine.ts
## Before you start
- Dhee has not validated scope
- no sandbox fixture set covering the 22 states exists yet
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipApplicationService.ts:23-34 (the ONLY writer of internship_applications.state)
- internshipApplicationService.ts:55 transition(), :63-66 SELECT FOR UPDATE, :79 assertTransition, :81-95 state write and audit event in one transaction
- internshipStateMachine.ts:33-63 (22 states), :77 actor union applicant|reviewer|system, :266-279 canTransition has no force/isAdmin/override parameter
- internshipApplicationService.ts:140 startApplication (idempotent), :194 saveAdministrativeIntake, :245 selectInterviewChannel, :312 getStatus
- ensureInternshipSchema.ts:107 uq_internship_applications_open (one live application per person), :427 names it load-bearing
- tables: internship_applications, internship_status_events, internship_administrative_intakes, internship_card_dismissals

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] The capability was performed end to end by hand, producing its real outcome on real inputs.
- [ ] A run log exists recording the date, the elapsed time, each step actually performed, every judgment call made, and at least one way the recorded workflow differed from what the work was really like.

When every box above is ticked, stop and show the demo.
