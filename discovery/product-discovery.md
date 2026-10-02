# Product Discovery

Product Discovery is the Product Manager's workflow for turning a product idea
into a product-defined Work Item. It is not an agent role and grants no
implementation permission.

## Explore the Feature boundary

Begin with useful Capability grouping, if established, and the Feature's
intended users, problem, outcomes, behaviors, variants, and edge cases. Capture the complete *known* Feature
boundary before optimizing for the smallest implementation. Unknown areas stay
visible; discovery need not pretend to be exhaustive.

Keep these categories distinct in the linked Feature/Work Item specification
and Discovery note. `architecture/product.md` remains a short Product overview.
Canonical paths and ownership are in `governance/artifact-model.md`:

- **Confirmed decisions:** accepted product behavior belongs in the maintained
  specification; record long-lived decisions and rationale in
  `architecture/decisions.md` and reference their IDs.
- **Proposed defaults:** sensible low-impact choices offered to the human for
  acceptance or correction; never silently treat them as confirmed.
- **Assumptions:** unverified beliefs on which the proposed behavior depends;
  state what would validate or change them.
- **Open questions:** local exploration stays in the Discovery note; questions
  requiring a durable decision use linked entries in the shared
  `architecture/decisions.md` Open items table.
- **Deferred ideas:** potentially useful future behavior, with the reason or
  trigger for revisiting it.
- **Explicit out-of-scope items:** behavior excluded from the current Feature or
  Work Item, even if it appears related.

Do not create another decision log. The Product Manager records product
decisions and open items in `architecture/decisions.md`; the Architect records
technical decisions and open items there.

## Run a voice-friendly conversation

Ask at most **one real decision question at a time**. Maintain unresolved
questions across turns rather than asking the human to remember a list. Remove
answered questions, incorporate partial answers, and retain the unanswered
portion. Periodically summarize confirmed decisions, assumptions and proposed
defaults, deferred items, remaining open items, and the next single question.
Summaries may contain several facts and open items, but must not end with a
bundle of decisions to answer. Prefer dialogue over form filling.

Propose visible defaults when a low-impact choice would keep discovery moving.
The human can accept or correct them. Until accepted, they remain proposals.

## Checkpoint and refinement

When new discussion yields mostly detail rather than new Feature areas,
explicitly offer a checkpoint: continue broad discovery, or choose a Work Item
and refine it. The human selects the path. A Work Item is an independently
useful product slice; deferred portions of the wider Feature remain visible.

For the selected Work Item, specify users, trigger, observable behavior,
important variants and failure cases, boundaries, dependencies, and product
acceptance criteria. Resolve or explicitly bound product questions that would
change that behavior. Use the maturity model and readiness checklist in
`discovery/work-item-model.md`.

## Architect handoff

When the selected Work Item reaches `PRODUCT_READY`, hand its product definition,
acceptance criteria, confirmed decisions, visible assumptions/defaults, open
items, exclusions, and deferred parts of the Feature to the Architect.
`PRODUCT_READY` is a product handoff, not implementation approval. The Architect
must define technical scope and Implementation Tasks; the human must approve
that scope before an Implementer starts.

## Persistent records

Use one maintained Feature file for behavior and rules across deliveries. Keep
exploration in linked project Discovery notes. Confirmed conclusions feed the
Feature or selected Work Item; source notes remain traceable. A Work Item
specifies a finite change, then retains its delivery history after acceptance.
Update the Feature's delivered/planned distinction from confirmed evidence;
never infer acceptance merely because an implementation agent finished.
Use `templates/` for readable records rather than expanding the Product index.
