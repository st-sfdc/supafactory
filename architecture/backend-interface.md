# Backend Interface

The authoritative contract between clients and the backend.

This file is owned by the Architect role. Frontend Implementers must use the
contracts documented here and must not invent endpoints, fields, or response
shapes. Backend Implementers must not change a documented contract without an
approved scope that says so.

## Conventions

<!-- Base path, content types, casing, date/time format, pagination style, -->
<!-- and the standard error shape. -->

## Authentication and session

<!-- How a client authenticates, what it receives, how the session is carried, -->
<!-- and which endpoints are public versus protected. -->

## Endpoints

<!-- One section per endpoint group. For each endpoint give method, path, -->
<!-- auth requirement, request shape, response shape, and error cases. -->

### <group>

<!--
`GET /api/<path>`

Auth: <required role, or public>

Response:

```json
{}
```

Errors: <status and meaning>
-->

## Contracts that must not change silently

<!-- Shapes that external or native clients depend on. Changing these is an -->
<!-- architecture decision, not an implementation detail. -->
