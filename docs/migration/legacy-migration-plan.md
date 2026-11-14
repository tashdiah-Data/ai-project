# Legacy migration plan: Internship & Career Readiness

**Story:** `internship.LEGACY-MIGRATION-PLAN` · **Domain:** Internship & Career Readiness · **Accountable human:** Dhee
**Author:** Tashima Diah (draft prepared with Claude) · **Written:** 2026-11-13
**Evidence base:** the Colaberry platform repository (`ColaberryIntern/ColaberryEnterprise_AI_LeadershipAccelerator`) at commit `22f3efa` (2026-11-13). Paths below are relative to `backend/src/` unless they start with `docs/` or `.colaberry/`. The inventory was researched on 2026-10-09 and the unresolved records were measured in production on 2026-10-28.

## Headline: 1 record migrates cleanly, 21 more belong to no domain, and only 8 of those 21 are finished

| What | Count | Status |
|---|---|---|
| Legacy records inherited by this domain | 1 | Migrated to a capability (`WorkforceCareerDirector`) |
| Legacy records that belong to no domain | 21 | Not in any project's plan |
| Of those 21: retired, never ran | 8 | Terminal. Ruled on 2026-11-01 |
| Of those 21: absorb, but no absorbing domain named | 8 | Not terminal. Live, 178,937 runs |
| Of those 21: parked, no owner and no autonomy source | 5 | Not terminal. Live, 1,312,192 runs |

Sources: `services/agentDomain/capabilities/internship.json` (`legacy_dispositions`) and `services/agentDomain/capabilities/_unresolved.json` (`rows`, `owner_ruling`). The run counts are the sum of each row's `measured.run_count`. They add up to 1,491,129, which matches the total in the file's own comment.

## 1. Disposition of this domain's legacy record

| Legacy record | Kind | Disposition | Successor capability |
|---|---|---|---|
| `WorkforceCareerDirector` | behavior | `migrated_to_capability` | `internship.employment_readiness_gap_flag` |

**Why it migrates and does not retire.** It is a live, enabled daily job (schedule `20 6 * * *`). It reads the employment-readiness signal for the `career` domain and flags the top gap as one task, with no AI call. The behaviour is real and still wanted. Source: `services/agentDomain/capabilities/internship.json`, `legacy_dispositions[0].reason`.

**The successor's ceiling.** In your repo's snapshot, the capability's highest permitted level is `autonomy:R6` and its effective level today is `autonomy:R0`. The recorded reason is that it writes a task, not a change to a person, and already runs deterministically. Source: `.colaberry/agent-domain.json`, `authority[]` entry for `internship.employment_readiness_gap_flag`.

**Rule from the platform roster.** The name `WorkforceCareerDirector` must not be reused as the new employee's name. Source: `docs/architecture/ai-workforce-management/AI_EMPLOYEE_ROSTER.md:45`.

## 2. What is unknown, and the one question that would resolve each

| # | Unknown | Question that resolves it | Who can answer |
|---|---|---|---|
| 1 | Whether the daily `career` employment-readiness signal is reliable | Where is the signal computed, and has anyone checked its values against real cases? | Dhee, as accountable human, to name the owner. The inventory records this as not verified (`internship.json`, `legacy_dispositions[0].reason`) |
| 2 | Who owns the shared function behind this job | Who approves a change to `runDomainFlag`, which seven directors use? | Unknown. I did not find an owner in the sources I read |
| 3 | Which domain absorbs each of the 8 "absorb" rows | Name one absorbing domain per row | Ali, who made the ruling, with the domain owners (`_unresolved.json`, rows with `owner_decision.ruling: absorb`) |
| 4 | Who owns the 5 parked rows, and what is their autonomy source | Which domain owns each, and what limits what it may do? | Ali. The file records "no owner recorded and no autonomy source" (`_unresolved.json`, rows with `owner_decision.ruling: parked`) |
| 5 | Whether the 8 retired rows have actually been removed from the platform | Has the retirement ruling been carried out on the live registry? | Unknown. I checked that the rows are recorded as retired, disabled and never run. I did not check the live registry |

## 3. The 21 records that belong to no domain

These are not in this domain's plan, but the story asks that they be named. Source for every row: `_unresolved.json`, measured 2026-10-28, ruling by Ali on 2026-11-01 (ticket AF6.1).

**Retired (8).** All are disabled and have never run, so retiring them risks nothing: `Alumni_Outreach_Agent`, `Alumni_Reengagement_Agent`, `Alumni_Referral_Agent`, `Corporate_Training_Agent`, `Employer_Relationship_Agent`, `Enterprise_Partnership_Agent`, `Opportunity_Detection_Agent`, `Trend_Detection_Agent`.

**Absorb, domain not yet named (8).** All are live. The file says they serve two domains at once, which is why they had no domain.

| Record | Runs |
|---|---|
| `SessionLifecycle` | 65,323 |
| `BehavioralSignalDetector` | 32,623 |
| `BehavioralTriggerEvaluator` | 32,516 |
| `IntentScoreRecomputer` | 21,399 |
| `OpportunityScoreRecomputer` | 15,837 |
| `SessionReminders` | 10,886 |
| `ICPInsightComputer` | 228 |
| `PortfolioGitHubSyncAgent` | 125 |

