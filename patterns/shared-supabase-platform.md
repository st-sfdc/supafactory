# SF-P-001 — Shared Supabase Platform, Database per App

**Status:** Active
**Applies to:** projects running self-hosted Supabase for more than one application

---

## Problem

A SupaFactory application normally runs its own full stack: application
containers plus a self-hosted Supabase platform. The application containers are
cheap. The platform is not — a full self-hosted Supabase deployment is a dozen
or more containers, roughly 2 GiB of resident memory and 11 GiB of container
images.

Running that platform once per application multiplies a cost that buys nothing:
the second, third, and fourth copy are identical. But naively sharing one
Supabase instance across applications quietly couples them — one user table, one
storage namespace, one reset that reaches everything — and makes separating them
later a data-migration project instead of a configuration change.

This pattern shares what is genuinely shareable and keeps hard boundaries where
they matter.

## The pattern

**One PostgreSQL cluster per environment. One database per application. One
auth server per application. One shared platform, owned by nobody's app repo.**

```text
                    ┌─────────────────────────────────────────┐
                    │  platform repo (owns the pinned stack)   │
                    │  gateway · rest · storage · studio · …   │
                    └──────────────────┬──────────────────────┘
                                       │  shared docker network
        ┌──────────────────────────┬────┴─────────┬──────────────────────┐
        │                          │              │                      │
   ┌────┴─────┐              ┌─────┴────┐    ┌────┴─────┐          ┌─────┴────┐
   │  app_a   │              │  app_b   │    │  app_c   │          │ postgres │
   │ backend  │              │ backend  │    │ backend  │          │ cluster  │
   │ frontend │              │ frontend │    │ frontend │          │          │
   │ gotrue   │              │ gotrue   │    │ gotrue   │          │ db app_a │
   └────┬─────┘              └─────┬────┘    └────┬─────┘          │ db app_b │
        └──────── own database ────┴──────────────┘                │ db app_c │
                                                                    └──────────┘
```

Each application owns a database. Each application's auth server points at its
own database and therefore gets its own `auth` schema. No application can reach
another's data, because PostgreSQL cannot join across databases.

## Shared and not shared

| Component | Shared? | Why |
|---|---|---|
| PostgreSQL cluster (the process) | **Yes** | One engine, one page cache, one port. The expensive part. |
| Database | **No** | The isolation boundary. Cross-database foreign keys are impossible, so "apps never share data" is structural rather than a convention. |
| Auth server | **No** | An auth server serves exactly one database. Sharing it means one shared user table — the single most expensive thing to unpick later. Its container costs ~15 MiB; the isolation is worth far more. |
| API gateway, REST, Storage, Studio, Realtime | **Yes** | Stateless or namespaced; one instance serves all tenants. |
| Storage buckets | Namespaced | One storage service, one bucket prefix per app. |
| Container images | **Yes** | Identical pins are stored once. See the pin risk below. |

## Ownership

The platform must **not** live inside an application repository.

A dedicated platform repository owns the pinned Supabase checkout, the shared
Docker network, the compose invocation, database and role provisioning, and the
per-environment inventory of which app holds which database and which ports.
Every application — including the first and oldest one — is a tenant.

If the platform lives in one app's repo, every other app depends on that repo,
and that app's destructive workflows become everyone's risk.

## Configuration contract

Applications declare their relationship to the platform through configuration,
never through code. Switching an app from tenant to standalone, or moving it to
its own cluster, must be a configuration change.

| Setting | Meaning |
|---|---|
| `PLATFORM_MODE` | `OWNER` — this repo bootstraps and starts the platform. `TENANT` — attach to a platform someone else runs. Default must reproduce existing single-app behaviour. |
| `PLATFORM_NETWORK` | Name of the external Docker network the platform publishes. |
| `DATABASE_HOST` / `DATABASE_PORT` | Cluster address. |
| `DATABASE_NAME` | **This app's own database.** Never another app's. |
| `DATABASE_USER` | This app's own role. |
| `DATABASE_SCHEMA` | Explicit search path, `public` excluded. |
| `AUTH_INTERNAL_URL` | This app's own auth server. |
| `GATEWAY_INTERNAL_URL` | The shared gateway, addressed by service role name, never by product or container name. |
| `STORAGE_BUCKET_PREFIX` | This app's bucket namespace. |
| `PORT_BASE` | Base host port; all published ports derive from it. |
| `COMPOSE_PROJECT_NAME` | Container name prefix, so tenants cannot collide. |

Two rules make this hold:

- No application code may contain a database name, schema name, host, or port.
  Everything resolves from configuration at startup.
- The gateway is addressed by its **functional service name**. Naming the
  gateway product couples every consumer to an engine the platform may replace.

## Naming

| Object | Convention |
|---|---|
| Database | the app slug — `acme` |
| Role | `<slug>_app` |
| Schema | `<slug>`, with a search path that excludes `public` |
| Bucket prefix | `<slug>/` |
| Container prefix | the app slug, via `COMPOSE_PROJECT_NAME` |
| Host ports | `PORT_BASE` + fixed offsets, one base per app, recorded in the platform inventory |

