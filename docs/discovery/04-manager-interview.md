# Manager interview plan: Dhee and a weekly check-in for Pip

**Story:** `internship.RESEARCH-4-MANAGER-INTERVIEW` · **Domain:** Internship & Career Readiness · **Accountable human:** Dhee
**Author:** Tashima Diah (draft prepared with Claude) · **Written:** 2026-10-29
**Evidence base:** the domain inventory `backend/src/services/agentDomain/capabilities/internship.json` (35 capabilities, researched 2026-10-09), and my earlier documents `docs/discovery/02-agent-anatomy.md` (platform repo commit `825dfe3`) and `docs/discovery/03-role-and-da-transition.md` (commit `bdcd032`). The story brief was not available to me, so none of its figures or blockers appear here.
**Status:** Draft. No meeting has been scheduled, invited or booked. The date, time and attendees are unknown and are arranged by a person outside this project.

## Headline: 30 minutes, 7 questions and 1 blocker to raise

| Part | Count | Where |
| --- | --- | --- |
| Blocker to raise, not fix | 1 | Section 2 |
| Questions | 7 | Section 3 |
| Proposed weekly check-in | 1 | Section 4 |

How I counted: the story says 30 minutes is roughly eight real questions. I cut the draft list to seven questions plus the access blocker, so the agenda fits in 30 minutes with time to listen. Every question exists because the inventory or my earlier documents left something open that only Dhee can answer.

**The finding behind the questions.** All 35 capabilities in the inventory are `draft`, each with the dependency "Dhee must confirm scope, exceptions and success measures" (`internship.json`, `requirement_validation_state`, `validation_dependency`). This interview is how those get confirmed. No agent exists for the domain yet (`agent_id: null`, per `02-agent-anatomy.md`).

---

## 1. Agenda (30 minutes)

| Minutes | Section | Purpose |
| --- | --- | --- |
| 3 | Opening | Explain that nothing is decided and the interview records Dhee's answers. |
| 5 | Access to the agent surface | Raise the access blocker (section 2). |
| 7 | Which capabilities first | Questions 1 to 3 |
| 5 | What must stay human forever | Questions 4 and 5 |
| 6 | What good looks like | Question 6 |
| 3 | What to switch off immediately | Question 7 |
| 1 | Wrap-up | Read back what was decided, and what remains unknown. |
| **30** | **Total** | |

## 2. The access blocker, to raise and not to fix

**What the inventory says.** The capability `internship.manager_surface_access_verification` exists so that "Dhee can open the AI employee's detail and talk surfaces under a scoped role." It is `partly built`, and the inventory records these blockers (`internship.json`, `blocked_by`):
- The manager role check, `requireAgentManagerOrAdmin`, is "proposed and not built."
- No login path carries the identity the check needs (`org_members.id` is absent from the login token).
- Nobody has verified that Dhee has opened the surface.

**An update from my own research.** `02-agent-anatomy.md` found that the middleware now exists (`middlewares/agentManagerAuthMiddleware.ts:50`) but is not used on the internship routes. That means the inventory's statement is partly out of date. I could not confirm the current position, and I recorded it as something for Dhee or Ali to confirm.

**Why it belongs in the meeting.** A manager who cannot reach the surface cannot validate what they are being asked to validate.

**What to ask, and nothing more.** Raise it; don't try to solve it in the meeting.
- Have you been able to open the agent's detail and talk surfaces? What happens when you try?
- Who would fix it if not?

## 3. Questions

