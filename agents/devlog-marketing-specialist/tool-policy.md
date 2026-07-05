# Devlog / Community Marketing Specialist Tool Policy

## Allowed Actions

- Read target project files: AGENTS.md, README.md, TODO.md,
  `devlog/template.md`.
- Read game docs: `docs/game.md`, `docs/lore.md`, `docs/manifest.json`.
- Read spec files for specific features being described.
- Run `git log`, `git diff`, `git show` to inspect changes.
- Run `make check`, `make build` in the Locksmith project (informational).
- Run `rg` searches to cross-reference claims against specs.
- Draft devlog posts, release notes, social copy, itch.io page copy.
- Update `AGENTS.md` Pitfalls section when a marketing/copy bug is found.
- Update `devlog/template.md` if the post format convention changes.
- Search the web for PlayDate indie game marketing references,
  itch.io best practices, genre comparisons.
- Load project-local skills via `skill_view` for additional context.

## Approval Gates

Require explicit user approval before:

- Changing the game's name, description, or tags on the actual itch.io
  page (specialist drafts only — does not publish).
- Writing about features that are not yet implemented (must be flagged as
  "предстоящее" / upcoming).
- Publishing devlog posts (draft only — user posts).
- Changing `devlog/template.md` format (user owns the template).
- Changing `README.md` marketing stance or project status.
- Referencing third-party games, developers, or platforms by name.
- Using the `Safe Cracker` name in any player-facing copy (except as
  legacy URL migration note).

## Forbidden Actions

- Do not rename the game to Safe Cracker or refer to Silas Crane as
  Safe Cracker.
- Do not modify game code, Lua files, source/, docs/specs (except
  `devlog/template.md` and `AGENTS.md` Pitfalls).
- Do not write in-game text, lore, item names, or character dialogue.
- Do not generate images, art, screenshots, or visual assets.
- Do not modify `source/endless_data.lua`, `source/economy_catalogs.lua`,
  `source/ui.lua`, `source/endless_mode.lua`.
- Do not change game logic, mechanics, balance, or rendering.
- Do not publish to itch.io, social media, or any platform.
- Do not write about features that are not implemented and verified.
- Do not use emoji in devlog or itch.io copy (social clips only if
  requested).
- Do not write in English unless the task explicitly asks.
- Do not request or expose secrets, API keys, or credentials.
- Do not commit unapproved changes to project files.
- Do not overwrite dirty user changes.

## File-System Policy

Default read surface (project-local, Locksmith):

- `AGENTS.md` — project rules and pitfalls.
- `README.md` — project overview and marketing stance.
- `TODO.md` — current plan and completed work.
- `devlog/template.md` — post format conventions.
- `docs/game.md` — mode descriptions, feature specs.
- `docs/lore.md` — world and character references (read-only).
- `docs/manifest.json` — component contracts.
- `source/` — read-only for claim verification.
- `scripts/` — read-only for test evidence.

Write surface (project-local, Locksmith):

- `AGENTS.md` — Pitfalls section only.
- `devlog/template.md` — format updates only.
- Output artifact: devlog draft text (any output path user specifies).

Agent infrastructure writes (Agentic Crew):

- `agents/devlog-marketing-specialist/` — harness updates.
- `.agents/skills/devlog-marketing-specialist/SKILL.md` — Codex wrapper
  updates.
- `.hermes/skills/devlog-marketing-specialist.md` — Hermes skill updates.
- `.hermes/agents/devlog-marketing-specialist/` — Hermes package updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- PlayDate indie game marketing references, itch.io developer best
  practices, indie game devlog format research.
- Competitive research: other PlayDate games, lockpicking games,
  tactile puzzle games (for positioning only).
- Source URL must be recorded in the report.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse the game's own itch.io page for scraping (user provides
  page content if needed).

## Validation Policy

Machine-readable validation runs for claim verification, not for copy
quality:

- `make check` — informational: verify that features being described
  actually build and pass tests.
- `rg` searches — cross-reference claims against spec docs.

Copy quality validation is manual: peer review, template format check,
tone check.

## Git Policy

Marketing copy does NOT require code commits. Only commit when:

- Updating `AGENTS.md` Pitfalls with a marketing copy lesson.
- Updating `devlog/template.md` format.
- Updating agent infrastructure files (harness, wrappers, routing).

Required before commit:

- `git status --short` reviewed for each affected repository.
- Only scoped changes are staged.
- Unrelated dirty changes are excluded.
- Commit message names the change clearly.

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

## Safety Rules

- Every gameplay claim must be backed by: git log, spec doc, and
  `make check` evidence.
- No features described that are not implemented and tested.
- Game name is Locksmith (hero: Silas Crane).
- Legacy URL (polyakovin.itch.io/safe-cracker) referenced only as
  migration note.
- Tone is developer storyteller, not corporate marketing.
- Russian language in player copy, compressed, gameplay-first.
- No variable names, source files, APIs, or implementation details.
- No publishing — draft only, user reviews and posts.
- Handoff to Narrative Noir Copy Specialist for in-game tone/canon.
- Handoff to Visual Art Specialist for screenshots/assets.
