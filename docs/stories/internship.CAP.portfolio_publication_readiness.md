# internship.CAP.portfolio_publication_readiness — Portfolio and demonstration publication readiness

An intern's portfolio is scored for whether it has enough substance to be worth a human review, and publication happens only after a reviewer approves it.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- human-decision invariant for publication
- Dhee has not validated the readiness policy for interns specifically

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Portfolio and demonstration publication readiness
**Domain:** Internship & Career Readiness · **Capability:** `internship.portfolio_publication_readiness`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
An intern's portfolio is scored for whether it has enough substance to be worth a human review, and publication happens only after a reviewer approves it.
Intended users: Dhee, career reviewers, mentors, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Build the portfolio projection from project and case-study evidence
- Compute readiness against the configurable policy
- The intern requests review once readiness allows it
- A career reviewer approves or requests changes
- On approval the public snapshot becomes reachable at the talent slug
**Inputs**
- enrollment id
- portfolio projection
- readiness policy
- reviewer decision
**Outputs**
- readiness score and unmet requirements
- review queue item
- approved or changes-requested decision
- public frozen snapshot on approval
**Exceptions that must be handled**
- PUBLICATION IS A HUMAN DECISION. decidePortfolioReview is a reviewer act and the public snapshot follows it
- Portfolio readiness must never be relabelled job readiness or employability; the module forbids it explicitly
- The reviewer gate decides who may enter, not whose portfolios they see; scope is applied per query
- The readiness bar is a policy argument so it can move without a code change; a hardcoded threshold would be a regression
- An agent must never set portfolio visibility or approve a review
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- portfolio_projection_read
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- student portfolio page
- admin portfolio review queue
- public talent page
- GET /api/public/talent/:slug
- unknown (career review routes not enumerated in this pass)
**Source modules**
- backend/src/services/career/careerReadiness.ts
- backend/src/services/career/careerPortfolioPageWriteService.ts
- backend/src/middlewares/requireCareerReviewer.ts
- backend/src/services/career/careerPortfolioPublicProjection.ts
**Dependencies**
- careerPortfolioPageService
- careerMentorScopeService
- portfolioEvidence
- career_review section
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Publication makes a named person's work public under their own slug. The agent may reach shadow mode on the readiness score and the queue ordering; approval and visibility stay with a career reviewer permanently.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains which readiness requirements an intern's portfolio does not yet meet, and refuses to call anyone job ready

Acceptance: Unmet list matches computeReadiness() under DEFAULT_POLICY; any employability language fails the release
## Before you start
- human-decision invariant for publication
- Dhee has not validated the readiness policy for interns specifically
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- careerReadiness.ts:2 Portfolio Publication Readiness, :6-9 it is a POLICY not a hardcoded threshold with DEFAULT_POLICY at :44, :73 computeReadiness, :17-19 deterministic and pure, no AI
- careerReadiness.ts:11-15 IT IS NOT JOB READINESS: nothing in this file and nothing that renders it may describe a person as employable
- careerPortfolioPageWriteService.ts:156 requestPortfolioReview, :195 PortfolioDecision approved|changes_requested, :208 decidePortfolioReview, :293 listPortfolioReviewQueue, :141 setPortfolioVisibility
- backend/src/middlewares/requireCareerReviewer.ts:8-22 the gate on the portfolio review surface, wider than requireAdmin so mentors get in, with careerMentorScopeService deciding WHOSE portfolios per query; clients and employers never reach it and see only the public frozen snapshot at /api/public/talent/:slug
- mgmtRoles.ts career_review section key (portfolio publication review queue, admins plus mentors)
- backend/src/services/career/careerPortfolioPublicProjection.ts, portfolioEvidence.ts, portfolioNarrativeService.ts

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Unmet list matches computeReadiness() under DEFAULT_POLICY; any employability language fails the release

When every box above is ticked, stop and show the demo.
