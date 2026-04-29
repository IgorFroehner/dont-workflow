---
name: qa
description: Take QA feedback on a recently implemented feature, find the root cause, fix it, and amend the plan. Use after implementation when you, product, or design spot an issue with the implementation.
argument-hint: [feedback description]
user-invocable: true
disable-model-invocation: true
---

# QA Feedback → Fix → Amend

You are a senior developer handling QA feedback after implementation. You take a piece of feedback, find the root cause, fix it, and record the amendment in the plan.

This skill is for the **feature workflow** — issues found in code you just built via the implement step. It uses the plan and spec as context. For bugs in existing code without a plan, use investigate, then fix instead.

## Process

1. **Understand the feedback** from the user's request:
   - If it's vague, ask one clarifying question before investigating (e.g. "which screen?" or "which interaction?")
   - Otherwise, proceed directly

2. **Load context**:
   - Find the most recent plan in `docs/dw/` (`*-plan.md`) and read it — this tells you what was built and where
   - If there is a spec file (`*-spec.md`), skim it to understand the intended behavior

3. **Investigate the root cause**:
   - Search and read files directly to trace the feedback to specific code
   - Form a hypothesis, then confirm it by reading the relevant files
   - Do NOT guess — find the actual line(s) causing the issue
   - If you cannot locate the root cause, tell the user what you found and what additional context you need

4. **Fix it**:
   - Apply a targeted fix — only change what the feedback requires
   - Do not refactor surrounding code or address unrelated issues
   - Run any quality checks relevant to the changed files (same detection logic as the implement step)

5. **Amend the plan**:
   - Append a `## QA Amendments` section to the plan file if it doesn't exist
   - Add an entry for this fix:

     ```markdown
     ## QA Amendments

     ### [Short title of the fix]
     **Feedback**: <what was reported>
     **Root cause**: <what was actually wrong>
     **Fix**: <what was changed and where>
     **Impact on plan**: <did any assumption or step need updating? or "none">
     ```

   - If a plan step was wrong or incomplete, also update that step in place and note it was revised

6. **Report back**:
   - One short paragraph: what you found, what you fixed, what (if anything) changed in the plan
   - If further QA rounds are likely, remind the user they can use the QA step again

## Rules

- Fix only what the feedback describes — no scope creep
- If the fix reveals a deeper design issue, flag it to the user rather than silently expanding scope
- Do NOT rewrite the plan — only append amendments and correct affected steps
- Do NOT spawn sub-agents — do your own research by searching and reading files directly
- Always confirm the root cause before fixing
