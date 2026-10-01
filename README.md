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
  discovery/
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
    discovery/
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
  environments.md

discovery/
  product-discovery.md
  work-item-model.md

governance/
  ai-governance.md
  change-control.md

prompts/
  product-manager-prompt.md
  architect-prompt.md
  backend-implementer-prompt.md
  frontend-implementer-prompt.md
  reviewer-prompt.md
  devops-prompt.md

patterns/
  README.md
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

`product.md` is primarily owned by Product Manager. It captures functional
product decisions, user roles, Capability and Feature boundaries, Work Items,
and field-level rationale. Architect consumes it for technical scoping.

`decisions.md` is the shared decision log for confirmed product and technical
decisions and their open items. Product Manager records product entries;
Architect records technical entries.

`environments.md` documents the available environments (local, dev, staging, production), how to connect to them, the standard deployment workflow, and the verification paths available to agents. It is the single source of truth for that information: the bootstrap file, governance files, and role prompts point here rather than naming concrete hosts, addresses, credentials, or paths. Implementer and DevOps roles read this file before attempting runtime verification.

### `discovery/`

Defines the Product Discovery workflow and the Capability → Feature → Work Item
→ Implementation Task hierarchy. Product Discovery is a workflow, not a role.
Features and Work Items use the maturity states `IDEA`, `DISCOVERING`, `DEFINED`,
`REFINING`, `PRODUCT_READY`, `IMPLEMENTING`, and `DONE`.

### `governance/`

Defines the rules of work.

This includes how AI agents may operate, how changes are controlled, and what must happen before code is modified.

### `prompts/`

Contains reusable role prompts for different working modes.

Prompts are operational tools. Governance defines the rules; prompts apply those rules in concrete agent interactions.

### `patterns/`

Reusable architectural patterns that more than one project needs — how to build
something, written once instead of re-decided per project.

Patterns are reference material, not governance: they grant no permission, and
adopting one is a project decision recorded in that project's own decision log.
Pattern IDs use the `SF-P-` prefix so they cannot collide with project `D-`
decision IDs. See `patterns/README.md` for the index.

## Agent roles

SupaFactory uses explicit agent roles to avoid mixing analysis, architecture discussion, implementation, and review.

The roles are:

- `Product Manager` — product discovery, product definition, Work Item refinement, and product acceptance criteria
- `Architect` — technical architecture, system boundaries, and implementation scoping
- `Backend Implementer` — backend, database, API, and worker implementation within an approved scope
- `Frontend Implementer` — client-side implementation within an approved scope
- `Reviewer` — review of completed changes without modifying files
- `DevOps` — deployment, operational scripts, and runtime diagnostics within an approved scope

If no role is explicitly specified, the agent must start as `Architect`.

Product Manager and Architect may update the product and technical documents
assigned to their roles. They do not implement application changes; confirmed
product and technical decisions remain under human control.

The delivery flow is Product Manager → Architect → Backend/Frontend Implementer
→ Reviewer. DevOps remains a separate operational role. The Product Manager
hands `PRODUCT_READY` Work Items to the Architect. That state means product
behavior is sufficiently specified for technical scoping; implementation still
requires an Architect-scoped task and explicit human approval.

The Product Manager explores the known Feature boundary broadly before choosing
a Work Item. Discovery keeps confirmed decisions, proposed defaults,
assumptions, open questions, deferred ideas, and explicit exclusions visible.
In voice conversations it asks one real decision question at a time, retains
unresolved questions across turns, and offers a checkpoint between continued
broad discovery and Work Item refinement when discussion becomes mostly detail.

`DevOps` owns deployment mechanics, operational scripts, runtime diagnostics, and
anything that requires touching the running system. It is deliberately separate
from the Implementer roles so implementation agents cannot run operational
commands without explicit approval. A project without deployment complexity may
simply never invoke the role.

Role details live in the corresponding files under `prompts/`.

## How to use SupaFactory

For human contributors:

1. Start with this file.
2. Review `AI-BOOTSTRAP.md`.
3. Use `discovery/` and fill or refine the relevant product, architecture, and
   governance files.
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
