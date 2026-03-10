---
name: implement
description: Implement a feature following an existing plan document, documenting findings along the way.
argument-hint: [plan file or step number]
user-invocable: true
disable-model-invocation: true
---

# Implement from Plan

You are a developer implementing a feature by following an existing plan.

## Process

1. **Load the plan**:
   - If `$ARGUMENTS` contains a path to a plan file, read it
   - If `$ARGUMENTS` contains a step number (e.g. "step 3" or "3"), find the most recent plan in `.claude/docs/` (files named `<YYYY-MM-DD>-<feature>-plan.md`) and jump to that step
   - If `$ARGUMENTS` is just a feature name, look for the most recent matching `*-<feature>-plan.md` in `.claude/docs/`
2. **Implement step by step**:
   - Follow the plan's steps in order (or from the specified step)
   - After completing each step, briefly tell the user what was done
   - If you discover something unexpected (API difference, better approach, edge case), document it in the plan's **Findings** section immediately
3. **Build and verify** after each step when possible
4. **Update the plan** as you go:
   - Mark completed steps with ✅
   - Add findings to the **Findings** section
   - If a step needs to change, update it and note why

## Rules

- Follow the plan — do NOT redesign mid-implementation unless you hit a blocker
- If you hit a blocker, stop and explain it to the user. Let them decide whether to adjust the plan
- Document ALL non-obvious discoveries in the plan's Findings section (API quirks, gotchas, workarounds)
- Do NOT skip steps or combine steps without telling the user
- Do NOT spawn sub-agents — do the work directly
- Keep commits atomic — one logical change per step when the user asks to commit
- After finishing all steps, summarize what was done and what's in Findings
