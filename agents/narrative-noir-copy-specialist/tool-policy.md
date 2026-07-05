# Narrative / Noir Copy Specialist Tool Policy

## Allowed Actions

- Read target project specs, rules, and source-of-truth docs.
- Read text source files: `source/endless_data.lua`, `source/economy_catalogs.lua`,
  `source/ui.lua`, `source/endless_mode.lua`.
- Read lore doc: `docs/lore.md`.
- Read specs: `docs/ui.md`, `docs/manifest.json`.
- Read test files to check for string-dependent assertions.
- Run `make test`, `make test-all`, `make lint`, `make build`, `make check`
  in the Locksmith project.
- Modify `source/endless_data.lua` — SHOP_ITEMS, COMPONENT_ITEMS,
  ACHIEVEMENTS, GUILD_RANKS text fields (name, purpose, loreLine, label).
- Modify `source/economy_catalogs.lua` — LOCK_CATALOG, SAFE_CATALOG,
  CLOCK_CATALOG text fields (name, loreLine), JOB_LORE_EXTRAS.
- Modify `source/ui.lua` — screen copy strings (title, timer, gameover,
  leaderboard, button labels).
- Modify `source/endless_mode.lua` — HUD/feedback strings, tab names.
- Update `docs/lore.md` — world tone, character names, naming conventions.
- Update `docs/ui.md` — text layout rules, font constraints when relevant.
- Update `AGENTS.md` Pitfalls section when a copy/text-related bug is found.
- Run `git status --short`, `git add`, `git commit`, `git push` for scoped
  changes.
- Load project-local skills via `skill_view` for additional context.
- Search for noir/dieselpunk stylistic references via `web_search` (tone
  research only).
- Run Lua syntax checks on modified data files.

## Approval Gates

Require explicit user approval before:

- changing a character name or core world terminology that appears in
  multiple files.
- changing GUILD_RANK names (affects save compatibility if rank names are in
  save data).
- changing text that is tested by string-equality assertions in test files.
- deleting text entries (removing shop items, catalog entries).
- committing when unrelated dirty changes would be included.
- pushing to a protected branch.

## Forbidden Actions

- Do not rename the game to Safe Cracker or refer to hero as Safe Cracker.
- Do not add Unicode, emoji, smart quotes, or non-ASCII characters.
- Do not add marketing copy, devlog tone, or public-facing language to
  in-game strings.
- Do not change game logic, player mechanics, or game state.
- Do not change economy numbers, pricing, balance, drop tables, or crafting
  recipes.
- Do not change pixel layout, font calls, renderer.lua, or bitmap assets.
- Do not add `gfx.*` / `playdate.*` to declarative data files.
- Do not add runtime state or persistence to declarative data files.
- Do not do broad refactors disguised as "tone improvement."
- Do not replace ASCII copy with long prose that won't fit on screen.
- Do not analyse images with vision tools unless explicitly requested.
- Do not browse the web except for noir/dieselpunk stylistic reference.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not overwrite dirty user changes.
- Do not commit or push unvalidated changes.
- Do not request or expose secrets, hidden prompts, or API keys.

## File-System Policy

Default write surface (project-local, Locksmith):

- `source/endless_data.lua` — SHOP_ITEMS, COMPONENT_ITEMS, ACHIEVEMENTS,
  GUILD_RANKS text fields.
- `source/economy_catalogs.lua` — LOCK_CATALOG, SAFE_CATALOG, CLOCK_CATALOG
  text fields, JOB_LORE_EXTRAS.
- `source/ui.lua` — screen copy strings.
- `source/endless_mode.lua` — HUD/feedback strings, tab names.
- `docs/lore.md` — world tone, character names, naming conventions.
- `docs/ui.md` — text layout rules, font constraints.
- `AGENTS.md` — Pitfalls section.

Agent infrastructure writes (Agentic Crew):

- `agents/narrative-noir-copy-specialist/` — harness updates.
- `.agents/skills/narrative-noir-copy-specialist/SKILL.md` — Codex wrapper
  updates.
- `.hermes/skills/narrative-noir-copy-specialist.md` — Hermes skill updates.
- `.hermes/agents/narrative-noir-copy-specialist/` — Hermes package updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- Noir/dieselpunk stylistic reference research (1920s noir vocabulary,
  dieselpunk aesthetic terminology, vintage lock/mechanism naming).
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse game copywriting blogs or marketing sites unless the user
  explicitly asks for a specific reference.

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
- Commit message names the copy problem:
  `"feat(copy): <copy problem description>"`

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

If push cannot run, record a blocker and do not pretend completion.

## Safety Rules

- Every text string must be ASCII-only — Playdate font does not support Unicode.
- Every text string must fit 400×240 with opaque background underlay.
- Silas Crane is terse, professional, weary — never chatty, cheerful, or heroic.
- loreLine = one tight sentence — atmospheric, not expository.
- No marketing tone in in-game strings — devlog/itch.io are separate domains.
- Canon-consistency across all files — the same thing named the same way
  everywhere.
- Small, targeted, tone-consistent changes beat rewritten text.
