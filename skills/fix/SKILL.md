---
name: fix
description: Implement specific fixes from an investigation document. Only fixes what the user asks for.
argument-hint: [investigation file] [finding numbers to fix]
user-invocable: true
disable-model-invocation: true
---

# Fix from Investigation

You are a developer applying targeted fixes based on a completed investigation.

## Process

1. **Load the investigation**:
   - Read the investigation file from `$ARGUMENTS` (first argument, or find the most recent in `docs/dw/`)
   - Identify which findings the user wants fixed (from `$ARGUMENTS` or by asking)
2. **Confirm scope**:
   - List the findings and their proposed fixes
   - Ask: "I'll fix findings [N, M]. Correct?" — wait for confirmation
   - If the user didn't specify which findings, ask them
3. **Implement each fix**:
   - Apply the fix described in each finding
   - Build/compile after each fix to verify it doesn't break anything
   - If the fix needs adjustment from what was described in the investigation, explain why
4. **Provide validation steps**:
   - After all fixes are applied, give the user specific steps to verify the fix works
   - Include commands to run, things to test, or behaviors to check

## Output (after fixing)

```
## Fixes Applied

### Finding <N>: <title>
**Changed**: `file:line` — <what was changed>
**Status**: ✅ Fixed

### Finding <M>: <title>
**Changed**: `file:line` — <what was changed>
**Status**: ✅ Fixed

## Validation Steps
1. <specific step to verify fix N>
2. <specific step to verify fix M>
3. <regression check>
```

## Rules

- ONLY fix what the user asks for — if they say "fix 1 and 2", do NOT touch finding 3
- Follow the investigation's proposed fix unless you have a concrete reason not to
- If the proposed fix seems wrong or incomplete, explain why and suggest an alternative — wait for approval
- Build/compile after each fix
- Do NOT spawn sub-agents
- Do NOT refactor surrounding code — minimal, targeted changes only
- If a fix requires changes not anticipated in the investigation, flag it before doing it
