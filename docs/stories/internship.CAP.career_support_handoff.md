# internship.CAP.career_support_handoff — Career preparation and support handoff out of the internship

An intern who lands a role, or who needs placement support, is handed to the right human with their evidence intact, instead of falling between the internship console and the placement pipeline.

**Built today:** absent. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- not implemented
- no in-code join between the internship lifecycle and the placement pipeline
- Dhee has not named the receiving human or the trigger

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Career preparation and support handoff out of the internship
**Domain:** Internship & Career Readiness · **Capability:** `internship.career_support_handoff`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** absent · **Source:** manager_defined
## The business outcome
An intern who lands a role, or who needs placement support, is handed to the right human with their evidence intact, instead of falling between the internship console and the placement pipeline.
Intended users: Dhee, placement staff, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Detect that an intern is completing or has landed a role
- Assemble their evidence and portfolio state
- Hand off to a named human in the placement pipeline
- Record the handoff and who owns the person next
**Inputs**
- completed or completing application
- portfolio projection
- unknown: the placement intake contract
**Outputs**
- unknown
- a named receiving human
- an audit record of the handoff
**Exceptions that must be handled**
- A handoff must name a receiving human; routing to a queue with no owner is how people get lost
- Completion is a reviewer act; an agent must never mark an internship completed
- No in-code join exists between the internship lifecycle and the CCPP placement pipeline, so any claim that a handoff happens today would be invented
- Employment outcome data must not be reported as a Colaberry-issued credential or a placement guarantee
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- ccpp_sql_read
- portfolio_projection_read
**Application surfaces**
- unknown (no handoff surface exists)
- unknown (no endpoint exists today)
**Source modules**
- unknown (not implemented)
**Dependencies**
- internship_applications completed state
- CCPP placement pipeline
- portfolio projection
- unknown placement intake contract
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: Nothing is built and the join between the two systems does not exist. A handoff transfers accountability for a person, which is a human act, so the agent is capped at describing the gap until Dhee defines the contract.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R0)**
Defines the handoff trigger, the receiving human and the evidence that must travel with the person

Acceptance: Dhee names the receiving human and the trigger in writing; until then this capability stays absent
## Before you start
- not implemented
- no in-code join between the internship lifecycle and the placement pipeline
- Dhee has not named the receiving human or the trigger
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- AI_INTERNSHIP_SPEC.md: the programme runs until you secure a full-time position in the AI field, there is no end date, and on landing a role the intern gives one to two weeks notice
- internshipStateMachine.ts:59 completed is a terminal state, and :223 and :230 show active and paused to completed are reviewer-only, so the exit from the programme is already a human act
- backend/src/scripts/lib/interviewPrepData.js and interviewPrepPeople.js are the destination pipeline and are CCPP-sourced
- internshipWeek3Handoff.ts is an INTERNAL project handoff at week 3 and is not a career handoff, despite the similar name
- no module, table, column or route linking a completed internship application to the placement or interview-preparation pipeline was found in this pass

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Dhee names the receiving human and the trigger in writing; until then this capability stays absent

When every box above is ticked, stop and show the demo.
