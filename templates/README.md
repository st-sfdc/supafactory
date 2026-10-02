# Artifact templates

Use these Markdown templates for real project records. Replace placeholders;
do not create empty backlog records or directories just to complete the tree.
All locations are relative to the SupaFactory control root.

[The artifact model](../governance/artifact-model.md) is authoritative.

| Template | Target | Purpose |
|---|---|---|
| [Capability](capability.md) | `architecture/capabilities/<id>.md` | Broad durable area and Feature links. |
| [Feature](feature.md) | `architecture/features/<id>.md` | Maintained behavior, rules and maturity. |
| [Work Item](work-item.md) | `architecture/work-items/<id>.md` | Finite change and product acceptance. |
| [Discovery note](discovery-note.md) | `discovery/areas/<id>.md` or `discovery/features/<id>.md` | Exploration and unresolved choices. |
| [Implementation scope](implementation-scope.md) | `architecture/scopes/<id>.md` | Versioned technical boundaries and task bundle. |
| [Implementation Task](implementation-task.md) | `architecture/tasks/<id>.md` | Single-role assignment and evidence. |

`architecture/product.md` is the Product overview template. Shared architecture,
data, interface, environment and decision templates remain in `architecture/`.

Retain existing project IDs. New IDs must be unique within the project and use
a consistent project convention; these templates do not mandate renumbering.
Paths stay stable when status changes. Use readable relative Markdown links
from the actual record location; placeholder paths in these templates are
instructions to replace, not valid project links.

Keep one canonical maturity state in each Feature/Work Item record. Capability
grouping is optional when it adds no useful boundary. Scope and Task templates
default execution, commit, push and deployment to **not authorized**. A filled
template is not permission. Record only approval actually given by the human.
