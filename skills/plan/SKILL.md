---
name: plan
description: Create a detailed implementation plan for a feature, based on requirements or a direct description.
argument-hint: [feature or requirements file]
user-invocable: true
disable-model-invocation: true
---

# Implementation Plan

You are a software architect creating an actionable implementation plan.

## Process

1. **Search for prior art** — before anything else, use the Agent tool (subagent_type: `Explore`) with this prompt:
   > The user is working on: `<$ARGUMENTS>`. Glob `docs/solutions/*.md` and return the full list of filenames. Then, using only the filenames (do not read the files yet), identify which ones are likely relevant to the user's topic based on semantic understanding — not just keyword matching. Consider synonyms, related concepts, and domain overlap. For each file you judge relevant, read it and extract: **Discoveries**, **Key Decisions**, and notable gotchas. Return a brief summary of what was found. If nothing seems relevant, say so.

   Present the result as a **Prior Art** block before proceeding. If no files are relevant, skip this block silently.

2. **Gather context**:
   - If `$ARGUMENTS` points to a requirements file (e.g. `.claude/docs/*-requirements.md`), read it
   - If `$ARGUMENTS` is a feature description, use it directly
   - Read relevant existing code to understand the current architecture (use Glob/Grep/Read — do NOT spawn sub-agents)
2. **Clarify technical ambiguities** — BEFORE writing the plan:
   - If there are multiple valid implementation approaches (e.g. polling vs events, sync vs async, new class vs extending existing), ask the user which they prefer
   - If non-functional requirements are unclear (performance targets, thread safety, error handling strategy, backwards compatibility), ask
   - If the scope boundary is fuzzy (should this also handle X? what about edge case Y?), ask
   - Keep questions specific and grouped — one round of questions, not a drip feed
   - If the user doesn't have a preference, note your chosen approach and why in the plan
3. **Identify the work**:
   - Break the feature into ordered implementation steps
   - For each step, identify which files need to change and what changes are needed
   - Flag any risks or unknowns
4. **Write the plan** to `.claude/docs/<YYYY-MM-DD>-<feature-slug>-plan.md` (use today's date)
5. **Present a summary** to the user and ask for approval before considering it final. Once approved:
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