**Parked (5).** All are live, production-only, with no owner and no autonomy source. The file says parking is a recorded answer, not a decision.

| Record | Runs |
|---|---|
| `visitor_tracker` | 1,300,725 |
| `forecast_engine` | 9,351 |
| `intent_scorer` | 1,954 |
| `revenue_aggregator` | 150 |
| `calendar_intent_booster` | 12 |

## 4. Replay: running the migration twice leaves the same end state

**What the source already does.** The job that is migrating checks for an existing open task before it creates one, so a second run does not create a duplicate. Source: `services/workforce/directorActions.ts:53` (`alreadyExists: () => openTaskIdForRecKey(rec.key)`), applied by `runDirectorWrite` in `services/workforce/workforceAgentRuntime.ts:155`.

**Proposed rules (my proposal, not from the source; Dhee decides).**
1. Record each disposition once, keyed by the record's name, so repeating the step rewrites the same row.
2. The successor capability runs in place of the legacy job, never alongside it. If both ran, the same daily task could be flagged twice.
3. Retire a row only when it is disabled and has zero runs. Every other row stays listed until an owner rules on it.

## 5. A record the migration cannot classify

The plan does not assume such a record does not exist. An unclassifiable record stays on the list, marked unresolved, with its live status and run count, until a named person rules on it. It is never retired by default and never dropped from the list. The 21 records above are the proof that this case is real: they belong to no domain, and 13 of them are live.

## 6. Sequence: what must be decided first

1. **Ali names the absorbing domain for each of the 8 absorb rows.** Until then they cannot reach a terminal disposition.
2. **Ali, or the domain owners he names, gives each of the 5 parked rows an owner and an autonomy source.**
3. **Dhee names who confirms the career signal is reliable (unknown 1).** Nothing about `internship.employment_readiness_gap_flag` should move past the first level before that.
4. **Dhee reviews `internship.employment_readiness_gap_flag`.** Every capability stays draft until the accountable human has reviewed it.
5. **Confirm the 8 retirements were carried out (unknown 5).** This is only a check, and nothing is blocked on it.

Items 1 and 2 do not block this domain's own record. They block the 21 shared records from being finished.

## 7. What I checked, and what I found

I checked these against the platform repository at commit `22f3efa` on 2026-11-13, while preparing this draft.

| Citation | What it claims | Found |
|---|---|---|
| `services/workforce/directorActions.ts:72` | `runCareerDirector` calls the shared `runDomainFlag` | Correct |
| `docs/architecture/ai-workforce-management/AI_EMPLOYEE_ROSTER.md:45` | The name `WorkforceCareerDirector` must not be reused | Correct |
| `services/aiOpsScheduler.ts:343` | The job's schedule `20 6 * * *` | **Stale.** The entry is now at line 392 |
| `services/agentRegistrySeed.ts:2260-2269` | The job's registry entry | **Stale.** The entry is now at lines 2431-2439. Lines 2260-2269 hold a different agent |
| `_unresolved.json` run counts | Total 1,491,129 | Correct. The 8 absorb rows, the 5 parked rows and the file's total reconcile |

**Still to do:** Tashima re-checks the four code citations above with Claude Code and records the date here: 2026-11-13.

## 8. What I believe is wrong in the brief

1. **"Five of them are live production rows."** The platform's own file corrects this. Five is the count of rows whose *proposed kind* is unresolved. Thirteen of the 21 are live, and they have run 1,491,129 times. Source: the `_comment` in `_unresolved.json`, measured 2026-10-28. The story text in this repo (`docs/stories/internship.LEGACY-MIGRATION-PLAN.md`) still says five.
2. **"They need an owner decision."** An owner ruling was recorded on 2026-11-01, but only 8 of the 21 reached a terminal disposition. "Absorb" needs a named domain and "parked" is a deferral, so 13 rows still need a decision. Source: `owner_ruling.note` in `_unresolved.json`.
3. **Two stale line numbers** (see section 7).
4. **"Four sibling directors."** The inventory says a change to the shared function reaches four sibling directors. The code says the function is shared by 7 directors, which is this one plus six siblings. Source: `services/workforce/directorActions.ts:39` (comment) and lines 70-76.
5. **The prompt says the page brief holds each record's disposition, reason and `file:line` evidence.** The brief shown on the story page names only the counts. The detail is in `services/agentDomain/capabilities/internship.json` in the platform repository.

## Sources

- `services/agentDomain/capabilities/internship.json` (platform repo), `legacy_dispositions`, researched 2026-10-09
- `services/agentDomain/capabilities/_unresolved.json` (platform repo), `rows`, `owner_ruling`, `_comment`, measured 2026-10-28
- `services/workforce/directorActions.ts`, `services/workforce/workforceAgentRuntime.ts`, `services/aiOpsScheduler.ts`, `services/agentRegistrySeed.ts` (platform repo, commit `22f3efa`)
- `docs/architecture/ai-workforce-management/AI_EMPLOYEE_ROSTER.md` (platform repo)
- `.colaberry/agent-domain.json` (this repo)
