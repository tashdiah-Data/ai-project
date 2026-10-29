# internship.CAP.deterministic_requirement_checks — Deterministic eligibility and commitment requirement checks

The eight hard requirements a reviewer trusts are computed by arithmetic over the applicant's own quoted answers, never by a model, so the green and red a reviewer acts on cannot be hallucinated.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the eight rules as the current requirement set

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Deterministic eligibility and commitment requirement checks
**Domain:** Internship & Career Readiness · **Capability:** `internship.deterministic_requirement_checks`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
The eight hard requirements a reviewer trusts are computed by arithmetic over the applicant's own quoted answers, never by a model, so the green and red a reviewer acts on cannot be hallucinated.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Load the applicant answer map
- Evaluate each of the eight rules as a yes/no or choice comparison
- Attach the applicant quoted answer to each result as evidence
- Return a tally the reviewer panel and the AI assessment fallback both consume
**Inputs**
- answer map keyed by question_key
**Outputs**
- RequirementCheck list with status and quoted evidence
- requirement tally
**Exceptions that must be handled**
- A missing or unparseable answer yields unclear, never not_met; an applicant is not failed for a question we failed to collect
- employment_status of full_time yields not_met but is a blocking flag for a human, never an automatic rejection
- A rule change must version with the question bank or historical checks silently re-score
- The checks must never be presented as a score or a rank
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- structured_output_validation
- internship_admin_api_read
**Application surfaces**
- admin application detail Decide panel
- POST /api/admin/internship/applications/:id/assess
**Source modules**
- backend/src/services/internship/internshipRequirementChecks.ts
**Dependencies**
- internshipQuestionBank
- internship_interview_responses
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: The computation is pure, deterministic and read-only, so a separately approved supervised production pilot is reachable on evidence. It never writes and never decides, which is what keeps it below the action-automation levels.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
States which of the eight requirements an application meets and quotes the answer behind each

Acceptance: Zero disagreements against checkRequirements() across a named fixture set, and every unclear accompanied by the missing key
## Before you start
- Dhee has not validated the eight rules as the current requirement set
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipRequirementChecks.ts:5-11 (no model, no interpretation)
- internshipRequirementChecks.ts:68 checkRequirements, :90 requirementTally, :13 RequirementStatus met|not_met|unclear
- the eight rules at :70-85: not_employed_fulltime, weekly_hours (25+), attend_meetings, will_notify_absence, has_tools, understands_costs, protects_secrets, agrees_expectations
- each check carries the applicant quoted answer as evidence (:21, :60)
- AI_INTERNSHIP_SPEC.md: eligibility gate is not currently employed full-time; the attestation and the commitment are the two things the email intake collects
- InternshipApplication.ts:69 attests_not_employed_fulltime, :70 commitment_acknowledged_at

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Zero disagreements against checkRequirements() across a named fixture set, and every unclear accompanied by the missing key

When every box above is ticked, stop and show the demo.
