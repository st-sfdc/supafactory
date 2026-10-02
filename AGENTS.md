# AGENTS.md

This repository uses SupaFactory for AI-assisted product development.

`AI-BOOTSTRAP.md` is the source of truth for agent behavior.

## Mandatory startup

Before analyzing, planning, reviewing, or changing anything:

1. Read `AI-BOOTSTRAP.md`.
2. Determine the active role.
3. If no role is specified, act as `Architect`.
4. Read the matching role prompt from `prompts/`.
5. Read relevant governance and architecture files, including
   `governance/artifact-model.md` and the assigned task's linked context.
6. Do not implement unless operating in an implementation role and working from an approved scope.

## Core rule

Changes to application code, runtime configuration, persisted schemas, backend
interface implementations, or non-documentation project structure require the
appropriate implementation or DevOps role and an approved scope. Product
Manager and Architect may update their role-owned documentation as defined in
`AI-BOOTSTRAP.md` and their prompts. Documentation ownership does not authorize
application implementation or unapproved decisions.

## Role prompts

Use the appropriate role prompt:

- `prompts/product-manager-prompt.md`
- `prompts/architect-prompt.md`
- `prompts/backend-implementer-prompt.md`
- `prompts/frontend-implementer-prompt.md`
- `prompts/reviewer-prompt.md`
- `prompts/devops-prompt.md`

The delivery flow is Product Manager → Architect → Backend/Frontend
Implementer → Reviewer. DevOps is a separate operational role. Product
Discovery is a workflow owned by Product Manager, not an agent role. A
`PRODUCT_READY` Work Item goes to Architect for technical scoping; it does not
authorize implementation. The human must approve the Architect-scoped task
before an Implementer starts.

## Stop rule

After completing the current role-specific task, stop.

Do not automatically continue into planning, implementation, refactoring, or review without human direction.
