# internship.CAP.skill_gap_identification_for_interns — Skill-gap identification for interns

An intern and their manager see which skills are demonstrated, which are decaying and which are missing for the work ahead, so coaching targets a gap rather than a guess.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no target skill profile for internship work exists
- skillGenomeService is not wired into the internship domain
- Dhee has not defined what a gap means here

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Skill-gap identification for interns
**Domain:** Internship & Career Readiness · **Capability:** `internship.skill_gap_identification_for_interns`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** manager_defined
## The business outcome
An intern and their manager see which skills are demonstrated, which are decaying and which are missing for the work ahead, so coaching targets a gap rather than a guess.
Intended users: Dhee, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the intern's demonstrated skills and evidence counts
- Apply the decay rule to get effective levels
- Compare against a target profile for the internship (does not exist today)
- Report gaps to the intern and the manager as coaching input
**Inputs**
- enrollment id
- SkillMastery rows
- skill ontology
- unknown: the target skill profile for internship work
**Outputs**
- skill genome with effective levels after decay
- unknown: a gap list against a target profile
**Exceptions that must be handled**
- A gap is coaching input, never an admission or retention criterion
- Decay must be shown as decay, not as a skill never held, or the intern reads it as being graded down
- An intern with no evidence has an unknown profile, not a zero profile
- No target skill profile for internship work exists, so any gap percentage produced today would be invented
- Skill data must never be presented as a ranking of interns against each other
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- skill_genome_read
- sandbox_database_read
**Application surfaces**
- unknown (no internship skill-gap surface exists)
- unknown (no internship endpoint exposes the skill genome)
**Source modules**
- backend/src/services/skillGenomeService.ts
**Dependencies**
- SkillMastery
- ontology.json
- lessonSkillMap.json
- unknown target profile
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R4**.
Why that ceiling: The underlying genome exists and can be read in sandbox, so R4 analysis work is justifiable. Production use is not, because the target profile the gap is measured against does not exist and a gap reported without it would be a number about a person with no referent.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R0)**
Defines what skills internship work requires and who owns the target profile

Acceptance: Dhee accepts a target profile definition in writing; until then the gap output stays unavailable
## Before you start
- no target skill profile for internship work exists
- skillGenomeService is not wired into the internship domain
- Dhee has not defined what a gap means here
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- backend/src/services/skillGenomeService.ts:1-3 reads SkillMastery plus ontology.json and lessonSkillMap.json, :5 DECAY_DAYS 30 (one proficiency level lost per 30 days of inactivity), :31 SkillGenome with overall_proficiency, skills_started and skills_mastered
- backend/src/services/cape/capeLearningValueScorer.ts and backend/src/services/learnerContextService.ts reference skill gap concepts
- backend/src/services/internship/internshipRequirementChecks.ts identifies REQUIREMENT gaps, not skill gaps, and is a different thing
- no import of skillGenomeService, SkillMastery or lessonSkillMap was found anywhere under backend/src/services/internship/ in this pass, so the internship domain does not consume it today

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Dhee accepts a target profile definition in writing; until then the gap output stays unavailable

When every box above is ticked, stop and show the demo.
