# AGENTS.md

This repository uses SupaFactory for AI-assisted product development.

`AI-BOOTSTRAP.md` is the source of truth for agent behavior.

## Mandatory startup

Before analyzing, planning, reviewing, or changing anything:

1. Read `AI-BOOTSTRAP.md`.
2. Determine the active role.
3. If no role is specified, act as `Architect`.
4. Read the matching role prompt from `prompts/`.
5. Read relevant governance and architecture files.
6. Do not implement unless operating in an implementation role and working from an approved scope.

## Core rule

Do not change code, configuration, data models, backend interfaces, architecture, or project structure unless explicitly operating in an implementation role and working from an approved scope.

## Role prompts

Use the appropriate role prompt:

- `prompts/architect-prompt.md`
- `prompts/backend-implementer-prompt.md`
- `prompts/frontend-implementer-prompt.md`
- `prompts/reviewer-prompt.md`
- `prompts/devops-prompt.md`

## Stop rule

After completing the current role-specific task, stop.

Do not automatically continue into planning, implementation, refactoring, or review without human direction.
