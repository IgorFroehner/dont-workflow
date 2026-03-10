# dont-workflow

A lightweight skill set for Claude Code. Two focused workflows — one for building features, one for fixing issues — without spawning a fleet of sub-agents.

## Workflows

### Build a Feature

```
/clarify  →  /plan  →  /implement  →  /review  →  /compound
```

### Fix an Issue

```
/investigate  →  /fix
```

---

## Skills

### `/clarify [feature description]`

Turns a vague feature idea into a precise requirements document.

- Asks targeted questions (scope, behavior, edge cases, constraints)
- Max 2 rounds of questions — doesn't interrogate endlessly
- Saves to `.claude/docs/<feature>-requirements.md`

---

### `/plan [feature or requirements file]`

Creates an actionable implementation plan, step by step.

- Reads existing code to understand the architecture before planning
- Asks about technical trade-offs (polling vs events, sync vs async, etc.) before writing
- Each step identifies specific files, functions, and changes
- Saves to `.claude/docs/<feature>-plan.md`

---

### `/implement [plan file or step number]`

Follows the plan and implements the feature.

- Implements step by step, telling you what was done after each
- Documents unexpected discoveries in the plan's **Findings** section
- Stops and explains blockers rather than improvising around them
- Can resume from a specific step: `/implement step 3`

---

### `/review [file, "staged", "last", or PR number]`

Reviews code for correctness, safety, and quality.

- Accepts a file path, `staged` (git staged changes), `last` (last commit), or a PR number
- Findings organized by severity: Critical / Important / Minor / Good
- Direct and specific — references `file:line`, explains why issues matter
- Won't nitpick style or suggest unrelated refactors

---

### `/compound [feature name or plan file]`

Produces a permanent record of what was built, to be committed to the repo.

- Reads the plan, requirements, and git history to summarize what changed
- Captures key decisions (the *why*, not just the *what*) and implementation discoveries
- Saves to `docs/solutions/<feature>.md` — meant for humans and future LLMs

---

### `/investigate [issue description]`

Finds root causes of bugs without guessing.

- Forms multiple hypotheses and challenges each with evidence
- Asks for specific context it can't get from code alone (logs, query plans, repro steps) before drawing conclusions
- If you can't provide context, it proceeds and marks it as an assumption
- Saves to `.claude/docs/<issue>-investigation.md`

---

### `/fix [investigation file] [finding numbers]`

Applies targeted fixes from an investigation.

- Only fixes what you ask for: `/fix investigation.md 1 2` skips finding 3
- Confirms scope before touching anything
- Provides specific validation steps after fixing
- Minimal, surgical changes — doesn't refactor surrounding code

---

## Design Principles

- **No sub-agents.** Every skill does its own research with Read/Grep/Glob.
- **Ask before assuming.** Skills pause and ask rather than fill gaps with guesses.
- **Scope discipline.** `/fix` only fixes what you ask. `/implement` follows the plan. Skills don't bleed into each other's jobs.
- **Lightweight docs.** Working files go in `.claude/docs/` (ephemeral). Final records go in `docs/solutions/` (committed).
