# Product hierarchy and Work Item model

The product-to-implementation hierarchy is:

```text
Capability → Feature → Work Item → Implementation Task
```

| Level | Meaning | Primary owner |
|---|---|---|
| Capability | Broad product or business area. | Product Manager |
| Feature | Coherent product behavior, explored broadly before selecting slices. | Product Manager |
| Work Item | Independently useful product slice that can progress toward implementation readiness. | Product Manager through `PRODUCT_READY` |
| Implementation Task | Technical breakdown of an Architect-scoped Work Item. | Architect during or after technical scoping |

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

Keep a Work Item in `architecture/product.md` with:

- parent Capability and Feature, title, and maturity state;
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
