# internship.CAP.da_cohort_analysis_automation — Data-analyst cohort analysis moving to the AI employee

The recurring cohort and funnel analysis a data analyst assembles by hand is produced by the AI employee with its sources cited, freeing analyst time for questions nobody has asked yet.

**Built today:** absent. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no DA deliverable for this domain identified
- Dhee has not named which analyst work should transition

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Data-analyst cohort analysis moving to the AI employee
**Domain:** Internship & Career Readiness · **Capability:** `internship.da_cohort_analysis_automation`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** absent · **Source:** proposed_new
## The business outcome
The recurring cohort and funnel analysis a data analyst assembles by hand is produced by the AI employee with its sources cited, freeing analyst time for questions nobody has asked yet.
Intended users: Dhee, Ali, data analysts.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- unknown until Dhee names the specific analyst deliverables to transition
**Inputs**
- unknown: which DA deliverables are in scope
**Outputs**
- unknown
**Exceptions that must be handled**
- An analysis must cite its source query or dataset; a number without a source is not a deliverable
- A metric whose reliability is unknown must be reported as unknown, not computed anyway
- No existing DA deliverable for the internship domain was identified in this pass, so naming one would be an invention
- Transitioning analyst work must not transition the judgement about what the numbers mean
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- kpi_aggregate_read
- sandbox_database_read
**Application surfaces**
- unknown (no surface exists today)
- unknown (no endpoint exists today)
**Source modules**
- unknown (not implemented)
**Dependencies**
- unknown
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: The owner named DA work transitioning to AI as a source of proposed responsibilities, but no specific internship DA deliverable was identified, so there is nothing to validate and nothing to measure. R0 definition work with at most R2 question-answering is the honest ceiling.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R0)**
Inventories which recurring analyst deliverables touch the internship domain and which are candidates

Acceptance: Dhee names at least one specific deliverable; the inventory lists only artifacts that were observed, with no invented reports
## Before you start
- no DA deliverable for this domain identified
- Dhee has not named which analyst work should transition
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
_none -- which is why this is classified proposed_new_

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Dhee names at least one specific deliverable; the inventory lists only artifacts that were observed, with no invented reports

When every box above is ticked, stop and show the demo.
