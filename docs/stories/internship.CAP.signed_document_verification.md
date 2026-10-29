# internship.CAP.signed_document_verification — Signed document verification by a human reviewer

Documents an applicant has already submitted are checked by a named person before any entitlement moves, so onboarding cannot be completed by a system that only saw a file arrive.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant
- Dhee has not validated what a completeness pre-check may surface

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Signed document verification by a human reviewer
**Domain:** Internship & Career Readiness · **Capability:** `internship.signed_document_verification`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Documents an applicant has already submitted are checked by a named person before any entitlement moves, so onboarding cannot be completed by a system that only saw a file arrive.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Open the submitted document from the internship document route
- Verify or reject it, recording the verifier and the reason
- Recompute outstanding requirements
- When all documents are verified, the reviewer moves the application to documents_verified
- A rejected document returns the application to offer_letter_ready for re-upload
**Inputs**
- document id
- verify or reject
- rejection reason
- verifier identity from the session
**Outputs**
- document status with verified_by and verified_at
- outstanding requirement list
- state transition to documents_verified or back to offer_letter_ready
**Exceptions that must be handled**
- DOCUMENT VERIFICATION IS UNCONDITIONALLY HUMAN: no system actor edge exists out of signed_documents_uploaded
- A rejection must carry a reason; a silent rejection leaves the applicant unable to act
- Re-upload after rejection creates a revision; the rejected file is retained for audit
- Ruth Kumera must not be touched or advanced by this programme
- An agent must never mark a requirement verified or granted for any applicant
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- document_metadata_read
**Application surfaces**
- admin application detail documents panel
- POST /api/admin/internship/documents/:documentId/verify
- GET /api/admin/internship/documents/:documentId/file
**Source modules**
- backend/src/services/internship/internshipDocumentService.ts
- backend/src/services/internship/internshipStateMachine.ts
**Dependencies**
- internship_documents
- internshipApplicationService.transition
- internshipOnboarding checklist
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Verification is the gate between a submitted file and a granted entitlement, and the state machine admits no automated actor. Shadow completeness checks are the highest useful level; the verification act is reserved to a human permanently.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Lists which document types are outstanding for an application without opening any file content

Acceptance: Outstanding list matches outstandingRequirements(); no document bytes are read
## Before you start
- human-decision invariant
- Dhee has not validated what a completeness pre-check may surface
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipDocumentService.ts:405 verifyDocument, :349 outstandingRequirements, :332 RequirementStatus
- internshipStateMachine.ts:191-199: every outbound edge of signed_documents_uploaded is reviewer-only; no system actor appears on any of them
- internshipStateMachine.ts:192 to documents_verified reviewer only, :196 back to offer_letter_ready for a correction loop, reviewer only
- InternshipDocument.ts:78 verified_by, :79 verified_at, :80 rejection_reason
- internshipRoutes.ts:780 POST /documents/:documentId/verify requireSection('internship')
- INTENDED_INTERN_ROSTER_2026-10-07.md: Ruth Kumera is at signed_documents_uploaded, documents submitted and awaiting staff verification, and must not be advanced

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Outstanding list matches outstandingRequirements(); no document bytes are read

When every box above is ticked, stop and show the demo.
