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

Cross-stack work must first be planned by the `Planner`.

The plan should identify:

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

## Completion

A change set is complete when:

- the approved scope has been addressed
- changed files or areas are reported
- assumptions or deviations are reported
- available checks or manual verification steps are reported
- open risks or follow-ups are listed
- the agent stops instead of continuing into the next change

Agent completion is not the same as human acceptance.

The human decides the next step.
