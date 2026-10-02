# Product and delivery artifacts

This is the authoritative artifact contract. Bootstrap, discovery workflows and
role prompts reference it; they must not define conflicting storage or ownership.

## Durable specification and finite delivery

Product specifications describe what the product does and the rules users can
rely on. They are maintained across deliveries. They must distinguish currently
delivered behavior from confirmed future behavior, proposals and open questions.

| Artifact | Meaning | Canonical location | Owner |
|---|---|---|---|
| Product | Purpose, users, goals, boundaries and linked overview. | `architecture/product.md` | Product Manager |
| Capability | Broad, durable area of product responsibility. | `architecture/capabilities/<id>.md` | Product Manager |
| Feature | Concrete, coherent behavior and product rules within that area. | `architecture/features/<id>.md` | Product Manager |
| Discovery note | Exploration, alternatives, assumptions and unresolved questions. | `discovery/areas/<id>.md` or `discovery/features/<id>.md` | Product Manager |
| Work Item | Finite, independently useful change to implement or extend behavior. | `architecture/work-items/<id>.md` | Product Manager |
| Implementation scope | Versioned technical boundaries and delivery order for a Work Item. | `architecture/scopes/<id>.md` | Architect |
| Implementation Task | Bounded technical or operational assignment to one responsible role. | `architecture/tasks/<id>.md` | Architect; limited result/review ownership below |

Capability → Feature → Work Item → Implementation Task is a breakdown, not a
requirement to invent artificial layers. Use Capability files when broad areas
provide useful grouping. A small product may link Features directly from its
Product overview until meaningful Capability boundaries are established.

A Capability groups behavior; a Feature specifies behavior. Neither is a
one-time implementation ticket. A Work Item describes the specific change from
the current state, not the complete lasting Feature specification. A Feature
may have many Work Items; a cross-cutting Work Item names one primary Feature
and links every other affected Feature. Do not duplicate the Work Item.

For example, a durable guest-navigation Feature may receive a first portal
Work Item, then another Work Item for improved navigation. After delivery the
Feature is updated to describe the current behavior and rules. Completed Work
Items remain as delivery history with links to scope, tasks and evidence; they
are not the primary source for learning how the product currently works.

## One source for each kind of fact

- Product overview: short description and links, not a combined specification.
- Capability: area purpose, boundaries and links to Features.
- Feature: maintained behavior, rules, delivered/planned distinction and links
  to discovery and Work Items.
- Work Item: delta, bounded behavior and product acceptance criteria.
- Scope: technical design for that delta, boundaries, roles and sequencing.
- Task: exact assignment, permitted actions and implementation/review evidence.
- `architecture/architecture.md`, `data-model.md`, `backend-interface.md`
  and `environments.md`: shared technical architecture and contracts.
- `architecture/decisions.md`: long-lived confirmed product/technical decisions,
  rationale and linked unresolved decision questions.

Keep stable IDs and paths when maturity changes. Existing IDs are retained.
Project overviews link to canonical records; do not maintain a second editable
status there. Optional GitHub Issues link to these files for execution tracking,
rather than becoming a competing specification or approval source.

Local exploratory questions belong in the relevant discovery note. Questions
requiring a durable product/technical decision use the shared decision log and
are linked from the note. This is one decision log, not separate product and
technical logs. Discussion transcripts and one-time execution approvals do not
belong in that log.

## Maturity and execution

Keep the existing seven Feature/Work Item maturity states defined in
[the Work Item model](../discovery/work-item-model.md). Each record owns its
state independently. Moving files or reorganizing content never changes status,
confirms assumptions, or authorizes implementation.

A Feature can stay `DEFINED` while a child Work Item is `DONE`. Feature
`DONE` refers to its currently agreed behavior; the file remains maintained
and future changes can create new Work Items. Finishing all current tasks does
not automatically mark the Work Item or parent Feature accepted.

Tasks separately track execution: Proposed, Authorized, In progress, Blocked,
Completed, Cancelled. Completed means the assignee finished the assignment;
the review outcome and product acceptance are separate evidence. Do not
conflate these execution markers with product maturity.

## Versioned authorization

Implementation requires an Architect-scoped assignment and explicit human
authorization matching that assignment. A role name, `PRODUCT_READY`, a
recommendation or an agent handoff does not provide authorization.

Record execution authorization in the exact scope or task, not in
`decisions.md`. Include:

- scope ID and version, task IDs, responsible role, repository and target;
- approving human, date, exact source reference or brief quote;
- permitted file/area boundaries and actions;
- separate explicit permissions for commit, push, deploy and operational work;
- limits, exclusions and stop conditions.

Authorization may cover an enumerated bundle of tasks once in its scope; each
task links that scope version and authorization record. Do not copy approvals
into several editable places. Unapproved actions default to not permitted.
Written task records preserve authorization given directly in the conversation;
they must not invent approval or broaden the human's words.

Freeze an authorized version's boundaries and preserve its approval evidence.
A material change creates a new scope version with matching human authorization
before changed work proceeds. Result and review sections can be appended without
changing the approved assignment. Interrupted, unfinished work may resume under
the same valid authorization without asking for the same approval again.
Completion ends the active assignment; evidence remains historical. A new run,
new task, changed scope or new operational action needs applicable authorization
and cannot reuse a completed assignment as an open-ended permission.

## Ownership and handoff

Product Manager owns Product, Capability, Feature, Work Item and project
Discovery notes, product maturity and acceptance criteria. Architect owns shared
technical documents, implementation scopes and task definitions. Each records
their own long-lived decisions in the shared log. Neither silently changes the
other role's specification.

Frontend/Backend Implementer and DevOps may append evidence only to their
assigned task's **Execution result** section, within the approved assignment.
Reviewer may append only the **Review** section of the assigned task when the
review assignment authorizes that documentation. This limited exception does
not authorize changes to definitions, approvals, product maturity, shared
architecture or code outside the implementation scope.

The Architect maintains task execution markers based on assignee evidence.
Product Manager records acceptance and updates the lasting product
specification based on confirmed delivery and human acceptance. Reviewer
reports findings; it does not implement fixes or declare product requirements.

Every handoff identifies the repository, exact task, scope version,
authorization record, Feature/Work Item, dependencies, relevant contracts,
allowed paths/actions, checks and stop point. Startup loads global rules plus
that linked context; it does not require reading the entire backlog.

## Adoption and migration

Use [the templates](../templates/README.md) to create records only when needed.
Folders need not exist until their first real record. Framework workflow files
in `discovery/` are human-owned rules; project notes under `discovery/areas/`
and `discovery/features/` are Product Manager-owned content.

Adopting this model in an existing product is a separately scoped documentation
migration. Preserve IDs, confirmed requirements, maturity, decisions,
authorization sources and implementation evidence. Reconcile superseded
requirements explicitly from human decisions; moving text does not resolve
contradictions. Framework updates alone do not migrate application repositories.
