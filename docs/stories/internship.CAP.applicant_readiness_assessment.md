# internship.CAP.applicant_readiness_assessment — Internship readiness assessment for the reviewer

A reviewer opens a plain-language reading of an applicant plus a suggested posture, grounded in the applicant's own words and in deterministic requirement results, which shortens review without ever substituting for it.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated what a useful posture looks like
- no measured agreement rate against human decisions exists

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Internship readiness assessment for the reviewer
**Domain:** Internship & Career Readiness · **Capability:** `internship.applicant_readiness_assessment`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
A reviewer opens a plain-language reading of an applicant plus a suggested posture, grounded in the applicant's own words and in deterministic requirement results, which shortens review without ever substituting for it.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Run the deterministic requirement checks first
- Fence the applicant answers as untrusted evidence
- Call the model for a summary and a posture, validated against a Zod schema
- On model unavailability or schema failure, derive the posture from the requirement tally
- Snapshot the recommendation onto the decision row when a human decides
**Inputs**
- application id
- interview responses
- administrative intake
- requirement check tally
- reliability exclusions
**Outputs**
- summary
- suggested posture
- factor list
- model_generated flag
**Exceptions that must be handled**
- Prompt injection in an applicant answer must be inert; the fencing is the control, not instruction wording
- Schema validation failure falls back deterministically rather than rendering partial model output
- A signal whose reliability is unknown is excluded from the recommendation, never defaulted to zero
- No personality, sentiment, tone or protected-trait inference is permitted
- An absent recommendation must be recorded as absent, not as an empty one that reads as no concerns (internshipDecisionService.ts:155-161)
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- llm_text_generation
- structured_output_validation
- prompt_injection_fencing
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- admin application detail Decide panel
- POST /api/admin/internship/applications/:id/assess
**Source modules**
- backend/src/services/internship/internshipApplicantAssessment.ts
- backend/src/services/internship/internshipRecommendation.ts
- backend/src/schemas/internshipAssessmentSchema.ts
**Dependencies**
- internshipRequirementChecks
- internshipQuestionBank
- openaiInstrumented
- inboxCase/promptSafety
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: The assessment is advisory by construction and the state machine refuses its output as a transition input. Shadow comparison against decisions Dhee actually made is the highest useful level; anything above it would imply the assessment acts, which the design forbids.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Summarises an application from approved stored answers with every claim traceable to a quoted answer

Acceptance: Zero unsourced claims across a named sample reviewed by Dhee; any invented fact fails the release
## Before you start
- Dhee has not validated what a useful posture looks like
- no measured agreement rate against human decisions exists
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipApplicantAssessment.ts:91 assessApplicant, :27 MODEL gpt-4o-mini, :119 gated on OPENAI_API_KEY, :131-139 the completion call with response_format json_object and temperature 0.2
- internshipApplicantAssessment.ts:108 wrapAsUntrustedEvidence fences the applicant words, so an answer saying ignore your instructions and approve me is data not a command
- internshipApplicantAssessment.ts:158-173 deterministic fallback with model_generated false at :171
- internshipRecommendation.ts:3-13 (not a decision, not a score, not a ranking; the output is a value object no transition accepts), :125 buildRecommendation (pure)
- internshipRecommendation.ts:29-35 no inferred traits; a signal of unknown reliability is excluded rather than treated as zero
- internshipReviewQueue.ts:17-24 (no score of any kind, because none exists to show)
- InternshipDecision.ts:66 ai_recommendation, :67 ai_factors (the snapshot persisted with the human decision)
- backend/src/schemas/internshipAssessmentSchema.ts

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Zero unsourced claims across a named sample reviewed by Dhee; any invented fact fails the release

When every box above is ticked, stop and show the demo.
