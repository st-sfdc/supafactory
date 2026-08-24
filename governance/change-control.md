# Change Control

This file defines the minimum rules for handling a single change set in a SupaFactory-governed project.

It is intentionally lightweight and may be expanded later.

## Purpose

Change Control defines the unit of work.

A change set should be small, explicit, approved, and reviewable.

## Change set

A change set is one planned and approved unit of work.

A change set should have:

- one primary goal
- a clearly defined scope
- explicit out-of-scope boundaries
- an expected result
- known assumptions or uncertainties
- a clear stopping point

## Approved scope

The approved scope defines what may be changed.

Anything outside the approved scope is out of scope.

If out-of-scope work appears necessary, the agent must stop and ask for clarification or approval before continuing.

## Change size

A change set should be small enough to understand, implement, and review as one unit.

A change should usually be split when it combines independently reviewable work, such as:

- feature work and refactoring
- data model changes and frontend behavior changes
- backend interface changes and frontend implementation
- implementation and broad documentation cleanup
- multiple user-visible outcomes

## Cross-stack changes

Cross-stack work must first be scoped by the `Architect`.

The Architect-approved scope should identify:

- affected layers
- required decisions
- suggested implementation order
- responsible role for each step
- approval needed before each step

Implementation roles may only execute their approved part of the cross-stack plan.

## Deviation handling

A deviation is any required change that was not part of the approved scope.

If a deviation is discovered, the agent must:

1. stop implementation
2. describe the deviation
3. explain why it appears necessary
4. ask for approval before continuing

## Release process

Each product release is prepared on a release branch named
`release-x.y.z`, where `x.y.z` is the product version.

The root `VERSION` file is the source of truth for the product version. On a
release branch, the branch name and `VERSION` must match exactly. For example,
`release-0.5.0` must contain `0.5.0` in `VERSION`.

Release preparation includes:

- creating or updating the matching release branch
- intentionally updating `VERSION`
- adding the release entry to `CHANGELOG.md`
- verifying that the release branch contains only approved release scope

Release notes must describe the final result of the release, not the internal
path taken to get there. Do not list intermediate branches, repeated fixes,
handoff steps, implementation sequencing, or discussion history unless they are
the actual released outcome.

CHANGELOG entries should be short and scannable:

- prefer flat bullet points
- use brief result-oriented wording
- include user-visible, operational, or product-contract changes
- label architecture decisions only by topic; keep detailed rationale in the
  architecture documents
- avoid long implementation detail, schema walkthroughs, or backend internals

## Database migration policy

### Pre-release (all `0.x.x` releases)

Until `release-1.0.0` is created and declared as the first customer-facing
release, the database has no migration history worth preserving.

During this period:

- Schema changes go into the **originating migration file** — the file where
  the table or type was first defined. Add columns to the `CREATE TABLE`
  statement; do not create a new migration file for the addition.
- No additive migration files are created for pre-release schema changes.
- A full database reset is the expected deployment step after any migration
  file is modified.
- The migration files at any point in time describe the complete intended
  schema, not a sequence of patches.

### Post-release (from `release-1.0.0` onwards)

Once `release-1.0.0` exists:

- Every schema change requires a new numbered migration file.
- Existing migration files must never be modified.
- The migration sequence is the permanent historical record of schema evolution.

---

## Completion

A change set is complete when:

- the approved scope has been addressed
- changed files or areas are reported
- assumptions or deviations are reported
- available checks or manual verification steps are reported
- version-control actions match the approved scope; no commit or push is
  performed unless explicitly approved
- open risks or follow-ups are listed
- the agent stops instead of continuing into the next change

Agent completion is not the same as human acceptance.

The human decides the next step.