Keep `public` empty. A search path that falls back to `public` lets a legacy
object silently satisfy a lookup that should have failed.

## Role isolation

Provisioning an application must:

1. Create the database.
2. Create a role owning only that database.
3. `REVOKE CONNECT` for that role on every other application database.
4. Grant the application role no membership in any superuser or platform role.

In a development environment this is hygiene. Where a shared cluster carries
more than one product's real data, it is the boundary that has to hold, and it
must be verified rather than assumed.

## Destructive operations

A shared cluster changes what a guard must check.

**Wrong:** refuse destructive operations when the deployment mode is production.
That is simultaneously too coarse — a not-yet-launched app must be able to reset
its own database while a live app sits beside it — and too weak, because it says
nothing about which data is actually at risk.

**Right:** a destructive operation must refuse unless

- the target is the **calling application's own database**, and
- the protected business tables and storage objects in that target are **empty**,
  or the human has confirmed their loss for that specific target, and
- no object outside the target depends on an object inside it.

With one database per app, the reset itself becomes `DROP DATABASE` followed by
recreation: complete for its target, incapable of reaching a neighbour, and it
needs no list of object names to stay correct.

Two traps worth naming explicitly:

- Platform-level teardown (`compose down -v`, volume pruning, key rotation,
  regenerating the platform) is **not** an application-level reset. It destroys
  every tenant. It belongs to the platform owner and needs its own gate.
- A data directory bind-mounted from inside a Git checkout is destroyed by
  ordinary repository hygiene. Data directories belong on a neutral path outside
  every application repository.

## Backup granularity

Back up **per database**, not per cluster, and **per bucket prefix**, not per
storage volume.

A cluster-wide dump taken as one app's backup artifact contains every other
app's personal data. That turns a routine operational file into a
cross-product data export, with the retention, access, and privacy obligations
that implies.

## The exit

The pattern is only safe if leaving it is cheap. Extracting an application to
its own cluster must be:

1. Quiesce writes for that app.
2. `pg_dump` its database — application schema and its own `auth` schema
   together.
3. Archive its bucket prefix.
4. Restore both into a new standalone platform.
5. Repoint that app's configuration: database host, database name, auth URL,
   gateway URL, network.
6. Revoke its role on the shared cluster.

No code change, no user-record filtering, no identity remapping. If any step
requires touching application code, the pattern has been violated somewhere and
that is the defect to fix.

Rehearse this once before relying on it, the same way a restore is rehearsed.

## Named risks

**Shared maintenance window.** A platform update touches every tenant at once.
With several live products this is the real cost of sharing, and it is
operational rather than technical. Accept it deliberately with joint windows, and
keep sharding — two platform instances serving groups of apps — as the
documented exit if the coordination cost outgrows the saving.

**Shared failure domain.** One cluster, one gateway. Acceptable in development;
in production it is a decision that must be made consciously, not inherited.

**Divergent pins.** Identical container images are stored once, so tenants cost
nothing extra in disk. Different platform pins per app cost the full image set
again — on the order of 11 GiB. A single centrally pinned platform version is
therefore not a preference but a requirement of the pattern.

**Auth drift.** If anyone shares one auth server between two apps "just for now",
the exit above stops working and the extraction becomes a data-migration
project. This is the one boundary that should never be crossed as a shortcut.

## Evidence

Measured on a development VM with 4 vCPU and 5.8 GiB of memory, running a full
self-hosted stack:

| | Resident memory |
|---|---|
| Full Supabase platform, 15 containers | **2.02 GiB** |
| — of which analytics/log ingestion | 625 MiB, ~12.7 % CPU at idle |
| — of which the admin UI | 232 MiB, ~15.7 % CPU at idle |
| — of which the auth server | **15 MiB** |
| Application stack, 5 containers | 0.19 GiB |
| Container images, total | ~11.8 GiB |

Three conclusions follow, and they are the quantitative basis of this pattern:

- The platform dominates. Duplicating it per app is the whole cost being avoided.
- **An auth server per app costs 15 MiB.** Isolation is essentially free; there
  is no resource argument for sharing it.
- Analytics and the admin UI together account for more memory and far more idle
  CPU than the entire application stack. On a development host, trimming them is
  a larger win than any consolidation.

The same cluster already ran two databases and three application schemas side by
side before this pattern was written, which is evidence that the mechanics work —
and that without a pattern they end up undocumented and unguarded.

## When not to use this

- A tenant requires hard infrastructure isolation for regulatory reasons.
- A tenant genuinely needs a different platform version, permanently.
- Two applications are meant to share one user base. Then design an identity
  service they both federate against, deliberately. Do not arrive at a shared
  user table by sharing infrastructure.

## Open items

- Whether the auth server can be pointed at a non-default schema within one
  database is untested and deliberately unused: the database boundary makes it
  unnecessary.
- Provisioning, guard, and extraction steps above are specified but not yet
  reference-implemented in a platform repository.
