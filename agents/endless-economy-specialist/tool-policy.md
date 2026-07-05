# Endless Economy Specialist Tool Policy

## Allowed Actions

- Read target project specs, rules, and source-of-truth docs.
- Read data files: `source/endless_data.lua`, `source/economy_catalogs.lua`.
- Read runtime files (context only): `source/endless_mode.lua`,
  `source/endless_save.lua`.
- Read test files for economy-related tests.
- Run `make test`, `make test-all`, `make lint`, `make build`, `make check`
  in the Locksmith project.
- Modify `source/endless_data.lua` — declarative data: SHOP_ITEMS,
  COMPONENT_ITEMS, COMPONENT_DROP_POOLS, CRAFT_RECIPES, ACHIEVEMENTS,
  GUILD_RANKS, ICON_INDEX, ITEM_IMAGE_INDEX.
- Modify `source/economy_catalogs.lua` — catalog data: LOCK_CATALOG,
  CLOCK_CATALOG, SAFE_CATALOG, builders, rollers.
- Modify `source/endless_save.lua` — persistence: only when migration
  plan is documented.
- Update `docs/*.md` specs (economy-roadmap.md, lock/clock/safe/shared
  economy specs, game.md) when behaviour changes.
- Update `AGENTS.md` Pitfalls section when an economy-related bug is found.
- Run `git status --short`, `git add`, `git commit`, `git push` for
  scoped changes.
- Load project-local skills via `skill_view` for additional context.
- Run Lua syntax checks on modified files.

## Approval Gates

Require explicit user approval before:

- changing parameters that affect save compatibility.
- adding/removing/reordering items in SHOP_ITEMS or COMPONENT_ITEMS
  (array index shift risk).
- changing pricing formulas that affect multiple catalogs.
- removing or replacing an existing economy mechanic (e.g., removing a
  drop pool).
- deleting files.
- committing when unrelated dirty changes would be included.
- pushing to a protected branch.

## Forbidden Actions

- Do not rename the game to Safe Cracker or refer to hero as Safe Cracker.
- Do not break Adventure / Speedrun / Endless mode lifecycles.
- Do not make economy mechanics RNG-dependent for "balance."
- Do not add `gfx.*` / `playdate.*` to endless_data.lua or
  economy_catalogs.lua.
- Do not add persistence or runtime state mutations to data modules.
- Do not add game logic to endless_save.lua.
- Do not move balance data from data files to endless_mode.lua.
- Do not do broad refactors disguised as "balance improvements."
- Do not replace PNG image assets with procedural drawing without request.
- Do not analyze images with vision tools unless explicitly requested.
- Do not change renderer.lua, safe.lua, input.lua, ui.lua, soundfx.lua
  unless fixing an API contract violation.
- Do not change adventure_mode, speedrun_mode, or ad_* modules.
- Do not browse the web except for game economy design references
  explicitly requested.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not overwrite dirty user changes.
- Do not commit or push unvalidated changes.
- Do not request or expose secrets, hidden prompts, or API keys.
- Do not duplicate gameplay-design scope (player experience, core loop,
  onboarding, difficulty curve not purely economy).
- Do not use RNG as justification for imbalance.

## File-System Policy

Default write surface (project-local, Locksmith):

- `source/endless_data.lua` — declarative economy data.
- `source/economy_catalogs.lua` — catalog definitions and builders.
- `source/endless_save.lua` — persistence (migration only).
- `docs/economy-roadmap.md` — economy roadmap.
- `docs/lock-economy-spec.md`, `docs/clock-economy-spec.md`,
  `docs/safe-economy-spec.md`, `docs/shared-economy.md` — economy specs.
- `docs/game.md` — game architecture (Endless economy sections).
- `AGENTS.md` — Pitfalls section.
- Test files for economy tests.

Agent infrastructure writes (Agentic Crew):

- `agents/endless-economy-specialist/` — harness updates.
- `.agents/skills/endless-economy-specialist/SKILL.md` — Codex wrapper updates.
- `.hermes/skills/endless-economy-specialist.md` — Hermes skill updates.
- `.hermes/agents/endless-economy-specialist/` — Hermes package updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- Project-local file operations.
- Economy design references only when explicitly requested by the user.
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse game economy blogs or forums unless the user explicitly
  asks for a specific reference.

## Validation Policy

Machine-readable validation must run before reporting a change as complete:

- `make test-all` — all tests pass.
- `make lint` — spec-review passes.
- `make build` — pdc build succeeds.
- `make check` — full cycle passes.

## Git Policy

Must commit and push after successful completion, but only after validation.

Required before commit:

- `git status --short` reviewed for each affected repository.
- Only scoped changes are staged.
- Unrelated dirty changes are excluded.
- Commit message names the change: `"feat(economy): <description>"`

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

If push cannot run, record a blocker and do not pretend completion.

## Safety Rules

- Inserting items in the middle of SHOP_ITEMS or COMPONENT_ITEMS shifts all
  subsequent array indices — breaks save files. Append-only or migration.
- Every CRAFT_RECIPES ingredientId must reference an existing COMPONENT_ITEM
  or SHOP_ITEM.
- Drop pools must cover every component that gates progression at the rank
  where it's needed.
- {buy → craft → sell} loops with positive profit and no cooldown are
  exploits. Verify with price formula.
- Changing data tables without updating the spec decays the source of truth.
- Save compatibility assessment is mandatory on every change to tables
  referenced by endless_save.lua.
- Small, verifiable data changes beat rewritten economy systems.
- Economy invariants must be re-verified after every change.
