# AGENTS.md

This repository contains `dont-workflow`, a Markdown-only workflow plugin for AI coding agents.

## Structure

```text
.claude-plugin/plugin.json  # Claude Code plugin manifest
.codex-plugin/plugin.json   # Codex plugin manifest
skills/<name>/SKILL.md      # Shared skill prompt files
```

The skill files are intentionally shared between Claude Code and Codex. Keep instructions agent-neutral when possible:

- Refer to the user's request or provided prompt instead of `$ARGUMENTS`.
- Ask concise plain-text questions unless a host environment provides a structured question UI.
- Refer to normal codebase search and file-reading tools instead of tool-specific names.
- Keep Claude slash-command examples in user docs, not as required behavior inside skill prompts.

## Workflow

Feature workflow: brainstorm -> specify -> plan -> implement -> qa -> review -> compound

Fix workflow: investigate -> fix

Working artifacts go in `docs/dw/`. Permanent solution records go in `docs/solutions/`.
