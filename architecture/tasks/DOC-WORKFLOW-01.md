# DOC-WORKFLOW-01 - Local development and Linux runtime workflow

Owner: Architect
Execution: Completed

## Authorization
Stefan, 02 Oct 2026, current chat: secure the complete current state, adopt the
just-discussed workflow in SupaFactory and the application, commit and push both.
Permitted: workflow documentation, local checkout preparation and Git checkpoint/
publish/synchronization. No application edits, package/tool installation, runtime
configuration, deployment, product-default confirmation or new agent assignment.
Existing branches are preserved; no new branching/merge policy is adopted.

## Result
Local editing/Git and available fast checks are separated from Linux final builds,
Docker deployment and runtime verification. Tool versions/lockfiles and exact
commit handoffs are required; dirty/divergent checkouts must be preserved.
Document links and whitespace are checked before commit. Current branch and
commit are evidenced by the containing Git commit, not copied approval logs.
Windows checkouts are prepared using verified Git bundles in this transition;
normal Windows GitHub authentication and matching local build tools are not
verified by this documentation assignment. No application tests are repeated.
No running service, printer, credentials or host configuration is changed.
