# internship.CAP.intern_project_to_catalog_matching — Project matching recommendation against a project catalog

An intern is matched to the project that best fits their skills, interests and the organisation's needs, instead of having a project generated from scratch every time.

**Built today:** absent. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no project catalog exists in the application
- Dhee has not said whether matching is wanted instead of generation

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Project matching recommendation against a project catalog
**Domain:** Internship & Career Readiness · **Capability:** `internship.intern_project_to_catalog_matching`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** absent · **Source:** proposed_new
## The business outcome
An intern is matched to the project that best fits their skills, interests and the organisation's needs, instead of having a project generated from scratch every time.
Intended users: Dhee, internship-section staff.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- unknown until Dhee defines whether a project catalog should exist at all
**Inputs**
- unknown
**Outputs**
- unknown
**Exceptions that must be handled**
- A match is a recommendation to a human, never an assignment; assignment is already the reviewer's act
- Matching must not rank interns against each other or produce a score on a person
- No catalog of assignable projects was found, so any matching claim today would be invented
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- sandbox_database_read
**Application surfaces**
- unknown (no surface exists today)
- unknown (no endpoint exists today)
**Source modules**
- unknown (not implemented)
**Dependencies**
- unknown (no project catalog exists)
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: Nothing exists. The owner's candidate named project matching recommendations, and what the application actually has is project GENERATION plus a readiness gate, which is a different capability. Until Dhee says a catalog should exist, R0 definition work and at most R2 question-answering is all that can be justified.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R0)**
Defines whether matching is wanted, what a catalog would contain, and which human decides a match

Acceptance: Dhee states in writing whether a catalog should exist; until then this capability stays absent and no evidence is claimed
## Before you start
- no project catalog exists in the application
- Dhee has not said whether matching is wanted instead of generation
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

- [ ] Dhee states in writing whether a catalog should exist; until then this capability stays absent and no evidence is claimed

When every box above is ticked, stop and show the demo.
