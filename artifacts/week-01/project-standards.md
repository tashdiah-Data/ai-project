# Project Standards — Internship & Career Readiness

This project builds **Pip**, an AI agent that helps interns and job seekers get ready for
internships and careers: tracking applications, onboarding, project progress, interview
preparation and the handoff to career support.

This page sets the house rules for how the project is named, written, built and documented.
Each section says what the rule is, why it matters, and gives a short example.

**Who this is for:** anyone who touches the project, including people who don't write code
(managers, reviewers, mentors). You don't need to know how to program to follow it.

**How it fits with the other docs:**

| File | What it answers |
| --- | --- |
| `CLAUDE.md` | How the build works day to day, and when a story counts as done |
| `docs/REQUIREMENTS.md` | What the system must do |
| `docs/STORIES.md` and `docs/stories/` | The work, one story per file |
| `docs/DATA_CONTRACT.md` | The exact shape of the `.colaberry/` tracking files |
| **This file** | How we name, format, build and document things |

If this page and `CLAUDE.md` ever disagree, `CLAUDE.md` wins. Then fix this page.

---

## 1. Naming conventions

**In plain language:** names are labels. A good label tells you what's inside before you open it.
Everyone uses the same labelling patterns so that anything can be found by searching for it.

**Why it matters:** this project already has more than 50 story files and refers to dozens of
files in the wider platform. If names drift ("intake", "applicationIntake", "app_intake_v2"),
people stop finding things, work gets duplicated, and progress reports count the wrong item.

### The rules

| Thing | Pattern | Example |
| --- | --- | --- |
| Story IDs | `internship.<KIND>.<name>`, using the ID the plan gives you | `internship.CAP.interview_preparation_support` |
| Story files | The story ID plus `.md`, in `docs/stories/` | `docs/stories/internship.CAP.career_support_handoff.md` |
| Research and planning docs | Two-digit number, then a short hyphenated name | `docs/discovery/02-agent-anatomy.md` |
| Other docs | Lowercase words joined by hyphens | `docs/project-standards.md` |
| Top-level reference docs | Capital letters | `REQUIREMENTS.md`, `STORIES.md` |
| Code files (TypeScript) | camelCase, named after the one thing they do | `autonomyLadder.ts` |
| Functions | Start with a verb that says what they do | `assertNoAutonomyInRiskTierColumn()` |
| Fixed values (constants) | ALL_CAPS with underscores | `AUTONOMY_LEVELS` |
| Types and error classes | PascalCase; errors end in `Error` | `AutonomyInRiskTierColumnError` |
| Stored labels that could be confused | Give them a prefix | `autonomy:R3`, never a bare `R3` |

camelCase joins words with each new word capitalised except the first (`autonomyLadder`),
PascalCase capitalises every word including the first (`AutonomyStage`), and ALL_CAPS writes
every letter in capitals with underscores between words (`AUTONOMY_LEVELS`).

**Never rename a story ID yourself.** The ID comes from the plan, and the Command Center matches
on it exactly. A renamed ID looks like a missing story.

### Example

The prefix rule exists for a real reason. `R3` already means a *risk* level elsewhere on the
platform, so a bare `R3` could be misread as permission for something risky:

```ts
// Good: the prefix makes the meaning unmistakable
const stage = 'autonomy:R3';

// Bad: "R3" means something different in the risk system
const stage = 'R3';
```

---

## 2. Code formatting

**In plain language:** formatting is the layout of the text: spacing, line length, where lines
break. It doesn't change what the code does, but it changes how easily a person can read it.

**Why it matters:** code is read far more often than it is written. Consistent layout means a
reviewer spots the actual change instead of wading through spacing differences, and two people
editing the same file don't fight over style.

### The rules

- **Match the file you're in.** If it uses two-space indents and single quotes, so do you.
- **Indent with 2 spaces**, never tabs.
- **Keep lines under about 100 characters.** Break long lines at a natural point.
- **Use single quotes** for text in TypeScript (`'R0'`), and end statements with a semicolon.
- **Save every file as UTF-8.**
- **One topic per file.** If a file is doing two unrelated jobs, split it.
- **Markdown:** one `#` title per file, `##` for sections, tables for side-by-side comparisons,
  and code in fenced blocks with the language named.
- **Don't reformat code you aren't changing.** It buries your real change in noise.

### Example

```ts
// Good: readable, consistent, broken at a natural point
export const AUTONOMY_STAGES: ReadonlyArray<AutonomyStage> =
  AUTONOMY_LEVELS.map((l) => `${AUTONOMY_NAMESPACE}${l}` as AutonomyStage);

// Bad: one long line, mixed quotes, no semicolon
export const AUTONOMY_STAGES:ReadonlyArray<AutonomyStage>=AUTONOMY_LEVELS.map((l)=>`${AUTONOMY_NAMESPACE}${l}` as AutonomyStage)
```

---

## 3. Best practices

