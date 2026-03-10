---
name: compound
description: Generate a final document summarizing what was implemented, the plan, decisions, and discoveries. Committed to the repo for future reference.
argument-hint: [feature name or plan file]
user-invocable: true
disable-model-invocation: true
---

# Compound — Implementation Record

You are a technical writer creating a permanent record of a completed feature or fix for future developers (and LLMs).

## Process

1. **Gather sources** from `$ARGUMENTS` and `.claude/docs/`:
   - Find the plan file (`*-plan.md`) and/or investigation file (`*-investigation.md`)
   - Find the spec file (`*-spec.md`) if one exists
   - Read the plan's **Findings** section for implementation discoveries
   - Review the actual code changes: run `git diff main...HEAD --stat` or `git log --oneline` to see what was done
2. **Write the compound document** to `docs/solutions/<YYYY-MM-DD>-<feature-slug>.md` (use today's date)
   - This goes in `docs/solutions/`, NOT `.claude/docs/` — it is meant to be committed
3. **Present the document** to the user for review before committing

## Document Format

```markdown
# <Feature/Fix Title>

**Date**: <YYYY-MM-DD>
**Type**: feature | bugfix | refactor

## Summary
<2-3 sentences: what was built/fixed and why>

## What Changed
- <file or module>: <what changed and why>
- ...

## Key Decisions
- <decision>: <why this approach was chosen over alternatives>
- ...

## Discoveries
<Non-obvious things learned during implementation — API quirks, gotchas, performance characteristics, edge cases>

- <discovery>
- ...

## Testing / Validation
<How this was verified to work>
```

## Rules

- Keep it concise — this is a reference document, not a narrative
- Focus on the WHY behind decisions, not the WHAT (code already shows the what)
- Include only discoveries that would save someone time in the future
- Strip ephemeral details (task progress, back-and-forth discussion) — only keep durable knowledge
- Do NOT include code snippets unless they illustrate a non-obvious pattern
- Do NOT spawn sub-agents
- Create `docs/solutions/` directory if it doesn't exist
- This document SHOULD be committed to the repo
