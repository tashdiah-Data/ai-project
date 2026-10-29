# internship.CAP.existing_intern_conversion_planning — Existing-intern conversion planning with a zero-write dry run

Interns who predate the portal are brought onto the managed lifecycle from a plan a human has read, with the dry run guaranteed to write nothing and send nothing.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant for commit
- Dhee has not validated the conversion plan shape

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Existing-intern conversion planning with a zero-write dry run
**Domain:** Internship & Career Readiness · **Capability:** `internship.existing_intern_conversion_planning`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Interns who predate the portal are brought onto the managed lifecycle from a plan a human has read, with the dry run guaranteed to write nothing and send nothing.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Plan the conversion and return the plan object with no transaction opened
- A human reads the plan row by row
- Commit applies the plan and records the report
- Converted applications appear in the converted bucket
**Inputs**
- existing intern identifiers
- requirement and document expectations
- commit confirmation
**Outputs**
- ConversionPlan (dry run, zero writes)
- ConversionReport on commit
- converted_from_existing_intern flag
**Exceptions that must be handled**
- THE DRY RUN MUST WRITE NOTHING AND SEND NOTHING; a test asserts zero writes and zero sends
- COMMIT IS A HUMAN ACT after reading the plan
- A re-run of commit must be idempotent; a converted intern must not be converted twice
- Ruth Kumera must not be swept into a conversion that advances her application state
- Imported signature documents must be recorded as imported, not as newly signed
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- sandbox_database_write
**Application surfaces**
- admin internship conversion panel
- POST /api/admin/internship/conversion/plan
- POST /api/admin/internship/conversion/commit
**Source modules**
- backend/src/services/internship/internshipConversionService.ts
**Dependencies**
- internship_applications
- internship_documents
- internship_requirement_acknowledgements
- cohort_memberships
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: The dry run is a zero-write sandbox-safe operation, which is exactly R4 work. Commit changes the lifecycle state of real people in bulk, which is the highest-blast-radius write in the domain and stays with a human.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains what a conversion plan contains and why the dry run cannot write

Acceptance: Matches ConversionPlan and the no-transaction guarantee; never claims a conversion was applied
## Before you start
- human-decision invariant for commit
- Dhee has not validated the conversion plan shape
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipConversionService.ts:124 planConversion, :350 commitConversion, :91 ConversionPlan, :68 ConversionPlanRow, :113 importedSignatureDocumentTypes, :330 ConversionReport
- internshipRoutes.ts:935 POST /conversion/plan, :968 POST /conversion/commit, both requireSection('internship')
- AI_INTERNSHIP_DISCOVERY.md risk R8: existing-intern conversion writes during dry-run, mitigated because the dry run returns a plan object and takes no transaction, with a test asserting zero writes and zero sends
- internshipReviewQueue.ts:146-148 the converted bucket is driven by the converted_from_existing_intern flag; InternshipApplication.ts:72 is the column
- INTENDED_INTERN_ROSTER_2026-10-07.md: 21 application rows measured on 2026-10-08, distributed 13 active, 4 started, 1 withdrawn, 1 under_review, 1 offer_letter_ready, 1 signed_documents_uploaded, with 13 distinct active interns in the ai_internship cohort

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches ConversionPlan and the no-transaction guarantee; never claims a conversion was applied

When every box above is ticked, stop and show the demo.
