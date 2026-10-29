# internship.CAP.employment_readiness_gap_flag — Daily employment-readiness gap flag as a workforce task

The day's largest employment-readiness gap is raised as a task with an owner, so a career-readiness problem gets a ticket rather than a mention.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated that this flag is worth keeping
- the reliability of the underlying career signal is unknown

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Daily employment-readiness gap flag as a workforce task
**Domain:** Internship & Career Readiness · **Capability:** `internship.employment_readiness_gap_flag`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** inherited
## The business outcome
The day's largest employment-readiness gap is raised as a task with an owner, so a career-readiness problem gets a ticket rather than a mention.
Intended users: Dhee, workforce directors.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- At 06:20 daily, read the career domain signal
- Select the top gap
- Create a WorkforceTask for it
- Take no further action
**Inputs**
- the daily employment-readiness signal for domain career
**Outputs**
- one WorkforceTask naming the top gap
**Exceptions that must be handled**
- No LLM call: the row's own description says so, and adding one would change its character
- A duplicate run on the same day must not create a second task for the same gap
- The flag is a task, not a decision about any named person
- The shared runDomainFlag shape means a change here affects the whole director family, so it is not a local edit
- The underlying signal's reliability was not verified in this pass
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
- workforce_task_write
**Application surfaces**
- unknown (workforce task surface not enumerated in this pass)
- unknown (cron-triggered, no inbound route identified)
**Source modules**
- backend/src/services/workforce/directorActions.ts
- backend/src/services/agentRegistrySeed.ts
- backend/src/services/aiOpsScheduler.ts
**Dependencies**
- directorActions.runDomainFlag
- WorkforceTask
- aiOpsScheduler cron
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: It writes a task, not a change to a person, and already runs deterministically on a cron. A separately approved supervised production pilot under the new employee is reachable, but the shared runDomainFlag shape means the blast radius of a change reaches four sibling directors, so no higher level is proposed.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains what the career director flags, on what schedule, and that it makes no model call

Acceptance: Matches the registry row and directorActions.ts:72 exactly; no claim of analysis the row does not perform
## Before you start
- Dhee has not validated that this flag is worth keeping
- the reliability of the underlying career signal is unknown
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- legacy-slices/internship.json: WorkforceCareerDirector, kind behavior, source registry, cron 20 6 * * * wired at aiOpsScheduler.ts:343, registered at agentRegistrySeed.ts:2224, domain name Career
- agentRegistrySeed.ts:2260-2269 the row: agent_type workforce_director, module workforce, source_file backend/src/services/workforce/directorActions.ts, trigger_type cron, schedule 20 6 * * *, description Jordan Ellis reads the daily employment-readiness signal and flags the gap as a WorkforceTask, No LLM call, enabled true
- backend/src/services/workforce/directorActions.ts:72 runCareerDirector = runDomainFlag('career', 'WorkforceCareerDirector', 'career'), :215 registered in the director dispatch table
- it is one of a family of identically shaped director rows (curriculum, certification, finance, student success), so the behaviour is a shared runDomainFlag and not career-specific logic

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches the registry row and directorActions.ts:72 exactly; no claim of analysis the row does not perform

When every box above is ticked, stop and show the demo.
