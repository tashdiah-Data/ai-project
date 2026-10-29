# internship.CAP.build_progress_and_pace_monitoring — Project progress, pace and deadline monitoring

A manager sees where each intern is against their build, how fast they are moving and when they are projected to finish, from deterministic arithmetic rather than a model's impression.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the pace thresholds
- the escalation ladder is Ali policy from 2026-05-30 and has not been re-validated for the internship cohort

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Project progress, pace and deadline monitoring
**Domain:** Internship & Career Readiness · **Capability:** `internship.build_progress_and_pace_monitoring`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
A manager sees where each intern is against their build, how fast they are moving and when they are projected to finish, from deterministic arithmetic rather than a model's impression.
Intended users: Dhee, Ali, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Harvest task state and Basecamp activity per intern
- Compute day of internship, weeks completed, pace and escalation level by arithmetic
- Derive a deterministic standing from verified versus self-reported stories
- Call the model only for the narrative and the manager's question, grounded in those facts
- Fall back to the deterministic standing when the model is unavailable
**Inputs**
- enrollment id
- student_tasks completion state
- Basecamp todo and comment activity
- join date
- optional manager question
**Outputs**
- ConsoleRosterRow with pace and day N
- deterministic standing
- completion percentage and projected completion date
- narrative answer to a manager question
**Exceptions that must be handled**
- Dates and percentages must never come from a model; a projected completion date a person is held to cannot be rounded by a language model
- Task titles and notes enter the prompt as fenced untrusted evidence
- An intern with no activity data is UNKNOWN, not BLACK; internActivityTracker.js:196 records that the nudge job skips UNKNOWN after a null-days bug produced false BLACK levels
- A model outage must yield a deterministic standing, never a blank or a guess
- Project review is generated on manager demand so no LLM cost is paid per page load
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- basecamp_read
- llm_text_generation
- structured_output_validation
- prompt_injection_fencing
**Application surfaces**
- admin Intern Console
- admin Intern Console detail
- intern delivery dashboard HTML
- GET /api/admin/internship/console
- GET /api/admin/internship/console/:enrollmentId
- POST /api/admin/internship/applications/:id/project-review
**Source modules**
- backend/src/services/internship/internConsoleRoster.ts
- backend/src/services/internship/internConsoleDetail.ts
- backend/src/services/internship/internshipProjectReview.ts
- backend/src/scripts/lib/internActivityTracker.js
- backend/src/scripts/lib/internDeliveryMetrics.js
**Dependencies**
- student_tasks
- projects
- Basecamp project 24865175
- runtime/runtimeAi chatJson
- inboxCase/promptSafety
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: Monitoring reads and reports and writes nothing to a person's record, so a separately approved supervised production pilot is reachable. It stays below action automation because the actions that follow a bad pace reading, nudging and exiting, are separate capabilities with their own ceilings.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Reports an intern's day N, weeks completed, pace and standing from stored facts

Acceptance: Figures match paceFor(), dayOfInternship() and deterministicStanding() exactly; zero dates or percentages originate from the model
## Before you start
- Dhee has not validated the pace thresholds
- the escalation ladder is Ali policy from 2026-05-30 and has not been re-validated for the internship cohort
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internConsoleRoster.ts:247 paceFor, :264 dayOfInternship, :229 weeksCompletedFrom, :280 getConsoleRoster, :386 consoleCounts, :46 ConsolePace, :65 ConsoleProject
- internshipProjectReview.ts:64 deterministicStanding, :74 internshipProjectReview, :7-16 (hard facts are deterministic, the model only writes the narrative standing and answers the manager's question; falls back to a deterministic standing when unavailable)
- backend/src/scripts/lib/internActivityTracker.js:1-22 per-intern activity snapshot with level thresholds GREEN today, YELLOW 1-3 days, ORANGE 4-6, RED 7-9, BLACK 10+ (Ali policy 2026-05-30)
- backend/src/scripts/lib/internDeliveryCoachNote.js:9-30 computes completion percentage and a projected completion date, DETERMINISTIC ON PURPOSE because a model that nudges a date by a week is not an acceptable failure mode
- backend/src/scripts/lib/internDeliveryMetrics.js, internDeliveryData.js, internDeliveryScope.js, internDeliveryNarrative.js
- internshipRoutes.ts:102 GET /console, :131 GET /console/:enrollmentId, :378 POST /applications/:id/project-review
- backend/src/services/internship/internConsoleDetail.ts getInternConsoleDetail

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Figures match paceFor(), dayOfInternship() and deterministicStanding() exactly; zero dates or percentages originate from the model

When every box above is ticked, stop and show the demo.
