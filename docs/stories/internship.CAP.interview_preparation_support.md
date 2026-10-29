# internship.CAP.interview_preparation_support — Career interview preparation support and nudges

An intern or alumnus with a real interview booked is moved one step down the preparation funnel at a time, and the staff report shows who is least prepared with the least time left.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- live email is a default-denied capability (executionPolicy.ts:33), so production sending requires a separately approved action class
- Dhee has not confirmed this pipeline belongs to the internship domain rather than to placement
- the CCPP view's current column contract was validated in 2026-06 and has not been re-verified in this pass

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Career interview preparation support and nudges
**Domain:** Internship & Career Readiness · **Capability:** `internship.interview_preparation_support`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern or alumnus with a real interview booked is moved one step down the preparation funnel at a time, and the staff report shows who is least prepared with the least time left.
Intended users: intern, mentors, Dhee, placement staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the CCPP upcoming-interviews view
- Classify each row into a funnel stage with a readiness and an urgency
- Select at most one nudge beat per interview per day
- Send the student-facing beat and record the beatKey
- Render the priority-ranked staff report
**Inputs**
- CCPP interview recordset
- days to interview
- preparation score
- auto and mentor mock counts
- survey response state
**Outputs**
- classified interview rows with readiness and priority
- the day's nudge beat per interview
- staff preparation report
**Exceptions that must be handled**
- One beat per interview per day; a new beat fires only on a stage advance or a timeline threshold crossing
- After the interview the messaging must flip from prepare to how did it go, or it reads as ignoring what happened
- The sign-off is the coaching system's, never Ali's signature block
- A missing preparation score with real mock activity must not read as unprepared
- This pipeline is CCPP-sourced and is NOT part of the internship application lifecycle; the link between an internship exit and entering this funnel is the separate handoff capability and does not exist in code
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- ccpp_sql_read
- approved_template_notification_send
- sandbox_email_sink
- structured_output_validation
**Application surfaces**
- interview preparation report HTML
- student email
- CCPP SQL (external, read-only view)
**Source modules**
- backend/src/scripts/lib/interviewPrepData.js
- backend/src/scripts/lib/interviewPrepNudges.js
- backend/src/scripts/lib/renderInterviewPrepReport.js
- backend/src/services/interviewService.ts
**Dependencies**
- CCPP SQL Server view
- Mandrill preflight
- nudge state file
- reportRunRecorder
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R7**.
Why that ceiling: Like the reminder capability, the sending half is an approved-template action class with a one-beat-per-day idempotency key, so R7 is reachable for that class alone once separately approved. R8 and R9 are not proposed, because handing a candidate onward is a human act that ends at recruiters.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains which funnel stage a candidate is in and which beat is next

Acceptance: Stage and beat match interviewPrepData.normalize and interviewPrepNudges on the unit fixtures
## Before you start
- live email is a default-denied capability (executionPolicy.ts:33), so production sending requires a separately approved action class
- Dhee has not confirmed this pipeline belongs to the internship domain rather than to placement
- the CCPP view's current column contract was validated in 2026-06 and has not been re-verified in this pass
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- interviewPrepData.js:3-24 pure classifier over the CCPP view vw_ColaberryInterviewPreparation_UpcomingInterviews, validated against live CCPP 2026-06-10, with the funnel logged, review 10 questions, generate answers, practice, auto mock, mentor mock, interview, post-interview survey
- interviewPrepData.js:26-30 the readiness model blends the app's Preparationscore with auto-mock and mentor-mock counts, so a zero prep score with real activity still reads as activity
- interviewPrepNudges.js:3-22 pure content builder, one beat per interview per day, beatKey stored in state so the same beat is not re-sent, flipping from prepare to how did it go after the interview
- interviewPrepNudges.js:32-34 a shared student-appropriate sign-off, deliberately not Ali's signature block
- backend/src/scripts/lib/interviewPrepPeople.js, renderInterviewPrepReport.js
- backend/src/services/interviewService.ts, backend/src/services/resumeIngestService.ts, resumeHistory.ts, backend/src/services/career/resumeHistoryAdapter.ts

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Stage and beat match interviewPrepData.normalize and interviewPrepNudges on the unit fixtures

When every box above is ticked, stop and show the demo.
