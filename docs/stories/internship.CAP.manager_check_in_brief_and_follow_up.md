# internship.CAP.manager_check_in_brief_and_follow_up — Manager check-in preparation and follow-up note

Before a check-in, the manager has the intern's real position, what they are working on and the projected completion date, and afterwards a supportive accountability note the manager can post as their own words.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no check-in record model exists
- the Basecamp token acts as Ali, so agent attribution is impossible today
- Dhee has not validated the check-in cadence

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Manager check-in preparation and follow-up note
**Domain:** Internship & Career Readiness · **Capability:** `internship.manager_check_in_brief_and_follow_up`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** existing_application
## The business outcome
Before a check-in, the manager has the intern's real position, what they are working on and the projected completion date, and afterwards a supportive accountability note the manager can post as their own words.
Intended users: Dhee, Ali.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Harvest the intern's delivery snapshot
- Compute completion percentage and projected completion date by arithmetic
- Compose the note with tone branching on the computed status
- Present it to the manager, who posts it as their own
- Record nothing about the meeting itself, because no check-in record exists today
**Inputs**
- intern delivery snapshot
- task completion state
- projected completion arithmetic
- optional manager question
**Outputs**
- check-in brief
- composed Basecamp comment for the manager to post
- narrative standing
**Exceptions that must be handled**
- The projected date and the percentage must stay deterministic; the person is held to them
- The note is the manager's to post; an agent posting it would speak as a named human
- The Basecamp token on the production host acts as Ali, so any automated comment would be attributed to him rather than to an agent
- No check-in or 1:1 record exists, so follow-up completion cannot be claimed as tracked
- House copy rules apply: no em dashes and no double sign-off
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- basecamp_read
- llm_text_generation
- sandbox_database_read
**Application surfaces**
- intern delivery dashboard HTML
- admin Intern Console detail
- GET /api/admin/internship/console/:enrollmentId
- POST /api/admin/internship/applications/:id/project-review
**Source modules**
- backend/src/scripts/lib/internDeliveryCoachNote.js
- backend/src/scripts/lib/internDeliveryNarrative.js
- backend/src/services/internship/internConsoleDetail.ts
- backend/src/services/internship/internshipProjectReview.ts
**Dependencies**
- internDeliveryData
- internDeliveryMetrics
- Basecamp
- internConsoleDetail
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Preparing a brief is safe and already deterministic. Posting the note speaks in a named human's voice through a token that acts as Ali, so shadow mode is the ceiling until an agent-attributed write path exists.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Produces a check-in brief for one intern from stored facts, with every figure traceable

Acceptance: Percentages and dates match internDeliveryCoachNote.js arithmetic exactly; zero figures originate from the model
## Before you start
- no check-in record model exists
- the Basecamp token acts as Ali, so agent attribution is impossible today
- Dhee has not validated the check-in cadence
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internDeliveryCoachNote.js:1-12 composes the Basecamp comment Ali posts on an intern's project list, asked for on 2026-08-25 to support but also outline if things are running behind, naming the part they are working on, where it sits against completion, and the projected completion date
- internDeliveryCoachNote.js:13-30 three reasons it stays deterministic, including that the same snapshot in gives the same note out so the manager can reread and paste it an hour later without the wording shifting
- internDeliveryCoachNote.js:31-37 house style enforced rather than requested, no em dashes
- internConsoleDetail.ts getInternConsoleDetail (one intern in depth: roster row, per-section training breakdown, cert attempt series, readiness and claim pair, activity feed)
- internshipProjectReview.ts:74 answers the manager's own question grounded in deterministic facts
- backend/src/scripts/lib/internDeliveryNarrative.js, renderInternDeliveryEmail.js, internDeliveryEmailSend.js
- no 1:1 or check-in record table was found in this pass, so the follow-up is a composed artifact rather than a tracked meeting

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Percentages and dates match internDeliveryCoachNote.js arithmetic exactly; zero figures originate from the model

When every box above is ticked, stop and show the demo.
