# internship.CAP.standup_attendance_tracking — Standup and required-meeting attendance tracking

Participation in the Monday and Friday scrum is recorded where it can be recorded, and where no denominator exists it is reported as unknown rather than as absence.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no denominator of required meetings is recorded, so a rate cannot be computed
- Dhee has not defined required attendance

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Standup and required-meeting attendance tracking
**Domain:** Internship & Career Readiness · **Capability:** `internship.standup_attendance_tracking`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** existing_application
## The business outcome
Participation in the Monday and Friday scrum is recorded where it can be recorded, and where no denominator exists it is reported as unknown rather than as absence.
Intended users: Dhee, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Ensure the intern is a member of the standup room
- Record a join event keyed on the Central-time date
- Summarise joins per intern
- Report unknown wherever no denominator of required meetings exists
**Inputs**
- enrollment id
- meeting room join events
- required meeting schedule
**Outputs**
- AttendanceSummary
- ConsoleAttendance
- internship_meeting_attendance rows
**Exceptions that must be handled**
- NO DENOMINATOR MEANS UNKNOWN, NOT ZERO. The console deliberately carries no attendance field for this reason and the review queue hard-codes attendance_rate unknown
- A duplicate join on the same day must not inflate the count; the date key is the idempotency boundary
- Central time is the day boundary; a UTC boundary would miscount evening sessions
- Attendance must never be used as an admission signal while it is unreliable
- An agent must never record attendance on an intern's behalf
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- internship_admin_api_read
- meeting_room_read
**Application surfaces**
- admin Intern Console detail
- GET /api/admin/internship/console/:enrollmentId
**Source modules**
- backend/src/services/internship/internshipAttendanceService.ts
- backend/src/services/internship/internshipMeetingRooms.ts
**Dependencies**
- internship_meeting_attendance
- meeting rooms
- internshipCohortService required meetings
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: The data is known-unreliable and the product has already decided not to render it. Until a denominator exists, nothing above sandbox analysis can be justified, because a production attendance report would be a number nobody can defend.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains why attendance is reported as unknown and what would be needed to make it reliable

Acceptance: The explanation states the missing denominator and never quotes an attendance percentage
## Before you start
- no denominator of required meetings is recorded, so a rate cannot be computed
- Dhee has not defined required attendance
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipAttendanceService.ts:30 summarizeAttendance (pure), :41 centralDateKey, :52 recordMeetingJoin, :71 meetingAttendanceSummary, :126 toConsoleAttendance, :138 consoleAttendance
- internshipMeetingRooms.ts:35 INTERNS_ROOM_SLUG ai-internship-standup, :130 ensureInternInStandupRoom, :145 ensureInternshipMeetingRooms
- internshipCohortService.ts:44 RequiredMeeting in the cohort settings
- internshipRoutes.ts:99-100: there is no attendance field in the console response, by product decision, because 7 join rows existed across 2 interns and no denominator exists
- internshipReviewQueue.ts:282-290 attendance_rate is hard-coded unknown and excluded from the recommendation
- AI_INTERNSHIP_SPEC.md: scrum Monday and Friday, project work Tuesday to Thursday
- table: internship_meeting_attendance (ensureInternshipSchema.ts:377)

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] The explanation states the missing denominator and never quotes an attendance percentage

When every box above is ticked, stop and show the demo.
