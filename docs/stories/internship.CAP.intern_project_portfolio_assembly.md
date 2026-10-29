# internship.CAP.intern_project_portfolio_assembly — Intern project portfolio assembly and demonstration evidence

An intern's build becomes a presentable portfolio of releases and stories with real evidence attached, so what they did can be shown rather than described.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated what belongs in an intern portfolio

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Intern project portfolio assembly and demonstration evidence
**Domain:** Internship & Career Readiness · **Capability:** `internship.intern_project_portfolio_assembly`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern's build becomes a presentable portfolio of releases and stories with real evidence attached, so what they did can be shown rather than described.
Intended users: intern, Dhee, career reviewers.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the intern's projects, releases and stories
- Assemble them into the portfolio shape
- Attach verified evidence and case-study links
- Hand the projection to the readiness scorer and the review queue
**Inputs**
- enrollment id
- projects
- student_task_lists
- student_tasks
- evidence records
**Outputs**
- InternProjectPortfolio with releases and stories
- evidence links
- case-study links
**Exceptions that must be handled**
- Self-reported completion must be distinguishable from verified completion in the portfolio
- An intern with no published build has an empty portfolio, which is an honest state, not an error
- Evidence that cannot be verified must not be presented as verified
- Assembly is a projection; it must not write to the underlying task records
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- portfolio_projection_read
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- student portfolio page
- admin Intern Console project tab
- unknown (portfolio read routes not enumerated in this pass)
**Source modules**
- backend/src/services/internship/internshipProjectPortfolio.ts
- backend/src/services/career/portfolioEvidence.ts
- backend/src/services/career/portfolioOverview.ts
**Dependencies**
- projects
- student_task_lists
- student_tasks
- portfolioEvidence
- timeline/typeRegistry internship_activity card
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: Assembly is a pure projection with no writes, so a separately approved supervised production pilot is reachable. It stays below action automation because everything that makes the portfolio public is the separate publication capability.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Describes what an intern's portfolio contains and which items are verified

Acceptance: Matches assembleProjectPortfolio() on fixtures; self-reported items are never described as verified
## Before you start
- Dhee has not validated what belongs in an intern portfolio
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipProjectPortfolio.ts:42 InternProjectStories, :55 InternProject, :80 InternProjectPortfolio, :98 assembleProjectPortfolio (pure), :155 internProjectPortfolio
- backend/src/services/career/portfolioEvidence.ts, careerEvidenceAdapters.ts, portfolioCaseStudyLinks.ts, portfolioHeroImage.ts, portfolioOverview.ts
- backend/src/services/portfolioGenerationService.ts, portfolioEnhancementService.ts, portfolioShareService.ts
- AI_INTERNSHIP_DISCOVERY.md: the internship_activity curriculum card type is already registered with evidence_required, github_required, instructor_review and portfolio_eligible at timeline/typeRegistry.ts:141
- internshipProjectGeneration.ts:26-30 the three tables an intern project occupies, which is what makes the portfolio render identically to a student's

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches assembleProjectPortfolio() on fixtures; self-reported items are never described as verified

When every box above is ticked, stop and show the demo.
