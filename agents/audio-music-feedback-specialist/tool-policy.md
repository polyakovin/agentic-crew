# Audio / Music Feedback Specialist Tool Policy

## Allowed Actions

- Read target project files: AGENTS.md, `docs/manifest.json`,
  `docs/soundfx.md`, `docs/playdate-best-practices.md`.
- Read audio source files: `source/soundfx.lua`, `source/music_data.lua`.
- Read main.lua for music toggle integration (read-only).
- Inspect audio assets: `source/sounds/` — file formats, sizes, presence.
- Run `make test-all`, `make lint`, `make build`, `make check` in the
  Locksmith project.
- Run `rg` searches to cross-reference audio parameters against specs.
- Run `ls`, `wc`, `file` on audio assets for size/format analysis.
- Run `unzip -l` on Locksmith.pdx for PDX audio footprint analysis.
- Modify `source/soundfx.lua` — audio parameters and lifecycle only,
  respecting the audio-only boundary.
- Modify `source/music_data.lua` — declarative playlist data only.
- Update `docs/soundfx.md` — spec updates for audio changes.
- Update `AGENTS.md` Pitfalls section when an audio-specific error is
  discovered.
- Search the web for PlayDate audio best practices, IMA ADPCM encoding,
  sampleplayer/fileplayer documentation.
- Load project-local skills via `skill_view` for additional context.

## Approval Gates

Require explicit user approval before:

- Adding new audio assets (must have PDX size analysis first).
- Changing the music toggle behaviour in `main.lua` (main.lua is outside
  audio scope — escalate, don't modify directly).
- Removing audio assets from the project.
- Changing the audio file format for a track that's already in use
  (requires re-encoding).
- Renaming audio files (breaks asset references in soundfx.lua).
- Making music louder than SFX for "artistic" reasons.

## Forbidden Actions

- Do not rename the game to Safe Cracker or refer to Silas Crane as
  Safe Cracker.
- Do not add game rules, logic, or state machines to `source/soundfx.lua`.
- Do not add `require("safe")`, `require("renderer")`, `require("ui")`,
  `require("input")`, or mode imports to `source/soundfx.lua`.
- Do not add `require("datastore")` or persistence reads to
  `source/soundfx.lua`.
- Do not add `gfx.*` or `playdate.*` API calls to
  `source/music_data.lua`.
- Do not add persistence, lifecycle, or runtime state mutations to
  `source/music_data.lua`.
- Do not modify `source/main.lua` (music toggle is main.lua's
  responsibility — escalate to owner).
- Do not modify `source/safe.lua`, `source/renderer.lua`, `source/ui.lua`,
  `source/input.lua` for audio reasons.
- Do not generate images, art, or visual assets.
- Do not write in-game copy, lore, item names, or character dialogue.
- Do not do broad refactors of the audio module without a specific
  audio problem.
- Do not make SFX quieter than background music.
- Do not remove nil checks on sampleplayer/fileplayer.
- Do not commit unapproved changes to project files.
- Do not overwrite dirty user changes.
- Do not request or expose secrets, API keys, or credentials.

## File-System Policy

Default read surface (project-local, Locksmith):

- `AGENTS.md` — project rules and pitfalls.
- `docs/manifest.json` — component contracts.
- `docs/soundfx.md` — audio spec.
- `docs/playdate-best-practices.md` — PlayDate audio constraints.
- `source/soundfx.lua` — audio runtime module.
- `source/music_data.lua` — declarative playlist data.
- `source/main.lua` — music toggle integration (read-only).
- `source/sounds/` — audio assets.
- `scripts/` — test evidence.

Write surface (project-local, Locksmith):

- `source/soundfx.lua` — audio parameters and lifecycle only.
- `source/music_data.lua` — declarative playlist data only.
- `docs/soundfx.md` — spec updates.
- `AGENTS.md` — Pitfalls section only.
- Audio assets in `source/sounds/` — only with user approval.

Agent infrastructure writes (Agentic Crew):

- `agents/audio-music-feedback-specialist/` — harness updates.
- `.agents/skills/audio-music-feedback-specialist/SKILL.md` — Codex
  wrapper updates.
- `.hermes/skills/audio-music-feedback-specialist.md` — Hermes skill
  updates.
- `.hermes/agents/audio-music-feedback-specialist/` — Hermes package
  updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- PlayDate SDK audio documentation, sampleplayer/fileplayer API reference.
- IMA ADPCM encoding best practices for embedded audio.
- Competitive research: other PlayDate games with notable audio design
  (for benchmarking only).
- Source URL must be recorded in the report.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not download audio assets from the web without explicit user approval.

## Validation Policy

Machine-readable validation runs for audio evidence:

- `make check` — full build pipeline: verify audio module builds and
  tests pass.
- `rg` searches — cross-reference audio parameters against spec docs.
- `ls -la source/sounds/` — asset presence check.
- `wc -c source/sounds/*.wav` — asset size baseline.
- `unzip -l Locksmith.pdx | grep -E '\.wav$'` — PDX audio footprint.

Audio quality validation is manual: Simulator listening tests, volume
mixing by ear, playlist lifecycle observation.

## Git Policy

Audio changes require code commits. Commit when:

- `source/soundfx.lua` audio behaviour changes.
- `source/music_data.lua` playlist data changes.
- `docs/soundfx.md` spec updates.
- Audio assets added/changed/recompressed.
- `AGENTS.md` Pitfalls updated with audio-specific error.

Required before commit:

- `git status --short` reviewed for each affected repository.
- Only scoped changes are staged.
- Unrelated dirty changes are excluded.
- Commit message names the change clearly.

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

## Safety Rules

- Every audio change is parametric: old→new with rationale.
- soundfx.lua stays audio-only: no game rules, no UI, no persistence.
- music_data.lua stays declarative: no gfx.*, no playdate.*, no state.
- Missing audio never crashes the game.
- SFX always louder than music.
- Music compressed as ADPCM, SFX as PCM.
- PDX size impact documented for any asset change.
- Spec updated before code when API/contract changes.
- Game name is Locksmith (hero: Silas Crane).
- AGENTS.md Pitfalls updated for audio errors.
- Handoff to other specialists when outside scope.
