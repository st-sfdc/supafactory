# Environments

Runbook-style reference for the environments this project runs in.

This file is the single source of truth for hosts, access paths, deploy
workflows, and verification targets. `AI-BOOTSTRAP.md`, `governance/*.md`, and
`prompts/*.md` must not restate concrete hosts, addresses, credentials, or
paths — they point here instead.

This file is owned by the Architect. Agents must read it before attempting any
runtime verification, deployment, or remote debugging.

## Local workstation

<!-- State what the local checkout is used for, and what it is NOT used for. -->
<!-- If the application stack is not run locally, say so explicitly, and say -->
<!-- which tooling agents must not assume is installed. -->

## Development environment

<!-- Name, address or host alias, access method, repo path on the host. -->
<!-- Deploy workflow and the commands that belong to it. -->
<!-- Which role may operate it, and under what approval. -->
<!-- Runtime verification targets: URLs, ports, API endpoints. -->
<!-- Known fragility, such as DHCP-assigned addresses. -->

## Production

<!-- Whether it exists yet. If it does: hostname, release branch, deployment -->
<!-- mode, and the operational rules that protect it. -->
<!-- List destructive workflows that are forbidden against this environment. -->
<!-- Reference the backup/restore runbook if one exists. -->

## Verification paths

<!-- Which checks belong to which environment, and which tool performs them. -->
<!-- Distinguish infrastructure-level checks from user-flow checks. -->

---

<!-- Last updated: <date> — <what changed>. -->
