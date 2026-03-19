# dont-workflow

Stop guessing. Start shipping.

A Claude Code plugin that gives you a structured engineering workflow — from fuzzy idea to production code — without spawning a fleet of sub-agents. Pure markdown, zero dependencies.

## Install

Clone the repo and add it as a Claude Code plugin:

```bash
git clone https://github.com/igorfroehner/dont-workflow.git
```

Then in Claude Code, add the plugin path to your settings.

## Workflows

### Build a Feature

```
╔══════════════════════╦════════════════════════════════════════╦══════════════╗
║   Pre-Engineering    ║            Engineering                 ║     Post     ║
╠══════════════════════╬════════════════════════════════════════╬══════════════╣
║  brainstorm  specify ║  plan  →  implement  →  qa  →  review  ║  compound    ║
║                      ║    ↑_______↓                           ║              ║
╚══════════════════════╩════════════════════════════════════════╩══════════════╝
         ↓                              ↓                              ↓
   What to build               How to build it               What was built
```

Start broad, get specific, then execute. Each step produces a document that feeds the next.

| Skill | What it does |
|-------|-------------|
| **brainstorm** | Explore 2-4 directions for a problem. Pick one before committing. |
| **specify** | Turn the chosen direction into a precise, testable spec. Handles UI/design flows too. |
| **plan** | Break the spec into ordered implementation steps with specific files and changes. |
| **implement** | Execute the plan step by step with built-in quality checks (linting, SOLID, file size). |
| **qa** | Take feedback from you, product, or design — find the root cause, fix it, and amend the plan. |
| **review** | Review code for correctness, safety, and design — by file, staged changes, last commit, or PR. |
| **compound** | Write a permanent record of what was built and why. Committed to the repo. |

### Fix an Issue

```
/dw:investigate  →  /dw:fix
```

| Skill | What it does |
|-------|-------------|
| **investigate** | Form hypotheses, gather evidence, find root causes. Asks for logs/context before concluding. |
| **fix** | Apply targeted fixes from the investigation. Only touches what you ask it to. |

## Why This Exists

Claude Code is powerful, but without structure it tends to guess, over-scope, and lose context. This plugin adds guardrails:

- **Skills don't bleed into each other.** `/fix` only fixes. `/implement` follows the plan. Each skill stays in its lane.
- **Ask before assuming.** Skills pause and ask rather than filling gaps with guesses.
- **No sub-agents.** Every skill does its own research with Read/Grep/Glob. Predictable, debuggable.
- **Lightweight docs.** Working files go in `docs/dw/` (ephemeral). Final records go in `docs/solutions/` (committed).

## Skill Details

### `/dw:brainstorm [problem or idea]`

Use this when you have a problem but don't know how to solve it yet. Brainstorm explores the solution space — proposes 2-4 directions with honest pros/cons, gives a recommendation, and captures the chosen direction. Hand off to `/dw:specify` once you've picked.

### `/dw:specify [feature description]`

Turns a feature idea into a precise spec. Asks targeted questions about scope, behavior, edge cases, and constraints (max 2 rounds). If the feature involves UI, it handles designs — uses provided mockups or proposes a layout if you have none.

### `/dw:plan [feature or spec file]`

Reads the codebase, asks about technical trade-offs, then produces an ordered implementation plan. Each step identifies specific files, functions, and changes. Suggests compacting context before implementation.

### `/dw:implement [plan file or step number]`

Follows the plan step by step. Auto-detects quality checks for your stack (TypeScript, Python, Rust, Go, Ruby, C/C++, and more) and runs them after each step. Applies SOLID principles and flags structural issues. Documents discoveries as it goes. Can resume from any step.

### `/dw:qa [feedback description]`

Use this after implement whenever something looks off — spotted by you, product, or design. Describe the issue in plain language: `/dw:qa the button is not using the correct layout`. The skill finds the root cause, applies a targeted fix, and appends a QA Amendment to the plan file so the change is captured. Run it as many times as needed — once per piece of feedback.

### `/dw:review [file, "staged", "last", or PR number]`

Reviews code for correctness, safety, and quality. Findings organized by severity: Critical / Important / Minor / Good. References specific `file:line` locations. Won't nitpick style or suggest unrelated refactors.

### `/dw:compound [feature name or plan file]`

Produces a permanent record of what was built — decisions, discoveries, and what changed. Saved to `docs/solutions/` for future developers and LLMs.

### `/dw:investigate [issue description]`

Systematic debugging. Forms multiple hypotheses, challenges each with evidence, and asks for context it can't get from code alone (logs, query plans, repro steps). Marks assumptions explicitly.

### `/dw:fix [investigation file] [finding numbers]`

Applies targeted fixes from an investigation. Only fixes what you ask — `/dw:fix investigation.md 1 2` skips finding 3. Confirms scope before touching anything, provides validation steps after.
