# Environments

Runbook-style reference for the environments this project runs in.

This file is the single source of truth for hosts, access paths, deploy
workflows, and verification targets. `AI-BOOTSTRAP.md`, `governance/*.md`, and
`prompts/*.md` must not restate concrete hosts, addresses, credentials, or
paths — they point here instead.

This file is owned by the Architect. Agents must read it before attempting any
runtime verification, deployment, or remote debugging.

## Local workstation

<!-- Name the primary source/documentation/Git checkout and its branch. -->
<!-- List available tools and required versions separately from assumptions. -->
<!-- Specify fast local checks; a local build is not remote runtime evidence. -->
<!-- Do not assume Docker or the complete application stack is available here. -->

## Development environment

<!-- Name, address or host alias, access method, repo path on the host. -->
<!-- Git synchronization: repository/branch/commit, clean-tree check, fetch -->
<!-- and fast-forward rules; preserve unfinished work and report divergence. -->
<!-- Final build uses matching declared versions and committed lockfiles. -->
<!-- Deployment commands and authorization are distinct from source push. -->
<!-- Which role may operate it, and under what approval. -->
<!-- Runtime verification targets: URLs, ports, API endpoints. -->
<!-- Known fragility, such as DHCP-assigned addresses. -->

## Production

<!-- Whether it exists yet. If it does: hostname, release branch, deployment -->
<!-- mode, and the operational rules that protect it. -->
<!-- List destructive workflows that are forbidden against this environment. -->
<!-- Reference the backup/restore runbook if one exists. -->

## Verification paths

<!-- Allocate local static/type/unit/build checks versus final target build. -->
<!-- Keep dependency directories platform-local; do not copy node_modules. -->
<!-- Which checks belong to which environment, and which tool performs them. -->
<!-- Distinguish infrastructure-level checks from user-flow checks. -->

---

<!-- Last updated: <date> — <what changed>. -->
