# internship.CAP.certification_readiness_tracking — Certification readiness and attempt tracking for interns

A manager sees each intern's certification readiness, their official claim and every sitting with its item count, so a stalled certification is visible before it becomes a surprise.

**Built today:** implemented. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- Dhee has not validated what certification signal matters for interns

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Certification readiness and attempt tracking for interns
**Domain:** Internship & Career Readiness · **Capability:** `internship.certification_readiness_tracking`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** implemented · **Source:** existing_application
## The business outcome
A manager sees each intern's certification readiness, their official claim and every sitting with its item count, so a stalled certification is visible before it becomes a surprise.
Intended users: Dhee, internship-section staff, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- Read the intern's readiness snapshot and official claim
- Summarise the claim deterministically
- Build the attempt series with each sitting's item count
- Surface both on the roster and the console detail
**Inputs**
- enrollment id
- CertSession rows
- CertReadinessSnapshot
- official claim rows
**Outputs**
- InternCertification with readiness and official claim
- CertAttemptSeries with per-sitting item counts
**Exceptions that must be handled**
- Readiness is preparation readiness; Colaberry issues no certification and must never imply it does
- A readiness snapshot that does not exist is unknown, not zero
- The certification timing logic must be read, not changed, from the internship side
- A readiness figure must not be presented as a pass prediction for a named person
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- cert_readiness_read
- sandbox_database_read
- internship_admin_api_read
**Application surfaces**
- admin Intern Console
- admin Intern Console detail
- GET /api/admin/internship/console
- GET /api/admin/internship/console/:enrollmentId
**Source modules**
- backend/src/services/internship/internshipCertification.ts
**Dependencies**
- CertSession
- CertReadinessSnapshot
- CertDomain
- CertTrack
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R6**.
Why that ceiling: Read-only over an existing subsystem the internship domain is instructed not to change, so a separately approved supervised production pilot of the report is the reasonable ceiling.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Reports an intern's certification readiness and sitting history without predicting an outcome

Acceptance: Matches internCertification() and toCertAttempts(); no pass prediction appears and no issued credential is implied
## Before you start
- Dhee has not validated what certification signal matters for interns
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipCertification.ts:35 InternCertReadiness, :49 InternOfficialCert, :94 summarizeOfficialClaim (pure), :112 internCertification, :199 toCertAttempts (pure), :221 certAttempts
- internConsoleRoster.ts:38 ConsoleCert on the roster row; internshipRoutes.ts:131 console detail returns the cert attempt series with each sitting's item count and the readiness and claim pair
- AI_INTERNSHIP_DISCOVERY.md: CertSession, CertReadinessSnapshot, CertDomain, CertTrack and ensureCertPrepSchema already exist and are to be read, not changed, with the instruction do not change timing
- CCAR-F is Anthropic's credential and Colaberry sells preparation, so readiness here is prep readiness and not an issued certification

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches internCertification() and toCertAttempts(); no pass prediction appears and no issued credential is implied

When every box above is ticked, stop and show the demo.
