# internship.CAP.onboarding_checklist_tracking — Document and onboarding checklist tracking

Both the intern and the manager see exactly which onboarding step is outstanding and whose move it is, so waiting on Colaberry never reads as the intern being late.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated the checklist step set or the actor split

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Document and onboarding checklist tracking
**Domain:** Internship & Career Readiness · **Capability:** `internship.onboarding_checklist_tracking`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
Both the intern and the manager see exactly which onboarding step is outstanding and whose move it is, so waiting on Colaberry never reads as the intern being late.
Intended users: intern, Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Gather evidence for each checklist step from documents, acknowledgements and membership
- Resolve each step to done or outstanding with its owning actor
- Compute activation blockers and the single next student action
- Render the student Today card copy for the resolved state
- Surface the same checklist to the manager on the application detail
**Inputs**
- application id
- checklist evidence (documents, acknowledgements, membership, payment)
**Outputs**
- ChecklistStepStatus list with actor and completion
- activation blockers
- next student action
- attention queue
**Exceptions that must be handled**
- Steps owned by Colaberry must render as ours and must never be shown as student lateness
- An unknown intern week yields an honest unknown phase rather than a late reading
- The Today card is feature-flag gated; with the flag off nothing renders
- Only the eligible card state is dismissible; a blocking state cannot be dismissed away
- An agent must never mark a checklist step done on a real applicant's behalf
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
**Application surfaces**
- student Today card
- student internship dashboard
- admin application detail onboarding panel
- GET /api/admin/internship/applications/:id/onboarding
**Source modules**
- backend/src/services/internship/internshipOnboarding.ts
- backend/src/services/internship/internshipActivationService.ts
- backend/src/services/internship/internshipStudentDashboard.ts
- backend/src/services/internship/internshipEligibility.ts
- backend/src/services/internship/internshipWeek3Handoff.ts
**Dependencies**
- internship_documents
- internship_requirement_acknowledgements
- cohort_memberships
- internshipEligibility
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: The checklist is a pure read-and-resolve over evidence another capability writes. It changes nothing, so a separately approved supervised production pilot of the reporting view is reachable on measured evidence.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
States which onboarding step is outstanding and whose move it is, for a named application

Acceptance: Matches resolveChecklist() and nextStudentAction() on every fixture; a Colaberry-owned step is never reported as the intern being late
## Before you start
- Dhee has not validated the checklist step set or the actor split
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipOnboarding.ts:34 ChecklistActor student|colaberry, :58 CHECKLIST, :175 resolveChecklist, :224 activationBlockers, :235 nextStudentAction, :239 checklistProgress
- internshipActivationService.ts:265 buildChecklist, :339 recordAcknowledgement, :387 ActiveInternView, :407 activeInternView
- internshipStudentDashboard.ts:58 deriveAttentionQueue, :66 internDashboard
- internshipEligibility.ts:76 cardStateFor, :100 isStudentActionable, :105 cardCopy, :156 shouldRenderCard (feature-flag gated at :162)
- internshipWeek3Handoff.ts:11-18: two phases are Colaberry's to move and carry owner colaberry with actionable false, because waiting on an assignment must not count as student lateness
- internshipRoutes.ts:823 GET /applications/:id/onboarding
- table: internship_requirement_acknowledgements (ensureInternshipSchema.ts:289)

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches resolveChecklist() and nextStudentAction() on every fixture; a Colaberry-owned step is never reported as the intern being late

When every box above is ticked, stop and show the demo.
