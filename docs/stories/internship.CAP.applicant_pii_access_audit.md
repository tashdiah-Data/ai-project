# internship.CAP.applicant_pii_access_audit — Applicant PII access audit over the internship surface

Every read of an applicant's resume, phone number or interview transcript is attributable, so the widest PII grant in the platform can be shown to have been used appropriately.

**Built today:** absent. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- not implemented
- no read-access log over the internship PII routes exists
- Dhee has not said whether an access audit is wanted

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Applicant PII access audit over the internship surface
**Domain:** Internship & Career Readiness · **Capability:** `internship.applicant_pii_access_audit`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** absent · **Source:** proposed_new
## The business outcome
Every read of an applicant's resume, phone number or interview transcript is attributable, so the widest PII grant in the platform can be shown to have been used appropriately.
Intended users: Dhee, Ali.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- unknown until Dhee confirms an access audit is wanted and what it must answer
**Inputs**
- unknown: no access log over the internship document and transcript routes was identified
**Outputs**
- unknown
**Exceptions that must be handled**
- An audit of PII access must not itself copy the PII it audits; it records who read what, not what they read
- The internship section grant is coarse, so every holder can reach the raw signed-document download; an audit that cannot distinguish readers would add nothing
- No access log over those routes was identified in this pass, so no coverage claim can be made
- An agent must never be granted the raw document read in order to audit it
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- audit_event_write
- sandbox_database_read
**Application surfaces**
- unknown (no surface exists today)
- unknown (no endpoint exists today)
**Source modules**
- unknown (not implemented)
**Dependencies**
- unknown
- internship_status_events (state audit exists; a READ audit does not)
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R2**.
Why that ceiling: Nothing exists. The write audit on state transitions is real, but no READ audit over the document or transcript routes was found, so there is no evidence to promote on and definition work is the only justifiable level.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R0)**
Defines what a PII access audit must answer and which reads must be logged

Acceptance: Dhee accepts the audit question set in writing; until then this capability stays absent and no coverage is claimed
## Before you start
- not implemented
- no read-access log over the internship PII routes exists
- Dhee has not said whether an access audit is wanted
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

- [ ] Dhee accepts the audit question set in writing; until then this capability stays absent and no coverage is claimed

When every box above is ticked, stop and show the demo.
