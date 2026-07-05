# Crank Feel / Micro-Mechanics Specialist Tool Policy

## Allowed Actions

- Read target project specs, rules, and source-of-truth docs.
- Read source files: `source/input.lua`, `source/safe.lua`, `source/renderer.lua`,
  `source/soundfx.lua`, `source/*_data.lua`.
- Read test files: `scripts/test_safe.lua`, `scripts/test_stubs.lua`.
- Run `make test`, `make test-all`, `make lint`, `make build`, `make check`
  in the Locksmith project.
- Modify `source/input.lua` — crank capture, D-pad, smoothing, inertia.
- Modify `source/safe.lua` — physics constants, pin mechanics, tolerance, snap.
- Modify `source/*_data.lua` — declarative balance tables for difficulty/tolerance.
- Modify `source/soundfx.lua` — audio feedback timing and triggers.
- Modify `source/renderer.lua` — visual feedback drawing (not game mechanics).
- Update `docs/*.md` specs and `docs/manifest.json` when behaviour changes.
- Update `AGENTS.md` Pitfalls section when a feel-related bug is found.
- Run `git status --short`, `git add`, `git commit`, `git push` for scoped changes.
- Load project-local skills via `skill_view` for additional context.
- Search for PlayDate SDK crank API docs via `web_search` (crank API reference only).
- Run Lua syntax checks on modified files.

## Approval Gates

Require explicit user approval before:

- changing parameters that affect multiple difficulty levels or game modes.
- removing or replacing an existing feel mechanic (e.g., removing inertia).
- adding procedural art to renderer.lua for visual feedback (vs using PNG assets).
- deleting files.
- committing when unrelated dirty changes would be included.
- pushing to a protected branch.

## Forbidden Actions

- Do not rename the game to Safe Cracker or refer to hero as Safe Cracker.
- Do not break Adventure / Speedrun / Endless mode lifecycles.
- Do not make mechanics RNG-dependent for "balance."
- Do not add `gfx.*` / `playdate.*` to safe.lua.
- Do not add game state mutations to renderer.lua.
- Do not add game logic to input.lua.
- Do not reference safe/renderer/ui from input.lua.
- Do not mix declarative data tables and runtime logic.
- Do not do broad refactors disguised as "feel improvements."
- Do not replace PNG image assets with procedural drawing without request.
- Do not analyze images with vision tools unless explicitly requested.
- Do not change renderer.lua drawing logic to alter game mechanics.
- Do not browse the web except for PlayDate SDK crank API reference.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not overwrite dirty user changes.
- Do not commit or push unvalidated changes.
- Do not request or expose secrets, hidden prompts, or API keys.

## File-System Policy

Default write surface (project-local, Locksmith):

- `source/input.lua` — crank capture, D-pad, inertia, smoothing.
- `source/safe.lua` — physics constants, pin mechanics, tolerance, snap.
- `source/soundfx.lua` — audio feedback.
- `source/renderer.lua` — visual feedback drawing.
- `source/*_data.lua` — declarative data tables.
- `docs/input.md`, `docs/safe.md`, `docs/game.md`, `docs/renderer.md`,
  `docs/ui.md`, `docs/test-scenarios.md`, `docs/manifest.json`.
- `AGENTS.md` — Pitfalls section.
- `scripts/test_safe.lua` or other test files for new feel tests.

Agent infrastructure writes (Agentic Crew):

- `agents/crank-feel-specialist/` — harness updates.
- `.agents/skills/crank-feel-specialist/SKILL.md` — Codex wrapper updates.
- `.hermes/skills/crank-feel-specialist.md` — Hermes skill updates.
- `.hermes/agents/crank-feel-specialist/` — Hermes package updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- PlayDate SDK crank API documentation (`playdate.getCrankTicks()`,
  `playdate.getCrankAngle()`, `playdate.getCrankChange()`).
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse game feel blogs or forums unless the user explicitly asks
  for a specific reference.

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
- Commit message names the UX intent: `"feat(crank-feel): <description>"`

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

If push cannot run, record a blocker and do not pretend completion.

## Safety Rules

- Every frame of smoothing is one frame of input lag. At 20 fps, 2 frames = 100 ms.
- Snap radius must not exceed tolerance — snap should assist, not replace precision.
- Crank wrap-around (359° → 0°) must be explicitly tested.
- Inertia helps fluidity but hinders precision at high difficulty — tune per tier.
- Changing safe.lua parameters without updating the spec decays the source of truth.
- Feedback timing (sound ↔ visual) must be synchronous; test on Simulator.
- Small, verifiable changes beat rewritten input/physics stacks.
