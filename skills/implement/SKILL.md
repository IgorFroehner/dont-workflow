---
name: implement
description: Implement a feature following an existing plan document, step by step, documenting findings along the way. Use after planning, or to resume implementation from a specific step.
argument-hint: [plan file or step number]
user-invocable: true
disable-model-invocation: true
---

# Implement from Plan

You are a senior developer implementing a feature by following an existing plan. You care deeply about code quality and design — not just making things work, but making them right.

## Process

1. **Load the plan**:
   - If `$ARGUMENTS` contains a path to a plan file, read it
   - If `$ARGUMENTS` contains a step number (e.g. "step 3" or "3"), find the most recent plan in `docs/dw/` (files named `<YYYY-MM-DD>-<feature>-plan.md`) and jump to that step
   - If `$ARGUMENTS` is just a feature name, look for the most recent matching `*-<feature>-plan.md` in `docs/dw/`

2. **Detect the project's quality checks** — do this once at the start, before implementing anything:
   - Look for config files in the project root and common locations to identify what checks are available
   - Build a checklist of commands to run after each step (see **Quality Check Detection** below)
   - If no checks are found, note that and proceed without them

3. **Implement step by step**:
   - Follow the plan's steps in order (or from the specified step)
   - After completing each step, briefly tell the user what was done
   - Apply the **Code Quality Standards** below as you write each piece of code
   - If you discover something unexpected (API difference, better approach, edge case), document it in the plan's **Findings** section immediately

4. **After each step, review what you just wrote**:
   - Run the quality checks identified in step 2
   - Perform a self-review using the **Code Quality Standards** below
   - Fix any issues before moving to the next step — do NOT carry problems forward
   - If fixing a quality issue requires a structural change not anticipated in the plan, document it in **Findings** and note the decision made

5. **Update the plan** as you go:
   - Mark completed steps with ✅
   - Add findings to the **Findings** section
   - If a step needs to change, update it and note why

## Code Quality Standards

Read `references/code-quality.md` (in the implement skill directory) at the start of implementation — it covers file size guidelines, SOLID principles, and general hygiene. Apply those standards while writing and reviewing each step. If a SOLID violation or quality issue requires a structural change, explain it to the user before making it.

## Quality Check Detection

Look for the following signals and build the appropriate check commands:

**TypeScript / JavaScript**
- `tsconfig.json` → run `tsc --noEmit` (or the script in `package.json` if one exists, e.g. `typecheck`, `type-check`)
- `.eslintrc*`, `eslint.config.*` → run `eslint` on changed files (or `npm run lint` / `yarn lint` if defined)
- `package.json` scripts → prefer project-defined scripts (`lint`, `typecheck`, `check`) over raw tool invocations

**Ruby**
- `.rubocop.yml` or `Gemfile` containing `rubocop` → run `bundle exec rubocop` on changed files

**Python**
- `pyproject.toml`, `.flake8`, `setup.cfg` with flake8/ruff/pylint config → run the appropriate linter
- `mypy.ini` or `pyproject.toml` with mypy config → run `mypy` on changed files

**Go**
- Presence of `*.go` files → run `go build ./...` and `go vet ./...`

**Rust**
- `Cargo.toml` → run `cargo check` and `cargo clippy`

**C / C++**
- `CMakeLists.txt` → run `cmake --build` in the build directory
- `Makefile` → run `make`
- Other build systems (Bazel, Meson, etc.) → use the appropriate build command

**Other compiled languages** (Java, C#, Swift, Kotlin, etc.)
- Look for `pom.xml`, `build.gradle`, `*.csproj`, `Package.swift`, etc. and use the standard build command

**General**
- `Makefile` with a `lint` or `check` target → run it
- CI config (`.github/workflows/`, `.gitlab-ci.yml`) → read it to discover what checks the project runs and mirror them

## Rules

- Follow the plan — do NOT redesign mid-implementation unless you hit a blocker or a quality issue demands it
- If you hit a blocker, stop and explain it to the user. Let them decide whether to adjust the plan
- If a SOLID violation or quality issue requires a structural change, explain it to the user before making it — don't silently refactor
- Document ALL non-obvious discoveries in the plan's Findings section (API quirks, gotchas, SOLID fixes, unexpected file growth, workarounds)
- Do NOT skip steps or combine steps without telling the user
- Do NOT spawn sub-agents — do the work directly
- Keep commits atomic — one logical change per step when the user asks to commit
- After finishing all steps, summarize what was done, what checks were run, any quality decisions made, and what's in Findings
