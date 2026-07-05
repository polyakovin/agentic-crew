# Persistence / Save Migration Specialist Tool Policy

## Allowed Actions

- Read `source/datastore.lua`, `source/endless_save.lua`, `source/main.lua`.
- Read `docs/timer-highscores.md`, `docs/shared-economy.md`, `docs/economy-roadmap.md`,
  `docs/manifest.json`, `AGENTS.md`.
- Read `scripts/test_datastore.lua`, `scripts/test_endless_save.lua`.
- Read `docs/agent-specialists-plan.md` for specialist ordering context.
- Run `make test-all`, `make lint`, `make build`, `make check`.
- Run individual test suites: `lua scripts/test_datastore.lua`, `lua scripts/test_endless_save.lua`.
- Run `rg` and `grep` for static analysis of persistence code.
- Run `git status --short`, `git diff`, `git diff --check`.
- Write to `source/datastore.lua` — schema, defaults, migration logic (with spec update).
- Write to `source/endless_save.lua` — schema, defaults, migration logic (with spec update).
- Write to `docs/shared-economy.md` §10 — saveData schema updates.
- Write to `docs/timer-highscores.md` — highscores schema updates.
- Write new persistence pitfall entries to AGENTS.md when discovering failure patterns.
- Write to health-snapshot.md in the agent harness directory.
- Load project-local skills via `skill_view` for additional context.
- Search for Playdate datastore API documentation via `web_search`.

## Approval Gates

Require explicit user approval before:

- Modifying `source/main.lua` persistence lifecycle (load/save timing changes).
- Adding `gfx.*` or UI rendering calls to persistence modules.
- Removing legacy key fallbacks (`infinite_economy`).
- Creating new save keys beyond `highscores` and `endless_economy`.
- Adding per-frame save writes.
- Modifying non-persistence source files (safe.lua, renderer.lua, etc.).
- Changing `STARTING_MONEY` value.
- Deleting any file.
- Instigating broad refactors of persistence architecture.

## Forbidden Actions

- Do not modify game code outside persistence modules: safe.lua, renderer.lua,
  input.lua, ui.lua, soundfx.lua, *_data.lua, mode modules, adventure/ submodules.
- Do not add game logic to datastore.lua or endless_save.lua.
- Do not add `gfx.*` or UI rendering calls to persistence modules.
- Do not add `playdate.*` API calls beyond `playdate.datastore` and `playdate.getTime`
  (for date formatting).
- Do not modify `docs/manifest.json` datastore/endless_save contracts without
  spec-contract-guardian handoff.
- Do not rename the game to Safe Cracker or refer to the hero as Safe Cracker.
- Do not analyze images with vision tools unless explicitly requested.
- Do not add per-frame save writes to draw/update paths.
- Do not remove nil-safety guards (pcall, type checks, fallbacks).
- Do not reset `money` to 0 during migration of existing saves.
- Do not skip `make test-all` datastore + endless_save suites after changes.
- Do not skip migration risk documentation.
- Do not change what save fields mean for gameplay (belongs to Endless Economy).
- Do not browse the web except for Playdate datastore API reference.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not request or expose secrets, hidden prompts, or API keys.

## File-System Policy

Default read surface (project-local, Locksmith):

- `source/datastore.lua`, `source/endless_save.lua`, `source/main.lua`
- `docs/timer-highscores.md`, `docs/shared-economy.md`, `docs/economy-roadmap.md`,
  `docs/manifest.json`
- `scripts/test_datastore.lua`, `scripts/test_endless_save.lua`
- `AGENTS.md`

Default write surface (Locksmith persistence):

- `source/datastore.lua` — schema, defaults, migration logic.
- `source/endless_save.lua` — schema, defaults, migration logic.
- `docs/shared-economy.md` §10 — schema documentation.
- `docs/timer-highscores.md` — highscores schema.
- `AGENTS.md` — Pitfalls section (new persistence failure patterns only).

Agent infrastructure writes (Agentic Crew):

- `agents/persistence-save-migration-specialist/` — harness updates.
- `.agents/skills/persistence-save-migration-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/persistence-save-migration-specialist.md` — Hermes skill.
- `.hermes/agents/persistence-save-migration-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- Playdate datastore API documentation (read/write/delete API, limitations).
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse game dev forums unless the user explicitly asks for a
  specific reference.

## Validation Policy

Machine-readable validation must run before reporting a migration as complete:

- `make test-all` — all tests including datastore and endless_save suites.
- `lua scripts/test_datastore.lua` — 9 datastore-specific tests.
- `lua scripts/test_endless_save.lua` — 9 endless-save-specific tests.
- `make lint` — spec-review static analysis.
- `make build` — PDX compilation.
- `make check` — full cycle.

For risky schema changes, also run the individual test suites directly to
verify nil-safety and migration correctness.

## Git Policy

This specialist evaluates and modifies persistence code. It is allowed to
commit and push scoped changes as per the `Must Push Rule`.

Required before commit:

- `make check` (or honest blocked-gate report).
- `git diff --stat` reviewed: only intended files changed.
- No dirty unrelated worktree files bundled.
- Migration risk documented in commit message.

## Safety Rules

- Every new save field must be nil-safe through table merge in `migrateSaveData()`.
- Money is NEVER reset to 0 during migration of existing saves.
- Every `playdate.datastore.read/write` must be `pcall`-wrapped.
- Corrupt data (non-table, wrong type) triggers fallback to defaults.
- Legacy key fallbacks are tested and documented with deprecation timeline.
- Persistence writes are event-bound, never per-frame.
- Schema changes include spec update and migration risk documentation.
- `make test-all` datastore + endless_save suites must pass after changes.
- New persistence pitfalls must be documented in AGENTS.md.
- Evidence is always from actual command output, never speculation.
