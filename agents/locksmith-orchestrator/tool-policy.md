# Locksmith Orchestrator Tool Policy

## Allowed Actions

- Read project files: AGENTS.md, docs/*.md, source/*.lua, Makefile, TODO.md.
- Read agentic-crew harnesses, packs, protocol docs.
- Run `make test-all`, `make lint`, `make build` in the locksmith project.
- Route tasks to specialist agents (via delegation).
- Resolve conflicts between specialist outputs using manifest contracts.
- Create plan entries in `plan/current.md` when coordinating.
- Document new pitfalls in AGENTS.md.
- Block work that violates architectural boundaries or guardrails.
- Report task status, test results, and failure analysis.

## Approval Gates

Require explicit user approval before:
- Modifying game code directly (the orchestrator is a coordinator, not an
  implementer).
- Creating or deleting files outside the plan/ or docs/ directories.
- Modifying AGENTS.md for any reason other than adding pitfall documentation.
- Deleting or renaming project assets.
- Changing the routing table in agentic-crew packs.
- Delegating to specialists that do not exist yet.
- Running destructive git operations (reset, hard checkout).

## Forbidden Actions

- Do not modify source/lua code unless no specialist exists for the task and
  the user explicitly permits direct implementation.
- Do not modify AGENTS.md except to add new pitfall entries.
- Do not rename the game to Safe Cracker inside project artifacts.
- Do not rename Silas Crane.
- Do not analyze images unless the user explicitly requests it.
- Do not generate procedural art as a substitute for the asset pipeline.
- Do not hardcode the Playdate SDK path.
- Do not replace PNG assets manually — route to Visual Art specialist.
- Do not run `git push` without user approval.
- Do not modify other projects outside locksmith and agentic-crew.

## File-System Policy

Default read surface:
- `locksmith/AGENTS.md`
- `locksmith/docs/`
- `locksmith/source/` (read only)
- `locksmith/Makefile`
- `locksmith/TODO.md`
- `agentic-crew/packs/`
- `agentic-crew/agents/*/role.md` (read only)

Default write surface:
- `locksmith/plan/` (coordination plans)
- `locksmith/AGENTS.md` (pitfall additions only)

Write to any other location requires user approval.

## Network Policy

- No external network calls unless the task requires checking Playdate SDK docs
  or asset references and the user approves.
- Do not download or execute external scripts.
- Do not send game code or project data to external services.

## Validation Policy

After a specialist returns results:
1. If code changed: run `make test-all` and `make build`.
2. If spec/docs changed: check for valid Markdown and JSON (manifest.json).
3. If assets changed: verify the asset exists at the expected path and
   dimensions (textual check only — no image analysis unless user asks).
4. Document any failure immediately in AGENTS.md.
