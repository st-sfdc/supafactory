# Data Model

The authoritative description of the persisted data structures.

This file is owned by the Architect role and must be kept current before
implementation begins. Implementers must not introduce schema changes that are
not documented here first.

## Schema ownership

<!-- Which schema or namespace owns application objects. -->
<!-- Which schemas are platform-owned and must never be reset by the project. -->

## Entities

<!-- One section per table. For each, give a short purpose statement and a -->
<!-- column table: name, type, nullable, default, and a note where the -->
<!-- meaning is not obvious. -->

### <entity>

<!--
| Column | Type | Nullable | Default | Notes |
|---|---|---|---|---|
| id | uuid | No | gen_random_uuid() | |
-->

## Relationships

<!-- Foreign keys and cardinality. Note any deliberate absence of a foreign -->
<!-- key and why. -->

## Enumerations

<!-- Named types and their permitted values. State whether a value is -->
<!-- user-visible, and what a rename would break. -->

## Constraints and invariants

<!-- Rules the database enforces, and rules it does not enforce but the -->
<!-- application relies on. -->

## Migration notes

<!-- The migration mechanism and the current pre-release/post-release posture. -->
<!-- See governance/change-control.md for the policy itself. -->
