---
name: plan
description: Create a detailed implementation plan for a feature, based on requirements or a direct description. Use after specifying requirements or when you know what to build and need to figure out how.
argument-hint: [feature description, or spec file path]
user-invocable: true
disable-model-invocation: true
---

# Implementation Plan

You are a software architect creating an actionable implementation plan.

## Process

1. **Search for prior art** — before anything else:
   - Glob `docs/solutions/*.md` and review the filenames
   - For any files that seem relevant to the user's topic (by semantic understanding, not just keyword matching), read them and extract: **Discoveries**, **Key Decisions**, and notable gotchas
   - Present the result as a **Prior Art** block before proceeding. If no files are relevant, skip this block silently.

2. **Gather context**:
   - If `$ARGUMENTS` points to a spec file (e.g. `docs/dw/*-spec.md`), read it
   - If `$ARGUMENTS` is a feature description, use it directly — the user can skip brainstorm/specify and jump straight here when they already know what they want
   - Read relevant existing code to understand the current architecture (use Glob/Grep/Read — do NOT spawn sub-agents)
3. **Clarify technical ambiguities** — BEFORE writing the plan:
   - If there are multiple valid implementation approaches (e.g. polling vs events, sync vs async, new class vs extending existing), use `AskUserQuestion` with each approach as an option — use the `preview` field to show a brief comparison (trade-offs, complexity, fit). Mark the recommended approach with "(Recommended)".
   - If non-functional requirements or scope boundaries are unclear, include these as additional questions in the same `AskUserQuestion` call when they have clear alternatives. Use regular text for open-ended questions.
   - Keep it to one round of questions, not a drip feed
   - If the user doesn't have a preference, note your chosen approach and why in the plan
4. **Identify the work**:
   - Break the feature into ordered implementation steps
   - For each step, identify which files need to change and what changes are needed
   - Flag any risks or unknowns
5. **Write the plan** to `docs/dw/<YYYY-MM-DD>-<feature-slug>-plan.md` (use today's date)
6. **Present a summary** to the user and ask for approval before considering it final. Once approved:
   - Suggest the user compact the context before implementing, to keep things focused. Give them the exact command to run, substituting the actual file paths:
     ```
     /compact Keep the full contents of <spec-file-path> and <plan-file-path> in context. Discard the rest of the conversation.
     ```
   - Then tell them to run `/dw:implement` after compacting.

## Plan Document Format

```markdown
# Plan: <feature name>

## Overview
<Brief description of what we're building and why>

## Steps

### Step 1: <title>
**Files**: `path/to/file.cpp`, `path/to/file.h`
**Changes**: <what to do>
**Notes**: <gotchas, risks, or design decisions>

### Step 2: <title>
...

## Testing Strategy
- <how to verify each step works>

## Risks & Open Questions
- <known risks>
- <decisions that may need revisiting>

## Findings
<This section is updated during /dw:implement with discoveries made during implementation>
```

## Rules

- Keep steps small and independently verifiable when possible
- Each step should be completable in one coding session
- Reference specific files, functions, and line numbers from the actual codebase
- Do NOT write code — only describe what to do
- Do NOT spawn sub-agents — do your own research with Read/Grep/Glob
- If the codebase is unfamiliar, spend time reading before planning
- Prefer modifying existing code over creating new files
- Consider the project's existing patterns and conventions (check CLAUDE.md)
