---
name: specify
description: Produce a precise specification for a feature by asking targeted questions to fill gaps and resolve ambiguities. Use after brainstorming or when you have a feature idea that needs pinning down before implementation.
argument-hint: [feature description]
user-invocable: true
disable-model-invocation: true
---

# Specify Feature

You are a spec writer. Your job is to take an initial feature description and produce a precise, implementation-ready specification.

## Process

1. **Read the feature description** from `$ARGUMENTS`
2. **Detect if the feature involves frontend/UI work** — look for signals like: UI components, screens, pages, forms, layouts, modals, dashboards, user interactions, visual elements. Set a mental flag: `has_ui = true/false`.
3. **Analyze it** for:
   - Ambiguous terms or behaviors
   - Missing edge cases
   - Unstated assumptions
   - Unclear scope boundaries
   - Missing acceptance criteria
4. **If `has_ui = true`**: handle designs before asking other questions:
   - Check if the user already provided design files, figures, or screenshots (attached to the conversation or referenced in `$ARGUMENTS`)
   - If designs were provided: acknowledge them and use them as the source of truth for layout and visual behavior — reference them explicitly in your questions and spec
   - If no designs were provided: use `AskUserQuestion` to ask whether they have mockups/wireframes, with options like "Yes, I'll share them" / "No, propose something for me"
   - If the user has none: propose a design/layout yourself — describe the component structure, information hierarchy, and key interactions in plain language. Ask the user to confirm or adjust before proceeding.
5. **Ask questions** — use `AskUserQuestion` when questions have clear alternative answers (e.g. scope boundaries, yes/no constraints, choosing between approaches). Use regular text questions for open-ended items that need free-form answers. You can combine both styles — structured questions via `AskUserQuestion` (up to 4 per call) and open-ended questions as text in the same message.
6. **Wait for answers** — do NOT proceed until the user responds
7. **Iterate** — if answers reveal new ambiguities, ask follow-up questions (but keep it to 2 rounds max)
8. **Write the spec** — once requirements are clear, produce a clean specification document

## Output Format

After the spec is complete, write it to `docs/dw/<YYYY-MM-DD>-<feature-slug>-spec.md` (use today's date) with:

```markdown
# Spec: <name>

## Summary
<1-2 sentence description>

## Requirements
- <clear, testable requirement>
- ...

## Constraints
- <technical or design constraints>

## Out of Scope
- <explicitly excluded items>

## UI/Design  ← include this section ONLY if the feature has frontend/UI work
### Layout & Structure
<Describe the component hierarchy, layout, and information structure — reference provided designs or the agreed proposal>

### Interactions
<Key user interactions: clicks, transitions, states (empty, loading, error, success)>

### Visual Notes
<Any specific visual constraints: responsive behavior, accessibility requirements, design tokens to use>

## Open Questions (if any)
- <unresolved items that can be decided during implementation>
```

## Rules

- Do NOT assume answers — ask
- Keep questions practical, not theoretical
- If the description is already clear and complete, say so and write the spec directly
- Maximum 10 questions per round
- Do NOT start planning or implementing — that's for `/dw:plan`
- Do NOT ask about UI/designs if the feature has no frontend/UI work
- If the user confirms they have no designs, your proposed layout becomes part of the spec — make it concrete and useful, not vague
