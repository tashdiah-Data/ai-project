# internship.CAP.cohort_kpi_reporting — Internship cohort KPI and funnel reporting

The programme's funnel and health are reported as numbers with their reliability stated, so a decision about the internship rests on measured volume rather than impression.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated which KPIs matter
- no accepted threshold exists for any KPI

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Internship cohort KPI and funnel reporting
**Domain:** Internship & Career Readiness · **Capability:** `internship.cohort_kpi_reporting`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
The programme's funnel and health are reported as numbers with their reliability stated, so a decision about the internship rests on measured volume rather than impression.
Intended users: Dhee, Ali, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Aggregate application states and activity into KPI rows
- Label each metric reliable or unknown
- Render the weekly report with its buckets
- Post to the message board and email the standard recipients
**Inputs**
- application states
- cohort membership
- activity buckets
- analytics events
**Outputs**
- InternshipKpi list with reliability labels
- weekly activity report
- console counts
**Exceptions that must be handled**
- Every metric carries a reliability label; an unknown metric must not be rendered as a number
- A report must not quote a count it did not measure in that run
- Aggregates must not become re-identifiable at small cohort sizes; the weekly report names individuals, so it is not aggregate-only
- The weekly report send must honour its skip flag and the kill switch
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- kpi_aggregate_read
- basecamp_read
- approved_template_notification_send
**Application surfaces**
- admin internship KPI view
- Basecamp intern message board
- weekly report email
- GET /api/admin/internship/kpis
- GET /api/admin/internship/profile/:enrollmentId
**Source modules**
- backend/src/services/internship/internshipTrackingService.ts
- backend/src/services/internship/internshipAnalytics.ts
- backend/src/scripts/weeklyInternReport.js
**Dependencies**
- internship_applications
- cohort_memberships
- Basecamp message board
- reportRunRecorder
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: Reporting reads and aggregates and writes nothing to a person's record, so a separately approved supervised production pilot is reachable. The weekly report names individuals and is sent to staff, so the sending half would need its own authorized action class before any higher level.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Reports the funnel counts with reliability labels and refuses to state an unmeasured number

Acceptance: Counts match internshipKpis(); every unknown is labelled unknown rather than rendered as zero
## Before you start
- Dhee has not validated which KPIs matter
- no accepted threshold exists for any KPI
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipTrackingService.ts:39 Reliability reliable|unknown, :41 TrackedMetric, :134 internshipProfileSection, :342 InternshipKpi, :359 internshipKpis
- internshipAnalytics.ts:39 InternshipAnalyticsEvent, :84 sanitizeMeta, :104 emitInternshipEvent
- internConsoleRoster.ts:386 consoleCounts, :373 ConsoleCounts
- internshipRoutes.ts:887 GET /kpis, :902 GET /profile/:enrollmentId
- backend/src/scripts/weeklyInternReport.js:3-8 the weekly activity report with STRONG, LIGHT and INACTIVE buckets over the last ten days, posted to a Basecamp message board and emailed
- reportingRegistry.js:133-144 the weekly report runs Saturday 13:00 UTC with STANDARD_RECIPIENTS and a skip flag

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Counts match internshipKpis(); every unknown is labelled unknown rather than rendered as zero

When every box above is ticked, stop and show the demo.
