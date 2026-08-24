# Architecture

The authoritative description of how this system is built.

This file is owned by the Architect role and must be kept current before
implementation begins. Agents read it before planning or implementing any change.

## Overview

<!-- One paragraph: what this system is and the shape it follows. -->
<!-- Follow it with a diagram of the runtime components and how requests flow. -->

## Entry points

<!-- Each way a user or client reaches the system: web app, admin app, API, -->
<!-- static site, native client. Note which origin or host serves each one. -->

## Key decisions

<!-- One short table per area. Record the decision, not the discussion. -->
<!-- Detailed rationale belongs in decisions.md. -->

### Authentication

<!-- Identity provider, session mechanism, where credentials are stored, -->
<!-- and which layer is the authoritative auth boundary. -->

### Database

<!-- Engine, hosting, schema ownership, migration mechanism. -->
<!-- Schema design itself belongs in data-model.md. -->

### Backend

<!-- Language, framework, API style. -->
<!-- API contracts belong in backend-interface.md. -->

### Frontend

<!-- Framework, build step, routing, asset strategy. -->

## Background and scheduled work

<!-- Any service that runs outside the request lifecycle: workers, schedulers, -->
<!-- queues. State the boundary between them and the request path. -->

## Open decisions

<!-- Architectural questions that are known but not yet decided. -->
<!-- Each entry should name the trigger that forces the decision. -->
