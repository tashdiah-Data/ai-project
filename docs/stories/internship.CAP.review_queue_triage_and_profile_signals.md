# internship.CAP.review_queue_triage_and_profile_signals — Review queue triage with reliability-labelled profile signals

Applications are bucketed so the reviewer sees what is actually waiting on them, with each supporting signal labelled reliable or unknown rather than silently defaulted.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated what the agent may read
- no PII-minimised agent projection of QueueRow exists

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Review queue triage with reliability-labelled profile signals
**Domain:** Internship & Career Readiness · **Capability:** `internship.review_queue_triage_and_profile_signals`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Applications are bucketed so the reviewer sees what is actually waiting on them, with each supporting signal labelled reliable or unknown rather than silently defaulted.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Partition open applications into state-derived buckets
- Add the two cross-cutting buckets: calls_failed from failed phone sessions, converted from the conversion flag
- Join name and email from enrollments at read time
- Build profile signals with explicit reliability labels
- Withhold the call transcript unless recording consent is present
**Inputs**
- bucket
- limit
- offset
**Outputs**
- QueueRow list
- bucket counts
- ApplicationDetail with person, intake, decisions, timeline, reason options
**Exceptions that must be handled**
- Unknown-reliability signals must be excluded from downstream recommendation, not coerced to zero
- Transcript without consent is withheld at read time, so a new read surface cannot leak it by omission
- reviewer_notes appear in the detail projection and are staff-only; they must never reach applicant-facing text
- No score, rank or inferred trait may be added to the row shape
- A single coarse internship section grant means every holder can reach every endpoint, including decide, verify, activate and the raw signed-document download; the only per-human control today is the audit trail
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
**Application surfaces**
- admin internship review queue
- admin application detail
- GET /api/admin/internship/queue
- GET /api/admin/internship/applications/:id
- GET /api/admin/internship/applications/:id/activity
**Source modules**
- backend/src/services/internship/internshipReviewQueue.ts
**Dependencies**
- internship_applications
- enrollments
- internship_administrative_intakes
- internship_interview_sessions
- internshipRecommendation
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: The queue is the widest applicant-PII read surface in the domain and mgmtRoles.ts calls it a materially wider grant than editing curriculum. Production agent access is not proposed and the ceiling stays at sandbox stateful work.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains bucket membership and which signals are unknown, from approved documentation rather than live rows

Acceptance: Bucket explanations match BUCKET_STATES exactly and no applicant identity is quoted
## Before you start
- Dhee has not validated what the agent may read
- no PII-minimised agent projection of QueueRow exists
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipReviewQueue.ts:89-99 ten QueueBuckets, :105-114 BUCKET_STATES, :125 queueCounts, :153 queue, :338 applicationDetail
- internshipReviewQueue.ts:239 buildProfileSignals with reliability reliable|unknown at :228; attendance_rate hard-coded unknown at :282-290
- internshipReviewQueue.ts:361-376 unknowns passed as exclusions to the recommendation, never as zeros
- internshipReviewQueue.ts:190 Enrollment attributes full_name and email; :399-415 intake projection including phone at :404 and work_auth_category at :410
- internshipReviewQueue.ts:378 and :428-429 transcript consent gate
- mgmtRoles.ts:18-27 internship is its own section key because the queue carries an applicant resume, phone number and interview transcript
- internshipRoutes.ts:311 GET /queue, :335 GET /applications/:id, both requireSection('internship')

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Bucket explanations match BUCKET_STATES exactly and no applicant identity is quoted

When every box above is ticked, stop and show the demo.