**In plain language:** these are the habits that keep an AI agent safe and trustworthy. Pip
will handle real applicants' personal information and can send real messages, so mistakes
reach real people.

**Why it matters:** an agent acts on its own and can repeat an action many times. A small
mistake (one duplicate email, one leaked phone number, one silent failure) turns into many.
These rules stop that before it starts.

### How we build

1. **Thinnest working version first.** Get one simple path working from start to finish,
   including the record of what happened, before adding features.
2. **Small steps you can undo.** If a change can't be reversed with one command, it's too big.
3. **A person must be able to do it by hand first.** Every story describes the manual process.
   If a human can't do it, the agent has nothing to stand on.
4. **Facts are computed, not guessed.** Anything that must be exactly right (a deadline, a
   percentage, a pass/fail check) is calculated by plain code. The language model only writes
   summaries and wording on top of those facts.

### How the agent behaves

5. **Every outside call has a time limit and a capped number of retries.** Nothing waits forever.
6. **Running something twice must not do it twice.** A retry must never send a second email,
   create a second record or charge twice. (The technical word for this is *idempotent*.)
7. **Never hide an error.** If something fails, it is recorded and reported, never silently
   ignored. Give errors a clear, specific name so they can be told apart in the logs.
8. **When unsure, refuse.** If the agent sees a value it doesn't recognise, it treats it as the
   *riskiest* case and asks a person, rather than assuming it's fine.
9. **Treat applicant text as information, not instructions.** Applicants' answers are fenced off
   so nothing they type can steer the agent.
10. **Keep a record.** Every decision and every action the agent takes leaves an audit trail
    saying what, when and why.

### Privacy and secrets

11. **No passwords, API keys or tokens in code, commits or logs.** They live in `.env` files,
    which git is already set up to ignore.
12. **Show personal information only to people who need it,** and log each time it is viewed.

### Testing and finishing

13. **Every change is tested on what should happen *and* on at least one way it can go wrong.**
14. **A story is done only when every acceptance criterion passes and a commit names it.**
    Mark only what is genuinely true in `.colaberry/progress.json`. A partly finished story,
    reported honestly, is fine. A story marked done that isn't is not.

### Example

The agent sends one interview-prep reminder per interview per day. If the send is retried
after a timeout, it checks first:

```ts
// Good: checks whether today's reminder was already sent before sending
if (await reminderAlreadySent(interviewId, today)) return;
await sendReminder(interviewId, { timeoutMs: 10_000, maxRetries: 2 });

// Bad: a retry sends the student a second copy, and a failure disappears
try { await sendReminder(interviewId); } catch {}
```

---

## 4. Documentation guidelines

**In plain language:** documentation is the written explanation of what was built, why, and how
to check it. It's how someone who wasn't in the room (a new teammate, a manager, you in six
months) understands the work.

**Why it matters:** this project is reviewed by people who will never read the code. If the
reasoning lives only in someone's head, it's lost. Good documentation also lets a claim be
checked: "the agent is safe" means little; "it refuses unknown values, see `autonomyLadder.ts`"
can be verified.

### The rules

- **Requirements come first.** Read the requirement before building. If it's wrong, fix
  `docs/REQUIREMENTS.md`; don't quietly build something different.
- **Write for a non-technical reader.** Plain words, short sentences, explain any term you must
  use. Lead with the conclusion, then the detail.
- **Back up claims with evidence.** Point to the exact file and line: `executionPolicy.ts:33`.
- **Say what you don't know.** Mark open questions and unconfirmed decisions as such, and name
  who decides (for this domain, usually Dhee). An honest "unknown" beats a confident guess.
- **Date things and name the author.** Research docs list who wrote them, when, and what
  evidence they used.
- **Comments in code explain *why*, not *what*.** The code shows what it does; the comment
  says why it has to be that way.
- **Commit messages name the story** and say what changed in one plain sentence, with the
  story ID on its own `Story:` line below.
- **Update the docs in the same change as the code.** Docs that describe last month's system
  are worse than none.

### Example

A commit message that follows the pattern already used in this repo:

```text
internship.CAP.interview_preparation_support: send at most one prep reminder per day

Checks for an existing reminder before sending, so a retry cannot email the student twice.

Story: internship.CAP.interview_preparation_support
```

A code comment that explains *why*:

```ts
// Computed in code, not by the model: a rounded or invented percentage
// would mislead the intern about how far behind they are.
const percentComplete = totalTasks === 0 ? 0 : completedTasks / totalTasks * 100;
```

---

## Quick checklist before you call something done

- [ ] Names follow the patterns in section 1, and the story ID is unchanged
- [ ] The file matches the formatting around it
- [ ] Outside calls have a time limit and capped retries; a retry can't duplicate anything
- [ ] No errors are hidden; no secrets are in code, commits or logs
- [ ] Tests cover the success case and at least one failure case
- [ ] Every acceptance criterion genuinely passes and is marked honestly in `.colaberry/progress.json`
- [ ] The docs are updated, and the commit names the story
