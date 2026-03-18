# Code Quality Standards

Apply these standards while writing and reviewing each step.

## File Size & Responsibility
- If a file you're editing or creating grows beyond ~300 lines, pause and ask: does this file have more than one reason to change? If yes, it should be split.
- If a new function exceeds ~40 lines, ask whether it's doing too much. Extract cohesive sub-operations into named helpers.
- Adding a lot of code to an existing file that wasn't expected by the plan is a signal — note it in Findings and consider whether a new file/module is the right call.

## SOLID Principles

**Single Responsibility (SRP)**
- Each class, module, or function should have one reason to change.
- Watch for: functions that both fetch data AND transform it AND render it; classes that handle business logic AND persistence AND formatting.
- If you catch an SRP violation mid-implementation, fix it and note it in Findings.

**Open/Closed (OCP)**
- New behavior should be addable without modifying existing, working code.
- Prefer extension points (strategies, hooks, config, inheritance) over editing existing logic.
- If you find yourself adding a new `if/switch` branch to core logic to accommodate this feature, ask whether a better abstraction exists.

**Liskov Substitution (LSP)**
- Subtypes must be usable wherever their base type is expected, without breaking behavior.
- Watch for: overriding methods that change preconditions, postconditions, or throw exceptions the base doesn't.
- If implementing an interface or extending a class, ensure the contract is fully honored.

**Interface Segregation (ISP)**
- Don't force callers to depend on methods they don't use.
- Watch for: large interfaces being partially implemented, objects being passed just to access one field.
- Prefer narrow, purpose-specific interfaces over fat ones.

**Dependency Inversion (DIP)**
- High-level modules should not depend on low-level modules — both should depend on abstractions.
- Watch for: business logic directly instantiating infrastructure (databases, HTTP clients, file I/O).
- Prefer injecting dependencies rather than hard-coding them. This also makes things testable.

## General Hygiene
- Don't leave dead code, commented-out blocks, or TODO stubs unless explicitly noted in the plan.
- Names should be intention-revealing — if you need a comment to explain what a variable or function does, rename it instead.
- Avoid deep nesting — prefer early returns, guard clauses, and extracted functions.
