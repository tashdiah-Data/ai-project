# internship.CAP.payment_gate_monitoring — Payment gate and unpaid-limbo monitoring

An accepted applicant who has not paid does not silently stall forever, and document verification never grants paid training for free.

**Built today:** partial. **Requirement validated:** draft -- Dhee must confirm scope, exceptions and success measures

**Blocked by**
- no scheduler wires the expiry or the payment reminder today
- Dhee has not set the expiry window

Full build prompt is attached to this story.

**Release:** r2 · r2 (weeks 0–0)
**Owner:** 
**Blocked by:** nothing — you can start this now

## The requirement this satisfies

_No requirement linked._

## How to build it

# Payment gate and unpaid-limbo monitoring
**Domain:** Internship & Career Readiness · **Capability:** `internship.payment_gate_monitoring`
**Accountable human:** Dhee · **Validation state:** draft
**Built today:** partial · **Source:** existing_application
## The business outcome
An accepted applicant who has not paid does not silently stall forever, and document verification never grants paid training for free.
Intended users: Dhee, intern.
## The human-operable process, as it works now
This must be operable by a person before an agent touches it. If a human cannot do it by hand
against the surfaces below, the agent version has nothing to stand on.
**Workflow**
- After documents are verified, resolve the next state from subscription and comp status
- Hold unpaid applications at payment_pending
- Surface the payment_pending set to the manager in the onboarding bucket
- On expiry, move the application to withdrawn rather than leaving it indefinitely open
**Inputs**
- application id
- cohort requires_subscription setting
- subscription status
- comp status
**Outputs**
- resolved post-verification state
- unpaid-limbo list
- expiry outcome
**Exceptions that must be handled**
- requires_subscription defaults true; flipping it to false must be a deliberate cohort-setting change, not an accident of code
- A comped applicant must not be held at payment_pending
- Expiry must be idempotent; running it twice must not double-withdraw or double-notify
- The expiry timer and the payment reminder are not wired to any scheduler today, so the limbo list is manager-reviewed, not automatic
## The tool interface
**Tools this capability requires** (none is one of the eight default-denied capabilities;
anything that would need one is listed under blockers instead)
- internship_admin_api_read
- sandbox_database_read
- subscription_status_read
**Application surfaces**
- admin review queue onboarding bucket
- GET /api/admin/internship/queue
**Source modules**
- backend/src/services/internship/internshipStateMachine.ts
- backend/src/services/internship/internshipCohortService.ts
**Dependencies**
- internshipCohortService
- enrollments payment status
- activeCompEnrollmentIds
- internshipApplicationService.transition
## The agent behaviour that uses it
Starting autonomy **autonomy:R0**; ceiling **autonomy:R5**.
Why that ceiling: Reading and reporting the unpaid set is safe. Expiring an application withdraws a real person's place, so the ceiling is shadow mode and the expiry act stays with the reviewer or the existing system edge.

Not a human-reserved decision, but promotion is still evidence-based and one step at a time.
**First release behaviour (autonomy:R2)**
Explains why a given application resolved to payment_pending rather than activation_pending

Acceptance: Matches stateAfterDocumentsVerified() for all four input combinations
## Before you start
- no scheduler wires the expiry or the payment reminder today
- Dhee has not set the expiry window
## Evidence this is done
Cite the real thing: a named passing test, a measured figure with its query, or a file:line.
**None of these count:** this story being marked complete, elapsed time, a conversation that
went well, or a mocked test passing. Dhee evaluates the evidence, and
nothing here is validated yet.
## Evidence this capability rests on
- internshipStateMachine.ts:55 payment_pending state, :345 stateAfterDocumentsVerified, :352-355 returns activation_pending when a subscription is not required or an active subscription or comp exists, else payment_pending
- internshipStateMachine.ts:209 payment_pending to activation_pending (system or reviewer), :211 payment_pending to withdrawn by expiry
- internshipCohortService.ts:94 DEFAULT_INTERNSHIP_SETTINGS, :183 internshipRequiresSubscription
- AI_INTERNSHIP_DISCOVERY.md risk R1: activation grants paid training free; mitigation is payment_pending plus requires_subscription defaulting true
- AI_INTERNSHIP_SPEC.md flow decision 2: payment on acceptance before access, needing a nudge and an expiry for accepted people who never pay

## Failure paths you must handle


## Acceptance — your stop condition

Tick each box as it genuinely passes. This file is yours — the platform reads
the same criteria out of `.colaberry/progress.json`, which Claude Code keeps in
step (see the managed block in CLAUDE.md). Ticking something you have not
actually met only misleads you.

- [ ] Matches stateAfterDocumentsVerified() for all four input combinations

When every box above is ticked, stop and show the demo.
