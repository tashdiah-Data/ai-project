# internship.CAP.active_project_drift_detection — Active project drift detection

An intern whose portal points at one project while their work is in another is found before they lose a day wondering why verified work shows zero.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated who receives a drift finding

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Active project drift detection
**Domain:** Internship & Career Readiness · **Capability:** `internship.active_project_drift_detection`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern whose portal points at one project while their work is in another is found before they lose a day wondering why verified work shows zero.
Intended users: Dhee, internship-section staff, platform staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the enrollment's active project pointer and every project's activity
- Detect the drift codes the module defines
- Report the finding to a human
- Take no corrective action
**Inputs**
- enrollment id
- active_project_id
- per-project activity
**Outputs**
- DriftFinding with a drift code, or null
**Exceptions that must be handled**
- IT ONLY REPORTS. Auto-correcting to the newest project is demonstrably wrong against live data and must not be added
- A null active_project_id is a distinct finding, not an error to swallow
- A finished newer project must not displace an in-progress older one
- An agent must never write enrollments.active_project_id
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- unknown (no dedicated admin surface identified in this pass)
- unknown (no endpoint identified in this pass)
**Source modules**
- backend/src/services/sbp/activeProjectDrift.ts
- backend/src/services/sbp/activeProjectDriftService.ts
**Dependencies**
- enrollments
- projects
- student_tasks
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: The module is pure and report-only by deliberate design. A supervised production pilot of the report is reachable; raising the agent to an action level would mean writing the pointer, which the module's own header forbids.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains the drift codes and why auto-correction is refused

Acceptance: Matches DriftCode and the module header; never proposes a correction
## Before you start
- Dhee has not validated who receives a drift finding
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- activeProjectDrift.ts:1-23 the failure it exists to catch, found on a named student on 2026-09-05 and repaired by hand once before on 2026-08-25, so twice is a pattern
- activeProjectDrift.ts:25-33 WHY THIS ONLY REPORTS AND NEVER CORRECTS: point them at their newest project is wrong, and the live data says so
- activeProjectDrift.ts:63 DriftCode, :71 DriftFinding, :100 detectActiveProjectDrift (pure)
- activeProjectDriftService.ts:57 driftForEnrollment
- INTENDED_INTERN_ROSTER_2026-10-07.md records Ruth Kumera with enrollments.active_project_id NULL, so the null case is live

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches DriftCode and the module header; never proposes a correction

When every box above is ticked, stop and show the demo.
