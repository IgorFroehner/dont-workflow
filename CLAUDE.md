# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository. Codex-specific guidance lives in `AGENTS.md`.

## What This Repo Is

A Claude Code and Codex plugin (`dw`) providing lightweight engineering workflow skills. It is a pure-markdown plugin — no build step, no dependencies, no runtime. All skills are prompt files loaded by the host agent.

## Structure

```
.claude-plugin/plugin.json   # Claude Code plugin manifest
.codex-plugin/plugin.json    # Codex plugin manifest
skills/<name>/SKILL.md       # Shared skill prompt files
```

Each `SKILL.md` has a YAML frontmatter block followed by the skill prompt:

```yaml
---
name: <skill-name>
description: <shown in Claude Code UI>
argument-hint: <shown as placeholder when invoking>
user-invocable: true
disable-model-invocation: true
---
```

## Adding or Modifying Skills

To add a skill: create `skills/<name>/SKILL.md` with the frontmatter above and a prompt body.

To modify a skill: edit the relevant `SKILL.md`. Changes take effect immediately — no reload needed.

## Skill Conventions

- Skills write working files to `docs/dw/` (ephemeral, not committed)
- Final artifacts go to `docs/solutions/` (committed)
- Skills do their own research with Read/Grep/Glob — no sub-agents
- Skills pause and ask the user rather than guess at missing context

## Workflows

**Feature workflow**: `/dw:brainstorm` → `/dw:specify` → `/dw:plan` → `/dw:implement` → `/dw:qa` → `/dw:review` → `/dw:compound`

**Fix workflow**: `/dw:investigate` → `/dw:fix`