| # | Group | Question | Why we ask | Source |
| --- | --- | --- | --- | --- |
| 1 | Which capabilities first | Nine capabilities are candidates for Pip's work: skill-gap identification, reminder sending, payment gate monitoring, manager check-in brief, standup attendance, comped-access expiry warning, project matching, DA cohort analysis, and applicant PII access audit. Which three matter most to you, and in what order? | Dhee decides what is in scope, and every capability is waiting on that. | `03` section 4; `internship.json` |
| 2 | Which capabilities first | You described Pip's job as onboarding plus open house advertising and reminders. The inventory has no advertising or open house capability. Do you want advertising and open houses in Pip's scope? | It is the one place your description and the inventory disagree. | `02` second finding |
| 3 | Which capabilities first | Ali defines Career Readiness as the skill-gap work and the career support handoff. For skill gaps, what counts as a gap, and who owns the target skill profile it is measured against? | The file says no target profile exists and you have not defined a gap. | `internship.skill_gap_identification_for_interns`, `blocked_by` |
| 4 | Stays human forever | The inventory reserves ten acts to a human, including admission decisions, document verification, activation and publishing. Is anything missing from that list, or anything on it you would move? | These limit everything else Pip could do. | `03` section 4; `human_decision` |
| 5 | Stays human forever | For the career support handoff: who receives the person, what triggers it, and what evidence goes with them? | The file says you have not named the receiving person or the trigger. | `internship.career_support_handoff`, `blocked_by` |
| 6 | What good looks like | For the first capability you chose, what number would tell you Pip is working, and what is the number today? After 30 days, what would make it worth building? | The inventory leaves success measures open on every capability, and no baselines exist. | `02` section 9 (Verification) |
| 7 | Switch off immediately | If Pip misbehaved, what would you want switched off straight away, and who is able to do it? | A global kill switch already stops sends, but nothing says what else should stop. | `02` section 8 (Safeguards), `services/launchSafety.ts:124` |

## 4. Proposed weekly check-in

This is a proposal for Dhee to change. It starts from the weekly watch routine in `02-agent-anatomy.md`, section 6.

| Item | Proposal |
| --- | --- |
| Day | Unknown. Dhee picks. |
| Duration | 15 minutes |
| What it reviews | 1. Which interns and applicants Pip flagged as needing attention, with the evidence. 2. Anything Pip could not determine and reports as unknown. 3. Any decision waiting on a person, such as an admission, a verification or a handoff. |

Nothing in this proposal sends an invitation. The check-in is a plan for Dhee to accept, change or reject.

## 5. What to do with each answer

So a completed interview changes the build and does not just record opinions:

| Answer | What changes |
| --- | --- |
| Dhee names the first three capabilities | Update `03` section 4 and the scope document. Those become the candidates to define first. |
| Dhee wants advertising or open houses | Write it as a new capability with its own scope, owner and success measure. It is not in the inventory. |
| Dhee defines a skill gap and names the owner | The skill-gap capability's `blocked_by` is partly resolved. Record it in the scope document. |
| Dhee changes the human-only list | Update `03` section 4. Any act moved off the list needs Dhee's written reason. |
| Dhee names the handoff receiver and trigger | Record it as the contract the inventory says is missing. Pip still only describes the handoff. |
| Dhee gives a number and a baseline | That becomes the success measure for that capability. If no baseline exists, record "start tracking" as the first task. |
| Dhee names what to switch off | Add it to the safeguards in `02`, and name the person who can do it. |
| The access blocker is confirmed | Record who owns the fix. Pip's capabilities stay in draft until Dhee can reach the surface. |

## 6. Answers (to be filled in after the interview)

**Interview held:** unknown. Date and time: unknown.

**Access to the agent surface**
Answer:

**Question 1: first three capabilities**
Answer:

**Question 2: advertising and open houses**
Answer:

**Question 3: skill-gap definition and owner**
Answer:

**Question 4: the human-only list**
Answer:

**Question 5: handoff receiver and trigger**
Answer:

**Question 6: success measure, baseline, and 30-day test**
Answer:

**Question 7: what to switch off immediately**
Answer:

**Weekly check-in: day, duration and what it reviews**
Answer:

**Still unknown after the interview:**

## Sources

- `services/agentDomain/capabilities/internship.json` (35 capabilities; `requirement_validation_state`, `validation_dependency`, `blocked_by`, `human_decision`). Capabilities used: `manager_surface_access_verification`, `skill_gap_identification_for_interns`, `career_support_handoff`, `offer_document_package_generation`
- `docs/discovery/02-agent-anatomy.md` in `tashdiah-Data/ai-project` (platform repo commit `825dfe3`): second finding; sections 6, 8 and 9; `services/launchSafety.ts:124`; `middlewares/agentManagerAuthMiddleware.ts:50`
- `docs/discovery/03-role-and-da-transition.md` in `tashdiah-Data/ai-project` (platform repo commit `bdcd032`): sections 3 and 4

Line numbers are as reported in `02-agent-anatomy.md`. I did not re-check them in the platform repo.
