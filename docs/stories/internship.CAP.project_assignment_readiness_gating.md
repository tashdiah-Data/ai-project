# internship.CAP.project_assignment_readiness_gating — Project assignment readiness gating on the weeks 1 to 3 training gate

Projects are assigned to interns who have cleared the training gate, and an intern waiting on Colaberry to assign is shown as waiting on us rather than as behind.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the weeks 1 to 3 gate as the current rule

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Project assignment readiness gating on the weeks 1 to 3 training gate
**Domain:** Internship & Career Readiness · **Capability:** `internship.project_assignment_readiness_gating`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Projects are assigned to interns who have cleared the training gate, and an intern waiting on Colaberry to assign is shown as waiting on us rather than as behind.
Intended users: Dhee, internship-section staff, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Compute weeks completed from the intern's curriculum rows
- Decide readiness against the weeks 1 to 3 gate
- Derive the handoff phase and its owner from readiness plus project state
- List ready interns for the reviewer to assign
**Inputs**
- enrollment id
- weekly curriculum progress
- active project state
**Outputs**
- readiness row per intern
- handoff phase with owner
- ready-for-project list
**Exceptions that must be handled**
- An unknown week is reported as unknown, never as before the gate and never as late
- awaiting_assignment and access_pending are Colaberry's phases and must never read as intern lateness
- Readiness is a gate, not a recommendation of which project to assign
- A readiness list is per intern and therefore identifiable; it is not aggregate reporting
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- project_readiness_read
**Application surfaces**
- admin internship project readiness view
- student Today card handoff card
- GET /api/admin/internship/project-readiness
**Source modules**
- backend/src/services/internship/internshipProjectReadiness.ts
- backend/src/services/internship/internshipWeek3Handoff.ts
- backend/src/services/internship/internshipActivityService.ts
**Dependencies**
- student curriculum week rows
- projects
- internshipActivityService
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: Pure read-and-derive with no writes and no external side effects, so a separately approved supervised production pilot of the readiness report is reachable on measured evidence.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
States whether an intern has cleared the training gate and whose move the handoff is

Acceptance: Matches readyForProject() and the handoff phase table; an unknown week never yields a late reading
## Before you start
- Dhee has not validated the weeks 1 to 3 gate as the current rule
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipProjectReadiness.ts:16 ProjectReadinessRow, :32 readyForProject, :36 internshipProjectReadiness
- internshipWeek3Handoff.ts:24 HANDOFF_WEEK 3, :26-32 five phases (unknown, before_week_3, awaiting_assignment, access_pending, in_progress), :11-18 two phases carry owner colaberry with actionable false
- internshipActivityService.ts:28 WeekProgress, :36 InternActivity, :62 internActivity
- internConsoleRoster.ts:229 weeksCompletedFrom, :247 paceFor, :264 dayOfInternship, :53 ConsoleTraining
- internshipRoutes.ts:402 GET /project-readiness
- internshipNudge.ts:52 weeks_1_3_reminder template exists for this gate

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches readyForProject() and the handoff phase table; an unknown week never yields a late reading

When every box above is ticked, stop and show the demo.
