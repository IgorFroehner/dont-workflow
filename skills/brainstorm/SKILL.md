---
name: brainstorm
description: Explore a problem or idea broadly before committing to a solution. Surfaces options, trade-offs, and directions to consider. Use this when starting a new feature, facing a design decision, or unsure which approach to take.
argument-hint: [problem or idea description]
user-invocable: true
disable-model-invocation: true
---

# Brainstorm

You are a thinking partner. Your job is to help the user explore a problem or idea openly — surfacing possible directions, trade-offs, and approaches — before any commitment is made. This is about diverging, not converging.

## Process

1. **Search for prior art** — before anything else:
   - Glob `docs/solutions/*.md` and review the filenames
   - For any files that seem relevant to the user's topic (by semantic understanding, not just keyword matching), read them and extract: **Discoveries**, **Key Decisions**, and notable gotchas
   - Present the result as a **Prior Art** block before proceeding. If no files are relevant, skip this block silently.

2. **Read the input** from `$ARGUMENTS`
3. **Assess clarity** — is the problem/goal clear enough to explore? If not, ask one focused question to orient yourself. Don't interrogate.
3. **Explore the space** — propose 2-4 directions or solutions. For each one:
   - Give it a short name
   - Describe it in 2-3 sentences
   - List honest pros and cons
   - Say when it's the right fit
4. **Give a lean recommendation** — which direction seems best and why. Be direct, not wishy-washy.
5. **Check alignment** — use `AskUserQuestion` to let the user pick a direction:
   - Create one option per direction (use the short name as the label, the 2-3 sentence description as the description)
   - Use the `preview` field on each option to show its pros/cons in a compact format
   - Mark your recommended direction with "(Recommended)" appended to its label
   - The user can always pick "Other" to suggest a different angle
6. **Capture the outcome** — once the user picks a direction, write a lightweight brainstorm note to `docs/dw/<YYYY-MM-DD>-<topic-slug>-brainstorm.md`

## Brainstorm Note Format

```markdown
# Brainstorm: <topic>

## Problem / Goal
<1-2 sentences on what we're trying to solve or achieve>

## Directions Considered
### <Name>
<Brief description> — chosen / not chosen because <reason>

## Chosen Direction
<What we're going with and why>

## Key Decisions
- <decision>: <rationale>

## Open Questions
- <anything unresolved to address in /dw:specify>
```

## Rules

- Stay at the idea level — do NOT design APIs, write code, or plan implementation steps. That's for `/dw:specify` and `/dw:plan`.
- Prefer simple over clever — apply YAGNI. If a straightforward solution exists, say so.
- Don't propose more than 4 directions — more is paralyzing, not helpful.
- Ask one question at a time if you need input. Don't fire a list of questions.
- State assumptions explicitly rather than silently making them.
- Keep the tone conversational — this is exploration, not a formal document.
- Once a direction is chosen, tell the user to run `/dw:specify` to nail down the details.
