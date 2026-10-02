# Product hierarchy and Work Item model

The product-to-implementation hierarchy is:

```text
Capability → Feature → Work Item → Implementation Task
```

| Level | Meaning | Primary owner |
|---|---|---|
| Capability | Broad, lasting product area that groups Features when useful. | Product Manager |
| Feature | Lasting behavior and rules, maintained across deliveries. | Product Manager |
| Work Item | Finite, independently useful change to that behavior; retained as delivery history. | Product Manager |
| Implementation Task | Technical breakdown of an Architect-scoped Work Item. | Architect during or after technical scoping |

Use `governance/artifact-model.md` for storage, ownership and versioned
authorization. Capability grouping is used when it clarifies broad areas; do not
invent an artificial level for a small product. Scope records hold technical
boundaries between Work Items and their Implementation Tasks.

The Product Manager does not create Implementation Tasks. A Feature may have
multiple Work Items. Readiness of one Work Item does not imply readiness of the
whole Feature or its other Work Items.

## Feature and Work Item maturity

Track each Feature and Work Item independently with one of these states:

| State | Meaning |
|---|---|
| `IDEA` | Candidate behavior is named; its boundary is not yet explored. |
| `DISCOVERING` | Users, outcomes, and the known Feature boundary or Work Item slice are being explored. |
| `DEFINED` | Product intent and known boundary are captured; details or slices may remain open. |
| `REFINING` | A selected Feature or Work Item is being specified for readiness. |
| `PRODUCT_READY` | Product behavior and acceptance criteria are sufficient for Architect handoff. |
| `IMPLEMENTING` | Architect scoping and human implementation approval have occurred; approved tasks are in progress. |
| `DONE` | The approved product behavior is delivered and reviewed. |

The sequence describes increasing maturity, not an automatic state machine.
A Feature may stay `DEFINED` while one of its Work Items becomes
`PRODUCT_READY` or `IMPLEMENTING`. A state change does not silently confirm a
proposed default or assumption. `PRODUCT_READY` never authorizes code changes;
the Architect must scope implementation and the human must approve it.

## Work Item record

Keep each Work Item in `architecture/work-items/<id>.md`, linked from its
Feature file and the short Product overview as appropriate, with:

- primary Feature, Capability if established, title, and maturity state;
- current behavior and intended change, plus any other affected Features;
- user, problem, useful outcome, trigger, and observable behavior;
- included behavior, important variants and failure cases, and explicit
  out-of-scope behavior;
- product acceptance criteria that a human can check against the behavior;
- confirmed product decision IDs, visible proposed defaults and assumptions,
  and open item IDs from `architecture/decisions.md`;
- dependencies and deferred related ideas.

Before marking `PRODUCT_READY`, check that the selected slice is independently
useful, its product behavior and acceptance criteria are clear, and unresolved
questions are either resolved or explicitly bounded so they cannot silently
change the slice. Technical design, API shape, schema, implementation order,
and Implementation Tasks belong to the Architect handoff, not this record.

## Delivery history and current behavior

A completed Work Item preserves its accepted delivery, scope/task links and
verification evidence. It is not the maintained full Feature specification.
After confirmed acceptance, Product Manager updates the Feature's current
behavior and separates remaining planned behavior. Further changes create new
Work Items; a Feature is not closed forever by one delivery.

Product maturity, task execution and review outcomes are distinct. Each has its
canonical record as defined in `governance/artifact-model.md`; do not copy status
into several editable index tables.
