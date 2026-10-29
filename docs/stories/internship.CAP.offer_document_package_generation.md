# internship.CAP.offer_document_package_generation — Offer and onboarding document package generation

An approved applicant receives the correct document package with the right rolling start date, and every signed upload is stored as a new revision so a submitted file is never overwritten.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the template set or the start-date rule

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Offer and onboarding document package generation
**Domain:** Internship & Career Readiness · **Capability:** `internship.offer_document_package_generation`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An approved applicant receives the correct document package with the right rolling start date, and every signed upload is stored as a new revision so a submitted file is never overwritten.
Intended users: applicant, Dhee.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Resolve the template set and compute the rolling start Monday
- Render each document and persist a row with an opaque storage key and checksum
- Serve the generated document to the applicant
- Record the signed upload as a new revision under the uniqueness index
- Expose the document set to the reviewer for verification
**Inputs**
- application id
- template keys
- desired start date
- uploaded signed file
**Outputs**
- internship_documents rows
- rendered PDF
- document revision history
**Exceptions that must be handled**
- A second upload for the same document type creates a revision; overwriting is impossible by index
- The upload allowlist for signed documents must stay its own multer instance; widening a shared allowlist is explicitly warned against
- A render failure must not leave a half-created package; the package is regenerated rather than patched
- The document is never described as a dated or ending programme; this internship is open-ended
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- document_template_render
- sandbox_file_write
- sandbox_database_read
- sandbox_database_write
**Application surfaces**
- student internship documents page
- admin application detail documents panel
- GET /api/admin/internship/applications/:id/documents
- GET /api/admin/internship/documents/:documentId/file
**Source modules**
- backend/src/services/internship/internshipDocumentService.ts
- backend/src/services/internship/internshipDocumentTemplates.ts
- backend/src/services/internship/internshipPdf.ts
- backend/src/models/InternshipDocument.ts
**Dependencies**
- config/upload.ts
- internshipPdf
- internship_document_templates
- internship_documents
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: Rendering a package in sandbox is safe and testable. Generating a real document attaches a legal-looking artifact to a named person, so production generation is not proposed here.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains which documents a given state requires and how the rolling start Monday is derived

Acceptance: nextMonday outputs reproduced exactly for a fixed set of input dates including a Sunday and a Monday boundary
## Before you start
- Dhee has not validated the template set or the start-date rule
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipDocumentService.ts:38 nextMonday (rolling weekly start computed per applicant), :101 generatePackage, :214 readGeneratedDocument, :260 recordSignedUpload, :471 documentsFor
- ensureInternshipSchema.ts:348 uq_internship_document_revision, named at :426 as never overwrite a signed file
- InternshipDocument.ts:70 storage_key (opaque UUID filename, not a path), :71 original_filename, :74 checksum_sha256, :76 status
- config/upload.ts per-surface multer instances with MIME allowlist and opaque filenames; AI_INTERNSHIP_DISCOVERY.md records the warning against widening a shared allowlist
- AI_INTERNSHIP_SPEC.md: rolling weekly start targets the Monday following the application, computed per applicant, and a UI must not render ends for an open-ended apprenticeship
- internshipPdf.ts (render path)

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] nextMonday outputs reproduced exactly for a fixed set of input dates including a Sunday and a Monday boundary

When every box above is ticked, stop and show the demo.
