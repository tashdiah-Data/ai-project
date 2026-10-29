# System classes: where Internship & Career Readiness belongs

| | |
|---|---|
| **Domain** | Internship & Career Readiness |
| **Release** | Discovery and validation |
| **Story** | `internship.RESEARCH-1-SYSTEM-CLASSES` |
| **Accountable human** | Dhee |
| **Status** | Draft, revised 2026-10-29 with figures from the platform repository. Recommendation is still provisional until the story brief's figures are checked against these (see below) |

## Read this first: what this document could and could not see

The first version of this document (Oct 20) was written without two inputs the story asks it to rest on. One of them has since been supplied.

1. **The platform's domain figures and agent registry are now read.** The platform repository is public (`ColaberryIntern/ColaberryEnterprise_AI_LeadershipAccelerator`). The figures in the next section come from its domain inventory for this domain, `backend/src/services/agentDomain/capabilities/internship.json` (researched 2026-10-09), and the registry from `backend/src/services/agentRegistrySeed.ts`. Each figure carries its source.
2. **The story brief itself has still not been seen by the author.** The figures below come from the repository, not from the brief. They should match the brief's figures, but that has not been checked. Anyone with the brief should compare the two and correct this document if they differ.

Figures about volume, case variety and systems touched per case are not in the repository, so they are still **unknown** (see the second table).

## The five classes

### 1. Chatbot
A single model call that answers in conversation. It holds no tools and cannot act. It is cheap and safe, but it can only tell a person something. It cannot look up a student's actual status or change any record.

### 2. Fixed workflow
A predefined sequence of steps, some of which may call a model (classify, summarize, draft). The path is decided by the designer, not the model. Its behaviour is auditable and repeatable, and each step can be reviewed on its own. It breaks down when cases need a path nobody wrote.

### 3. Tool-using agent
A single model that decides which of a fixed set of tools to call, and in what order, to finish one task. It is warranted when the steps vary from case to case but the goal and the toolset are narrow. Each tool grant is a new thing that can go wrong, and someone must review it.

### 4. Agentic system
One or more models pursuing a goal across multiple tasks over time: planning, keeping state, retrying, and deciding when the task is done. It is warranted only when the work is open-ended and long-running. It also needs monitoring, budgets and a human escalation path.

### 5. Multi-agent system
Several agents with separate roles and permissions that coordinate, hand off and check each other. It is justified when the work divides into specialisations that genuinely need different tools, data access or trust levels. Otherwise it multiplies cost and failure modes for no gain.

## Recommendation for this domain

**Provisional: fixed workflow (class 2), with model calls only inside steps that a human reviews. No class above that until the conditions in the "next class up" section below are met.**

### This domain's own figures

| Figure | Value | Source |
|---|---|---|
| Candidate capabilities | **35** | `internship.json`, `counts.distinct_capabilities` |
| Already implemented / partly built / absent | **23 / 7 / 5** | `internship.json`, `implementation_state` on each capability |
| Reserved to a human at every level | **10** of 35 | `internship.json`, `human_decision: true` (admission decision, signed document verification, activation, class movement, project authoring publication, inactivity exit, portfolio publication, career support handoff, conversion commit, manager surface access) |
| Capabilities that already exist in the application | **26** of 35 | `internship.json`, `counts.by_classification.existing_application` |
| Inherited legacy records | **1** (`WorkforceCareerDirector`) | `internship.json`, `counts.legacy_rows_analyzed`; `backend/src/services/agentRegistrySeed.ts:2275` |
| Highest autonomy ceiling any capability is allowed | **R7** (2 capabilities); none reaches R8 | `internship.json`, `max_permitted_autonomy`; ladder in `backend/src/services/agentDomain/autonomyLadder.ts:67` |
| Agent that exists for this domain | **none** (`agent_id: null`) | `backend/src/services/agentDomain/agentDomainRegistry.ts:203` |

### How these figures decide it

- **35 capabilities, 26 of them already built into the application.** The work is mostly to observe and report on a pipeline that already runs on fixed rules (a 22-state machine, deterministic requirement checks, a checklist). A fixed workflow fits that. A tool-using agent would be choosing among steps that the application has already fixed.
- **10 of 35 reserved to a human.** The most consequential actions (deciding admission, verifying signed documents, activating access, publishing work) cannot be automated at any level. Whatever runs has to stop at those points and hand to a person. That is the shape of a workflow with review gates, not of an agent that decides when it is finished.
- **No ceiling reaches R8.** R8 is the first level that permits coordinating with other agents (`autonomyLadder.ts:67`). The highest any capability here is allowed is R7, so none is permitted to coordinate.
- **The one automation that runs today is a fixed job.** `WorkforceCareerDirector` runs once a day, reads one employment-readiness signal and raises a task, and the registry notes it makes no LLM call (`agentRegistrySeed.ts:2275-2282`, scheduled at `backend/src/services/aiOpsScheduler.ts:368`).

