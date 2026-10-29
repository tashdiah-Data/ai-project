# internship.CAP.admission_decision_recording — Admission decision recording with a mandatory student-safe reason

Acceptance, rejection, waitlist and information requests are recorded by a named human with a reason code the applicant can be told, so no one is declined without an explanation and no decision is anonymous.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant; no path to agent decisioning exists in this or any later task
- Dhee has not validated the shadow comparison design

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Admission decision recording with a mandatory student-safe reason
**Domain:** Internship & Career Readiness · **Capability:** `internship.admission_decision_recording`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Acceptance, rejection, waitlist and information requests are recorded by a named human with a reason code the applicant can be told, so no one is declined without an explanation and no decision is anonymous.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Validate the reason code against the decision scope; a reject or waitlist without a valid code is refused
- Snapshot the AI recommendation as it stood at decision time
- Transition the application as the reviewer actor
- Append the immutable decision row with decided_by taken from the verified session
- Send the decision email last; a send failure is recorded and never rolls back the decision
**Inputs**
- application id
- decision verb
- reason code
- optional student message
- optional reviewer notes
- optional conditions
- decider identity from the session
**Outputs**
- internship_decisions append-only row
- state transition with audit event
- decision email
- ai_recommendation and ai_factors snapshot
**Exceptions that must be handled**
- AN AGENT MAY NEVER RECORD AN ADMISSION DECISION. Internship acceptance is always a human decision and the state machine enforces it by actor, not by convention
- other_see_message requires a non-empty custom message, so Other cannot become the default rejection
- reviewer_notes must never reach applicant-facing text; buildStudentFacingReason cannot accept them
- Email failure after a recorded decision is swallowed and logged; the decision stands and the send outcome is recorded
- Two reviewers deciding at once are serialised by the row lock in transition()
- A terminal application refuses a second decision
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- reason_code_catalog_read
**Application surfaces**
- admin application detail Decide panel
- POST /api/admin/internship/applications/:id/decide
**Source modules**
- backend/src/services/internship/internshipDecisionService.ts
- backend/src/services/internship/internshipReasonCodes.ts
- backend/src/models/InternshipDecision.ts
**Dependencies**
- internshipApplicationService.transition
- internshipReasonCodes
- internshipEmails
- internshipRecommendation
- email_send_ledger
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Internship acceptance is always a human decision. The agent may reach shadow mode, producing a would-be posture compared against the human outcome, and no further: R6 and above would imply a pilot in which the agent acts, and there is no action here it is permitted to take.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains the ten reason codes, their scopes and their reapply windows, and refuses to suggest a decision

Acceptance: Explanations match REASON_DEFINITIONS; any output phrased as a decision fails the release
## Before you start
- human-decision invariant; no path to agent decisioning exists in this or any later task
- Dhee has not validated the shadow comparison design
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipDecisionService.ts:96 decide(), :166-171 transitions with actor reviewer, :173-186 appends the InternshipDecision row, :265 decisionHistory
- internshipStateMachine.ts:155-161 and :170-175: approved, rejected and waitlisted are reachable only by the reviewer actor
- internshipStateMachine.ts:67-76: the system actor is deliberately the weakest and may never decide admission
- InternshipDecision.ts:59 reason_code STRING(60) allowNull false (the student-safe-reason rule enforced at the database)
- internshipReasonCodes.ts:30-43 ten codes, :210-212 requiresCustomMessage only for other_see_message, :229 reapplyDate, :245 buildStudentFacingReason which does not take reviewer_notes as a parameter
- internshipRoutes.ts:650 POST /decide requireSection('internship'); :660-662 decided_by derived from the verified JWT, never the body
- internshipRoutes.ts:51-55: a generic PATCH state endpoint was deliberately not built
- AI_INTERNSHIP_SPEC.md flow decision 1: Dhee approves first; a human checks the not-employed-full-time attestation

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Explanations match REASON_DEFINITIONS; any output phrased as a decision fails the release

When every box above is ticked, stop and show the demo.
