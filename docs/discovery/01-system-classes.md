# System classes: where Internship & Career Readiness belongs

| | |
|---|---|
| **Domain** | Internship & Career Readiness |
| **Release** | Discovery and validation |
| **Story** | `internship.RESEARCH-1-SYSTEM-CLASSES` |
| **Accountable human** | Dhee |
| **Status** | Draft. Provisional recommendation, because the two primary inputs are unavailable (see below) |

## Read this first: what this document could and could not see

This document was written without either of the inputs the story asks it to rest on:

1. **The story brief** is the source of this domain's figures and the blockers that discovery found. It was not available to the author. **Every domain figure in this document is therefore unknown.** None has been estimated or filled in.
2. **The platform's agent registry** (`backend/src/services/agentRegistrySeed.ts`) is not in this repository, and the author does not have access to it. **Every claim about what the platform currently runs is therefore unknown.**

So the recommendation below is written as a decision rule. It names the figures that decide it and says which way each figure pushes. Once someone with the brief supplies them, the answer follows mechanically, without re-arguing.

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

### Why, in terms of this domain's own figures

The figures that decide this, all **unknown** (brief not available):

| Figure | Pushes toward fixed workflow if… | Pushes toward tool-using agent if… | Value |
|---|---|---|---|
| Interns/candidates handled per cycle | low enough that staff can review each case | too high for per-case human review | **unknown** |
| Share of cases that follow a standard path (intake → readiness check → placement) | high | low; many cases need a path nobody scripted | **unknown** |
| Number of distinct systems a case touches | one or two | several, in an order that varies by case | **unknown** |
| Blockers discovery found | about data access, consent or ownership | about step variety the workflow can't express | **unknown** |

The argument does depend on these numbers. If volume is high *and* most cases do not follow a standard path, the recommendation moves up to a tool-using agent. If either one is false, it stays a fixed workflow.

### What is known, and supports the provisional choice

These constraints come from the story itself, not from the brief:

- **No outbound contact is authorized.** "Nothing in this story authorizes an outbound message", and arranging anything with a person is "a human action taken outside this project" (story prompt, *Before you start*). In this domain, the main actions an agent could take autonomously are contacting students, employers and mentors, or booking interviews. With those excluded, the remaining work is internal review and drafting, which is what a fixed workflow is for.
- **No agent may be enabled or granted tools, and capabilities stay `draft` until reviewed** (story prompt, *Before you start*). A class whose value comes from choosing its own tools (3 and above) cannot deliver that value under this constraint.
- **The domain has a single named accountable human (Dhee).** A workflow with review gates produces output one person can check step by step. Classes 4 and 5 produce plans and handoffs that are harder for one reviewer to audit.

## What this domain explicitly does NOT need

**Multi-agent systems (class 5).** This exclusion does not depend on the unknown figures. A multi-agent design is justified by specialisations that need separate permissions or trust levels. In this domain, the permission that would differ between agents is the authority to contact people outside the platform, and the story withholds that from every agent ("Nothing in this story authorizes an outbound message"). With no difference in permissions to separate, splitting the work across agents adds coordination cost and new failure points without adding any capability.

**Agentic systems (class 4)** are also not justified now. That conclusion is weaker, because it rests on the unknown "share of cases that follow a standard path".

## What would justify the next class up (tool-using agent)

All three of these must hold. Each is a check someone can perform, not a judgment of "maturity":

1. **Volume exceeds review capacity.** The number of cases per cycle (unknown) exceeds what Dhee's team can review case by case in that cycle (unknown), as measured from the domain's actual records.
2. **The scripted path doesn't fit most cases.** In a sample of real cases, the fixed workflow's path fits fewer than a threshold share of them. Dhee sets the threshold. The current share is unknown.
3. **The tools exist and have been reviewed.** Every tool the agent would call already exists in the registry, read-only or draft-only, with no outbound-message capability, and Dhee has reviewed each tool grant. Today's registry entries are unknown.

## Sources

| Claim | Source |
|---|---|
| Domain, release, story ID, accountable human, deliverable path | Story prompt for `internship.RESEARCH-1-SYSTEM-CLASSES` (header) |
| No outbound messages; no agent enabled or granted tools; capabilities stay `draft` until reviewed | Story prompt, section *Before you start* |
| This repository contains no platform code: tracked files are only `README.md`, `.gitignore` and `.colaberry/connect.txt` | `git ls-files` at commit `d899038` |
| Domain figures (volume, path variety, systems touched, blockers) | **Unknown.** The story brief was not available to the author |
| What the platform currently runs for this domain | **Unknown.** `backend/src/services/agentRegistrySeed.ts` is not in this repo, and the author has no access to it |

### What would have to exist to close the unknowns

- **The brief's figures,** copied into this document's decision table, each with a citation to the brief.
- **Read access to `backend/src/services/agentRegistrySeed.ts`.** The entries near this domain should be cited by `file:line`, recording each entry's class, tools and autonomy level. If no entries exist for this domain, that is a finding in itself and should be stated.
- **Dhee's threshold for condition 2** in the "next class up" section above, written down.