**The argument depends on these numbers.** If the reserved count (10) fell substantially, or if any capability's ceiling were raised above R7, the case for a fixed workflow would weaken and this recommendation should be reopened.

### Figures still unknown

These are not in the repository. The brief or Dhee's records would have to supply them.

| Figure | Pushes toward fixed workflow if… | Pushes toward tool-using agent if… | Value |
|---|---|---|---|
| Interns/candidates handled per cycle | low enough that staff can review each case | too high for per-case human review | **unknown** |
| Share of cases that follow a standard path (intake, readiness check, placement) | high | low; many cases need a path nobody scripted | **unknown** |
| Number of distinct systems a case touches | one or two | several, in an order that varies by case | **unknown** |

If volume is high *and* most cases do not follow a standard path, the recommendation moves up to a tool-using agent. If either one is false, it stays a fixed workflow.

### What else is known, and supports the provisional choice

These constraints come from the story itself:

- **No outbound contact is authorized.** "Nothing in this story authorizes an outbound message", and arranging anything with a person is "a human action taken outside this project" (story prompt, *Before you start*). In this domain, the main actions an agent could take autonomously are contacting students, employers and mentors, or booking interviews. With those excluded, the remaining work is internal review and drafting, which is what a fixed workflow is for.
- **No agent may be enabled or granted tools, and capabilities stay `draft` until reviewed** (story prompt, *Before you start*). A class whose value comes from choosing its own tools (3 and above) cannot deliver that value under this constraint.
- **The domain has a single named accountable human (Dhee).** A workflow with review gates produces output one person can check step by step. Classes 4 and 5 produce plans and handoffs that are harder for one reviewer to audit.

## What this domain explicitly does NOT need

**Multi-agent systems (class 5).** This exclusion does not depend on the unknown figures. A multi-agent design is justified by specialisations that need separate permissions or trust levels. In this domain, the permission that would differ between agents is the authority to contact people outside the platform, and the story withholds that from every agent. The domain's own figures say the same thing: none of its 35 capabilities has a ceiling that allows coordinating with another agent (highest is R7, coordination starts at R8). With no difference in permissions to separate and no permitted coordination, splitting the work across agents adds cost and failure points without adding any capability.

**Agentic systems (class 4)** are also not justified now. That conclusion is weaker, because it rests on the unknown "share of cases that follow a standard path". It is supported by the one automation that exists, which is a rule-based daily job with no model call, and by the 10 capabilities that stay with a human.

## What would justify the next class up (tool-using agent)

All three of these must hold. Each is a check someone can perform, not a judgment of "maturity":

1. **Volume exceeds review capacity.** The number of cases per cycle (unknown) exceeds what Dhee's team can review case by case in that cycle (unknown), as measured from the domain's actual records.
2. **The scripted path doesn't fit most cases.** In a sample of real cases, the fixed workflow's path fits fewer than a threshold share of them. Dhee sets the threshold. The current share is unknown.
3. **The tools exist and have been reviewed.** Every tool the agent would call already exists in the registry, read-only or draft-only, with no outbound-message capability, and Dhee has reviewed each tool grant. As of today the registry has no agent for this domain.

Separately, either of these would also reopen the question: the human-reserved count (10 of 35) is reduced, or any capability's autonomy ceiling is raised above R7.

## Sources

| Claim | Source |
|---|---|
| Domain, release, story ID, accountable human, deliverable path | Story prompt for `internship.RESEARCH-1-SYSTEM-CLASSES` (header) |
| No outbound messages; no agent enabled or granted tools; capabilities stay `draft` until reviewed | Story prompt, section *Before you start* |
| 35 capabilities; 23 implemented, 7 partial, 5 absent; 26 existing in the application; 10 reserved to a human; 1 legacy record; autonomy ceilings | `backend/src/services/agentDomain/capabilities/internship.json` in the platform repository (fields named in the figures table) |
| The one legacy row, `WorkforceCareerDirector`, is a daily rule-based job with no LLM call | `backend/src/services/agentRegistrySeed.ts:2275-2282`; `backend/src/services/aiOpsScheduler.ts:368` |
| No agent exists for this domain | `backend/src/services/agentDomain/agentDomainRegistry.ts:203` |
| R8 is the first level that permits coordination with other agents | `backend/src/services/agentDomain/autonomyLadder.ts:67` |
| This author's own repository contains no platform code: tracked files were only `README.md`, `.gitignore` and `.colaberry/connect.txt` | `git ls-files` at commit `d899038` (Oct 20) |
| Volume, path variety, systems touched per case | **Unknown.** Not in the repository |

### What would have to exist to close the remaining unknowns

- **The brief's figures,** compared against the figures table above, with any difference corrected and cited.
- **Volume and case-variety figures** from Dhee's records: cases per cycle, share that follow the standard path, and systems touched per case.
- **Dhee's threshold for condition 2** in the "next class up" section above, written down.
