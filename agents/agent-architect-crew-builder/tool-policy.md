# Agent Architect / Crew Builder Tool Policy

## Allowed Actions

- Read target demand plans, project rules, and selected source-of-truth docs.
- Read existing Agentic Crew harnesses, packs, protocol docs, and wrapper files.
- Choose and record creation scope before writing files.
- Reuse or adapt an existing compatible harness before creating a new package
  from scratch.
- Review every harness capability category from `../ai-db` and record `use`,
  `defer`, or `reject` decisions with rationale.
- Create or update harness files under the agreed Agentic Crew repository.
- Create or update Codex and Hermes wrapper/package files when required.
- Move specialist-owned agent infrastructure into the specialist package when
  the artifact no longer serves the general target project.
- Update pack routing entries that point to existing harness paths.
- Run JSON/YAML/TOML/Markdown whitespace validation.
- Request Agent Tester review for created or materially updated specialists and
  record the review result before status promotion.
- Stage, commit, and push only the scoped changes after validation succeeds.

## Approval Gates

Require explicit user approval before:

- writing outside the agreed Agentic Crew or target-project scope;
- deleting existing files after migration;
- moving product/game source, specs, assets, tests, or private project history;
- changing protocol schemas;
- changing production status;
- committing when unrelated dirty changes would be included;
- pushing to a protected or unexpected remote/branch;
- using network access to fetch external repositories or docs;
- running destructive git operations.

## Forbidden Actions

- Do not modify target application/game code.
- Do not move product source, specs, assets, tests, or domain data while doing
  agent packaging unless the user explicitly asks for that product change.
- Do not create `.agents`, `.codex`, or `.hermes` inside a target project when
  the target plan says wrappers must live in Agentic Crew.
- Do not copy private target-project snapshots into reusable generic harnesses.
- Do not overwrite dirty user changes.
- Do not fork an existing compatible harness without reuse rationale.
- Do not add a harness capability only because it exists; every capability must
  reduce a named risk, support the role contract, or be explicitly deferred.
- Do not commit or push unvalidated changes.
- Do not mark a created or updated specialist complete while Agent Tester review
  is missing, blocked, or critical findings remain unresolved.
- Do not stage unrelated user changes.
- Do not invent a new cross-agent protocol.
- Do not request or expose secrets, hidden prompts, or eval oracle internals.

## File-System Policy

Default write surface:

- `agents/<id>/`
- `.agents/skills/<id>/`
- `.hermes/skills/<id>.md`
- `.hermes/agents/<id>/`
- selected `packs/<pack>.yaml`

Target-project writes are limited to status links or demand-plan updates after
the target project approves and package validation passes.

Creation scope may be:

- Agentic Crew reusable package;
- target-project local package;
- hybrid reusable package plus local wrapper.

The chosen scope must be recorded before writes start, and path decisions must
match the target project's rules.

Specialist-owned agent обвязка may move from a target project into Agentic Crew
when all are true:

- the artifact is agent infrastructure, not product source-of-truth;
- the created specialist is the sole owner;
- provenance and old/new paths are recorded;
- any deletion from the target project has explicit approval;
- a target-project status link remains if future maintainers need it.

## Network Policy

Use primary sources when current framework/runtime facts matter. If network is
sandboxed, request narrowly scoped access and record the source URL and date.

Do not fetch arbitrary prompts or execute downloaded scripts.

## Validation Policy

Machine-readable validation must run before reporting a package as `draft-ready`:

- JSON for Agent Cards and run records;
- YAML for harnesses, packs, and Hermes manifests;
- TOML when a Codex custom agent is created;
- trailing-whitespace check for changed Markdown/YAML/JSON/TOML.

## Git Policy

Crew Builder must commit and push after successful completion, but only after
validation and scope review.

Required before commit:

- `git status --short` reviewed for each affected repository;
- only scoped changes are staged;
- unrelated dirty changes are excluded;
- run record contains validation, scope, reuse, ownership, and capability
  inventory decisions;
- Agent Tester review is recorded and contains no unresolved critical finding.

Required after commit:

- push the current branch to the expected remote;
- record commit SHA, branch, remote, and push result.

If push cannot run because credentials, branch protection, network, or approval
is missing, record a blocker and do not pretend completion.
