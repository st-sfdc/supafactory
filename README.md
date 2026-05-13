# SupaFactory

SupaFactory is a lightweight framework for AI-assisted product development.

It provides a structured working model for building software products with AI coding agents such as Cursor, while keeping architectural, product, and delivery decisions under human control.

The goal is not to let agents autonomously build an application from vague prompts. The goal is to create a controlled development environment where agents work against explicit context, defined rules, small scoped tasks, and reviewable changes.

## Repository purpose

This repository contains the source version of the SupaFactory framework.

The framework files live directly at the repository root:

```text
supafactory/
  README.md
  AI-BOOTSTRAP.md
  architecture/
  governance/
  prompts/
```

In a concrete application project, SupaFactory may be copied into a dedicated project-control folder, for example:

```text
my-app/
  src/
  package.json
  supafactory/
    README.md
    AI-BOOTSTRAP.md
    architecture/
    governance/
    prompts/
```

## Core principle

AI agents must not silently make decisions or assume product behavior, architecture, backend interface structure, or data model design.

They must not perform large-scale changes based on unconfirmed assumptions.

They may assist with analysis, planning, implementation, refactoring, testing, and documentation — but only within explicit boundaries, in manageable increments, and after review and explicit confirmation.

The required working mode is:

1. Read the relevant SupaFactory context.
2. Determine the active agent role.
3. Read the matching role prompt from `prompts/`.
4. Understand the current system state.
5. State assumptions and uncertainties.
6. Propose a limited plan.
7. Wait for human confirmation, clarification, or questions.
8. Refine the plan if needed.
9. Implement only after explicit human approval.
10. Implement only the approved scope.
11. Show the resulting changes.
12. Stop.

## MVP structure

The initial supafactory MVP focuses on three areas:

```text
README.md
AI-BOOTSTRAP.md

architecture/
  architecture.md
  data-model.md
  backend-interface.md
  product.md
  decisions.md

governance/
  ai-governance.md
  change-control.md

prompts/
  planner-prompt.md
  architect-prompt.md
  backend-implementer-prompt.md
  frontend-implementer-prompt.md
  reviewer-prompt.md
```

## File roles

### `README.md`

Human entry point.

Explains what SupaFactory is, why it exists, and how the framework is intended to be used.

### `AI-BOOTSTRAP.md`

Agent entry point.

This is the first file an AI coding agent must read before analyzing, planning, or changing a project that uses SupaFactory.

It defines the global startup procedure, the default role, and the mapping from agent roles to role-specific prompt files.

### `architecture/`

Defines intentional system structure.

This includes high-level architecture, data model design, backend interface contracts, system boundaries, and stack-specific decisions.

`product.md` captures functional product decisions, user roles, feature scope, and field-level rationale.

`decisions.md` is the decision log — a running record of confirmed architectural decisions and deferred open items.

### `governance/`

Defines the rules of work.

This includes how AI agents may operate, how changes are controlled, and what must happen before code is modified.

### `prompts/`

Contains reusable role prompts for different working modes.

Prompts are operational tools. Governance defines the rules; prompts apply those rules in concrete agent interactions.

## Agent roles

SupaFactory uses explicit agent roles to avoid mixing analysis, architecture discussion, implementation, and review.

The MVP roles are:

- `Planner`
- `Architect`
- `Backend Implementer`
- `Frontend Implementer`
- `Reviewer`

If no role is explicitly specified, the agent must start as `Planner`.

Role details live in the corresponding files under `prompts/`.

## How to use SupaFactory

For human contributors:

1. Start with this file.
2. Review `AI-BOOTSTRAP.md`.
3. Fill or refine the relevant architecture and governance files.
4. Use role prompts from `prompts/` when delegating work to an AI agent.
5. Keep changes small and reviewable.

For AI agents:

1. Read `AI-BOOTSTRAP.md` first.
2. Determine the active role.
3. Read the matching role prompt from `prompts/`.
4. Follow the referenced governance files.
5. Load the relevant architecture files into context.
6. Do not change code before producing a plan and receiving approval.
7. Do not change code unless the active role explicitly allows implementation.

## Status

SupaFactory is currently in MVP definition.

The framework may evolve, but changes to SupaFactory itself should follow the same principle as application changes:

- one focused change at a time
- explicit rationale
- reviewable diff
- no hidden assumptions
