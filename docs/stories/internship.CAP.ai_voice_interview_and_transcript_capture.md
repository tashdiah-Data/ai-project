# internship.CAP.ai_voice_interview_and_transcript_capture — AI voice interview with dual consent, transcript capture and answer extraction

An applicant who prefers to talk completes the same interview by phone, with permission to call and permission to record held as separate consents, and the transcript converted into the same keyed answers a form would produce.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated scope
- no sandbox voice endpoint identified
- SYNTHFLOW_INTERNSHIP_AGENT_ID provisioning state unknown

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# AI voice interview with dual consent, transcript capture and answer extraction
**Domain:** Internship & Career Readiness · **Capability:** `internship.ai_voice_interview_and_transcript_capture`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An applicant who prefers to talk completes the same interview by phone, with permission to call and permission to record held as separate consents, and the transcript converted into the same keyed answers a form would produce.
Intended users: applicant, Dhee.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Check permission_to_call; refuse with a student-safe message if absent
- Build the call prompt from the active question bank
- Trigger the voice call through the dedicated internship agent slot, or skip if the slot is unset
- On completion, store outcome and transcript and run reconcile for calls that never reported back
- Extract answers from the transcript into the same question_key shape the form writes
**Inputs**
- application id
- intake phone
- permission_to_call
- consent_recording
- question bank facts
**Outputs**
- internship_interview_sessions row with outcome and transcript
- extracted keyed answers
- call failure bucket in the review queue
**Exceptions that must be handled**
- No permission to call: skip with reason no_permission_to_call, never dial
- Recording not consented: the transcript is withheld at read time, not just at storage
- Agent slot unset: skip the call rather than dialling through an agent carrying a conflicting saved script
- Call never reports completion: reconcile resolves it into a failed bucket instead of leaving the application stuck
- Extraction ambiguity must leave the answer unset rather than guess a requirement answer
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- voice_call_trigger_sandbox
- transcript_extraction
- sandbox_database_read
- sandbox_database_write
- structured_output_validation
**Application surfaces**
- admin application detail call panel
- review queue calls_failed bucket
- unknown (call trigger and webhook routes not enumerated in this pass)
**Source modules**
- backend/src/services/internship/internshipCallService.ts
- backend/src/services/internship/internshipCallPrompt.ts
- backend/src/services/internship/internshipCallCompletion.ts
- backend/src/services/internship/internshipCallReconcile.ts
- backend/src/services/internship/internshipTranscriptExtraction.ts
**Dependencies**
- synthflowService
- internship_interview_sessions
- internshipQuestionBank
- SYNTHFLOW_INTERNSHIP_AGENT_ID
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: Placing a real call to a real applicant is an external side effect on a person holding two separate consents. Sandbox exercise is legitimate; production dialling is not proposed at any level in this task.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains the consent model and why an unset agent slot results in no call

Acceptance: Explanation matches internshipCallService.ts:19-26 with no claim that a fallback agent exists
## Before you start
- Dhee has not validated scope
- no sandbox voice endpoint identified
- SYNTHFLOW_INTERNSHIP_AGENT_ID provisioning state unknown
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipCallService.ts:19 (no permission to call means no call), :74 permission_to_call guard, :113 recording_consented, :127 triggerVoiceCall
- internshipCallService.ts:26 SYNTHFLOW_INTERNSHIP_AGENT_ID unset means triggerVoiceCall skips rather than borrowing another agent script
- internshipCallService.ts:62 callNow, :234 scheduleCall, :279 cancelScheduledCall
- internshipCallCompletion.ts:48 handleInternshipCallCompleted; internshipCallReconcile.ts:35 reconcileInternshipCall
- internshipTranscriptExtraction.ts:50 parseTurns, :172 extractAnswersFromTranscript
- internshipCallPrompt.ts:60 buildInternshipCallPrompt
- InternshipInterviewSession.ts:53 transcript TEXT column
- internshipReviewQueue.ts:378 consent gate, :428-429 transcript withheld when consent_recording is false
- AI_INTERNSHIP_DISCOVERY.md risk R4: dedicated env agent slot; unset means skip the call, never fall back

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Explanation matches internshipCallService.ts:19-26 with no claim that a fallback agent exists

When every box above is ticked, stop and show the demo.
