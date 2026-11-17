---
name: skill-template
description: Use when creating, scaffolding, or standardizing a new reusable Agent Skill, or when the user asks for a skill template, starter SKILL.md, or a guide to writing skills. Produces a spec-compliant skill folder with valid frontmatter (name, description, license, compatibility, metadata, allowed-tools), a clear trigger-first description, and a structured instruction body covering purpose, inputs, step-by-step workflow, output format, examples, edge cases, and quality checks. Also use to review an existing skill against the Agent Skills specification and fix naming, description length, or structure problems.
license: MIT
compatibility: Works with any Agent Skills-compatible client (Claude Code, Claude.ai, Claude API). Requires file read/write access. No network access or external packages needed.
metadata:
  author: "Tash"
  version: "1.0.0"
allowed-tools: Read Write Edit Glob Grep
---

# Skill Template

A reusable starting point for building Agent Skills that load reliably, trigger at the right time, and give the agent clear, repeatable instructions.

## When to use this skill

Use this skill when the user wants to:

- Create a new skill from scratch ("make me a skill that…", "turn this workflow into a skill").
- Get a blank or annotated `SKILL.md` template.
- Check an existing skill against the Agent Skills specification.
- Convert a repeated prompt, checklist, or SOP into a reusable skill.

Do **not** use it for one-off tasks that will never be repeated; a plain prompt is enough.

## Specification rules (must follow)

### Folder and file layout

```
<skill-name>/
├── SKILL.md          # required: frontmatter + instructions
├── scripts/          # optional: executable helpers
├── references/       # optional: long docs loaded only when needed
└── assets/           # optional: templates, images, sample data
```

- The main file is always named `SKILL.md`.
- The folder name **must exactly match** the `name` field.

### Frontmatter: only these six fields

| Field | Required | Rules |
|---|---|---|
| `name` | Yes | 1–64 chars; lowercase letters, numbers, hyphens only; no leading/trailing or double hyphens; matches folder name. |
| `description` | Yes | 1–1,024 chars; states **when to use it first**, then what it does; include trigger keywords. |
| `license` | No | License name (e.g., `MIT`, `Apache-2.0`) or reference to a bundled license file. |
| `compatibility` | No | Max 500 chars; environment needs (product, packages, network access). |
| `metadata` | No | Key/value map; **all values are strings** (e.g., `author`, `version`). |
| `allowed-tools` | No | Space-delimited list of pre-approved tools (experimental; support varies by client). |

Do not add any other top-level fields. Put extra information such as `author`, `version`, or `tags` inside `metadata` as quoted strings.

### Writing a strong description

The description is what the agent reads to decide whether to load the skill, so:

1. **Lead with the use case:** "Use when…" followed by the situations and phrases that should trigger it.
2. **Then state the output:** what the skill produces or does.
3. **Include keywords** users are likely to say (file types, task names, domain terms).
4. **Stay under 1,024 characters**; aim for 300–700.
5. Avoid vague phrasing like "Helps with documents."

## Implementation guide

### Step 1: Define the skill's job

Answer these before writing anything:

- What single, repeatable task does this skill handle?
- What does the user typically say when they need it?
- What inputs does it need (files, pasted text, parameters)?
- What exactly should the output look like (format, file name, location)?
- What should it refuse or hand off to another skill?

### Step 2: Choose the name and create the folder

- Pick a short, descriptive, lowercase-hyphenated name, e.g. `weekly-status-report`.
- Create the folder with that exact name and add `SKILL.md` inside it.

### Step 3: Write the frontmatter

Copy the block at the top of this file, then:

- Set `name` to the folder name.
- Write the description (use case first, under 1,024 characters).
- Set `metadata.author` and `metadata.version` as quoted strings; start at `"1.0.0"` and bump it on every change.
- List only the tools the skill actually needs in `allowed-tools`.
- Delete optional fields you don't need rather than leaving them blank.

### Step 4: Write the instruction body

Use this structure, written as direct instructions to the agent:

1. **Title and one-line summary.**
2. **When to use / when not to use.**
3. **Inputs:** what to collect and what to ask for if missing.
4. **Workflow:** numbered, concrete steps in the order the agent performs them.
5. **Output format:** exact structure, headings, file names, and save location.
6. **Examples:** at least one input → output pair.
7. **Edge cases:** missing data, ambiguous requests, very large inputs.
8. **Quality checklist:** what to verify before finishing.

Tips:

- Use imperative voice ("Extract the dates", not "You might want to extract…").
- Explain *why* a rule exists when it isn't obvious; the agent handles edge cases better.
- Keep `SKILL.md` under ~500 lines. Move long reference material to `references/` and point to it ("Read `references/style-guide.md` before formatting").
- Put deterministic or repeated logic in `scripts/` and tell the agent when to run it.

### Step 5: Add supporting files (optional)

- `scripts/`: helpers the agent runs (e.g., `scripts/validate.py`). Document arguments and expected output in `SKILL.md`.
- `references/`: detailed specs, schemas, or style guides loaded on demand.
- `assets/`: templates, logos, or sample files used in outputs.

Reference every file by relative path from the skill root.

### Step 6: Test the skill

1. Write 3–5 realistic prompts that **should** trigger the skill and 2–3 that **should not**.
2. Run each one and confirm the skill loads (or doesn't) as expected.
3. Check that outputs match the format in Step 4.
4. If triggering is unreliable, rewrite the description with clearer use-case keywords.
5. Bump `metadata.version` after each fix.

### Step 7: Install and share

- **Claude Code:** place the folder in `~/.claude/skills/` (personal) or `.claude/skills/` in a project (shared with the repo).
- **Claude.ai:** zip the skill folder and upload it under Settings → Capabilities → Skills.
- **API:** upload through the Skills API and reference it by ID.

## Blank SKILL.md template

Copy this into `<your-skill-name>/SKILL.md` and fill in the placeholders:

```markdown
---
name: your-skill-name
description: Use when [situations and trigger phrases]. [What the skill does and produces, including key file types or terms.]
license: MIT
compatibility: [Product and environment requirements, or delete this line.]
metadata:
  author: "Your Name"
  version: "1.0.0"
allowed-tools: Read Write
---

# Your Skill Title

[One-sentence summary of what this skill does.]

## When to use
- [Trigger situation 1]
- [Trigger situation 2]

## When not to use
- [Out-of-scope situation]

## Inputs
- [Required input and how to ask for it if missing]

## Workflow
1. [First step]
2. [Second step]
3. [Third step]

## Output format
[Exact structure, file name, and save location.]

## Example
**Input:** [Sample request]
**Output:** [Sample result]

## Edge cases
- [Situation] → [What to do]

## Quality checklist
- [ ] [Check 1]
- [ ] [Check 2]
```

## Quality checklist (for any skill you build)

- [ ] Folder name equals `name`; `name` is lowercase with hyphens only.
- [ ] Frontmatter uses only the six spec fields.
- [ ] `author` and `version` live inside `metadata` as quoted strings.
- [ ] Description starts with the use case and is under 1,024 characters.
- [ ] Workflow steps are numbered, concrete, and in order.
- [ ] Output format is explicit (structure, file name, location).
- [ ] At least one example is included.
- [ ] Long reference material is moved out of `SKILL.md`.
- [ ] Trigger and non-trigger test prompts behave as expected.
