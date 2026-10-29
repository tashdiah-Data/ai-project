# internship.CAP.manager_surface_access_verification — Verifying the accountable manager can actually reach the agent surface

Dhee can open the AI employee's detail and talk surfaces under a scoped role, so accountability is exercisable rather than nominal.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- requireAgentManagerOrAdmin is proposed and not built
- no org_members.id is present in the JWT on any login path today
- verification that Dhee opened the surface has not happened

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Verifying the accountable manager can actually reach the agent surface
**Domain:** Internship & Career Readiness · **Capability:** `internship.manager_surface_access_verification`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** manager_defined
## The business outcome
Dhee can open the AI employee's detail and talk surfaces under a scoped role, so accountability is exercisable rather than nominal.
Intended users: Dhee, Ali.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Confirm the org_members row exists and matches the admin login email
- Point the employee's reports_to at that org_members id
- Grant the narrowest mgmt_role that reaches the internship section
- Verify by having the manager open the surface, not by asserting the grant
**Inputs**
- admin_users login for dhee@colaberry.com
- org_members row bbf01827-c005-5cb2-8a96-903fe8ddadac
- mgmt_role assignment
- agent reports_to chain
**Outputs**
- a verified chain from the agent to one real human
- a recorded statement of which surfaces the manager can reach
**Exceptions that must be handled**
- Manager instructions may only RESTRICT an agent, never expand its authority
- Every management action must be authorized server-side against a real is-this-human-the-manager check, never frontend visibility alone
- No manager-versus-admin distinction exists today; until requireAgentManagerOrAdmin ships, any admin can act on any agent, and that must be stated rather than assumed away
- An agent must never hold accountable or approver for itself in either direction
- A grant is not verification; Phase 5 must confirm the manager actually opened the surface
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- admin Agent Detail
- admin Agent Talk tab
- admin internship section
- unknown (agentDetailRoutes and agentGovernanceRoutes use requireAdmin today)
**Source modules**
- backend/src/services/access/mgmtRoles.ts
- backend/src/middlewares/authMiddleware.ts
- backend/src/services/orgChartHierarchyService.ts
**Dependencies**
- org_members
- admin_users
- mgmtRoles
- orgChartHierarchyService.isAgentInHumanDownstream
- ai_agents.reports_to_id
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: This is the governance prerequisite for every other capability here. Granting or changing a role is an authorization change, which no agent may make at any level, so the ceiling is explaining the chain and its gap.

**THIS IS A HUMAN DECISION.** It stays human-required at every autonomy level, forever. An
agent may prepare, summarise and propose; it may never decide. No amount of evidence
promotes this into automation.
**First release behaviour (autonomy:R2)**
Explains the manager chain, which surfaces the manager can reach, and the manager-versus-admin gap that still exists

Acceptance: Names the requireAdmin-only state of the agent routes and does not claim a manager-scoped gate that has not shipped
## Before you start
- requireAgentManagerOrAdmin is proposed and not built
- no org_members.id is present in the JWT on any login path today
- verification that Dhee opened the surface has not happened
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- HUMAN_OWNERSHIP_MAP.md row 7: Internship and Career Readiness, Dhee, has an admin login dhee@colaberry.com with role admin but had no org_members row, so reports_to could not point at Dhee
- HUMAN_OWNERSHIP_MAP.md answer 3: one additive org_members row approved, to be created when the Internship employee's Phase 1 begins
- execution-contract.md:275 the domain table row, and :308-310 A5 the Internship org_members row for Dhee may be created because Ali pre-approved it; one row, additive
- MANAGER_AUTHORIZATION_MAP.md point 2: both agent-detail and agent-governance routes use only requireAdmin today, so any admin can act on any agent
- MANAGER_AUTHORIZATION_MAP.md proposed requireAgentManagerOrAdmin over OrgMember.email equals AuthPayload.email plus isAgentInHumanDownstream (orgChartHierarchyService.ts:110-141), NOT built and NOT authorized to build
- MANAGER_AUTHORIZATION_MAP.md: initial management capability stays admin-only until requireAgentManagerOrAdmin ships and is verified
- mgmtRoles.ts admissions role sections are dashboard, lead_ingestion and internship, the narrowest existing role that fits Dhee

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Names the requireAdmin-only state of the agent routes and does not claim a manager-scoped gate that has not shipped

When every box above is ticked, stop and show the demo.
