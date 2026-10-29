# internship.CAP.inactivity_escalation_and_exit_recommendation — Inactivity escalation ladder and exit recommendation

An intern who has gone silent is escalated on a known ladder and, at the exit cliff, surfaced to a human with the exact next step rather than quietly dropped or quietly kept.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant for exit
- Dhee has not validated the ladder thresholds for the internship cohort
- Basecamp token acts as Ali, so an agent write would be attributed to him

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Inactivity escalation ladder and exit recommendation
**Domain:** Internship & Career Readiness · **Capability:** `internship.inactivity_escalation_and_exit_recommendation`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern who has gone silent is escalated on a known ladder and, at the exit cliff, surfaced to a human with the exact next step rather than quietly dropped or quietly kept.
Intended users: Dhee, Ali.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Compute days since last activity per intern from their own comments
- Assign an escalation level on the fixed ladder
- Nudge at YELLOW or worse, honouring the mode file and the per-day state
- At BLACK, send a human the exit-cliff digest with the exact command for each person
- A human confirms the exit; a wrongly flagged intern is reinstated
**Inputs**
- Basecamp activity per intern
- days since last activity
- nudge mode file
- prior nudge state
**Outputs**
- escalation level per intern
- per-level reminder
- exit-cliff digest with the exact CLI per person
- exit or reinstatement record
**Exceptions that must be handled**
- EXIT IS A HUMAN DECISION. The job produces a digest and a command; a person runs the confirmation step
- An intern with no activity data must be UNKNOWN, never BLACK; this exact bug produced false BLACK flags on named people and cost reinstatements
- The nudge state file prevents re-nudging the same intern twice in a day; the mode file can suppress sends entirely
- Reinstatement must restore the intern without erasing the history of the false flag
- An agent must never execute an exit
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- basecamp_read
- basecamp_comment_write
- approved_template_notification_send
- sandbox_email_sink
- internship_admin_api_read
**Application surfaces**
- intern delivery dashboard HTML
- Basecamp intern project comments
- Basecamp API (external)
**Source modules**
- backend/src/scripts/lib/internActivityTracker.js
- backend/src/scripts/dailyInternNudges.js
- backend/src/scripts/lib/internExit.js
- backend/src/scripts/confirmInternExit.js
- backend/src/scripts/lib/internReinstate.js
**Dependencies**
- Basecamp project 24865175
- internExit.js
- internNudgeMode.js
- reportRunRecorder
- Mandrill preflight
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Removing a person from the programme is irreversible in practice and the repo already carries two documented false positives on named interns. The agent may reach shadow mode on the escalation list; the exit act stays with a human permanently.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains the ladder, the UNKNOWN exemption and why exit requires a human confirmation step

Acceptance: Thresholds match internActivityTracker.js:11-17 and the UNKNOWN rule is stated, not omitted
## Before you start
- human-decision invariant for exit
- Dhee has not validated the ladder thresholds for the internship cohort
- Basecamp token acts as Ali, so an agent write would be attributed to him
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internActivityTracker.js:11-17 the five levels and their day thresholds, BLACK at 10+ days being the exit cliff
- internActivityTracker.js:196 a dark intern with no comments is UNKNOWN, not BLACK, and the nudge job skips UNKNOWN
- backend/src/scripts/dailyInternNudges.js:1-9 for BLACK it emails Ali a consolidated digest of who hit the exit cliff today plus the exact CLI commands to process them out
- backend/src/scripts/lib/internExit.js executeExit, backend/src/scripts/confirmInternExit.js (the human confirmation step), internReinstate.js and reinstateInternFalseBlack.js
- backend/src/scripts/reinstateInternFalseBlack.js:3 records a real false BLACK caused by a null-days bug, and reinstateIsaac.js:6 records another on a named person
- backend/src/scripts/weeklyInternReport.js:5-8 leads the report with BLACK and inlines the confirmInternExit CLI for each
- backend/src/scripts/lib/internNudgeMode.js, a mode file that is the source of truth and can suppress sends

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Thresholds match internActivityTracker.js:11-17 and the UNKNOWN rule is stated, not omitted

When every box above is ticked, stop and show the demo.
