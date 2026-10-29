# internship.CAP.two_channel_structured_interview — Two-channel structured interview over one versioned question bank

Form and phone applicants answer the same canonical questions under the same keys, so a reviewer compares like with like and no channel collects less than the other.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the question bank scope

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Two-channel structured interview over one versioned question bank
**Domain:** Internship & Career Readiness · **Capability:** `internship.two_channel_structured_interview`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Form and phone applicants answer the same canonical questions under the same keys, so a reviewer compares like with like and no channel collects less than the other.
Intended users: applicant, Dhee.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Resolve the active question set version for the application
- Serve ordered questions by section; remainingQuestions and nextQuestion drive the form
- Validate each answer against its declared answer type before persisting
- Persist one response per question_key, enforced by a unique index
- completeInterviewIfDone advances the application when the required set is answered
**Inputs**
- application id
- question set version
- answers keyed by question_key
**Outputs**
- internship_interview_responses rows
- InterviewProgress
- reviewer-facing SummaryLine list
**Exceptions that must be handled**
- Re-answering the same question must update, never duplicate (unique index)
- An invalid answer type is rejected before persistence rather than stored and re-interpreted later
- A question set version change must not retroactively invalidate answers stored under the prior version
- Phone and form key sets must stay equal; divergence is a contract violation, not a cosmetic difference
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- sandbox_database_write
- structured_output_validation
**Application surfaces**
- student internship interview form
- admin application detail interview section
- unknown (participant-facing interview routes not enumerated in this pass)
**Source modules**
- backend/src/services/internship/internshipQuestionBank.ts
- backend/src/services/internship/internshipInterviewService.ts
- backend/src/models/InternshipInterviewSession.ts
- backend/src/models/InternshipInterviewResponse.ts
**Dependencies**
- internship_applications
- internshipAnalytics
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: Authoring and validating question sets is sandbox-safe work. Writing an applicant's own answers in production is the applicant's act, not the agent's, so R4 is the ceiling.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Answers which questions exist, in which section order, and which keys are required for completion

Acceptance: Answers match activeQuestionKeys() exactly for ten sampled questions; evaluator is a diff against the module
## Before you start
- Dhee has not validated the question bank scope
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipQuestionBank.ts:41 ACTIVE_QUESTION_SET_VERSION, :99 QUESTION_BANK_V1, :311 questionBank(version), :358 isValidAnswer
- internshipInterviewService.ts:132 openSession, :160 saveAnswers, :220 markInterviewStarted, :250 completeInterviewIfDone, :295 buildSummary
- ensureInternshipSchema.ts:260 uq_internship_response_per_question, named at :425 as the ask-once guarantee
- AI_INTERNSHIP_DISCOVERY.md risk R7: one versioned question bank; both channels write the same question_key
- tables: internship_interview_question_sets, internship_interview_questions, internship_interview_sessions, internship_interview_responses

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Answers match activeQuestionKeys() exactly for ten sampled questions; evaluator is a diff against the module

When every box above is ticked, stop and show the demo.
