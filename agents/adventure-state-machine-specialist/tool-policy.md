# Adventure State Machine Specialist Tool Policy

## Allowed Actions

- Read target project specs, rules, and source-of-truth docs.
- Read source files: `source/states.lua`, `source/adventure_mode.lua`,
  `source/adventure/ad_curtain.lua`, `source/adventure/ad_safedial.lua`,
  `source/adventure/ad_clock.lua`, `source/adventure/ad_renderer.lua`,
  `source/adventure/ad_story_data.lua`.
- Read `source/main.lua` — dispatcher (title ↔ mode transitions, cleanup).
- Read `source/datastore.lua` — persistence (markAdventureComplete).
- Read test files: `scripts/test_safe.lua`, `scripts/test_stubs.lua`.
- Read `docs/test-scenarios.md` — cross-reference map for test scenarios.
- Run `make test`, `make test-all`, `make lint`, `make build`, `make check`
  in the Locksmith project.
- Modify `source/states.lua` — ADVENTURE_STATE enum.
- Modify `source/adventure_mode.lua` — state machine, transitions, phase
  coordination.
- Modify `source/adventure/ad_curtain.lua` — phase 1 logic.
- Modify `source/adventure/ad_safedial.lua` — phase 3 logic.
- Modify `source/adventure/ad_clock.lua` — phase 4 logic.
- Modify `source/adventure/ad_renderer.lua` — adventure-specific drawing
  (only to reflect state machine changes in rendering, not game mechanics).
- Modify `source/adventure/ad_story_data.lua` — declarative story data.
- Update `docs/game.md` — Adventure architecture and flow spec.
- Update `docs/manifest.json` — component contracts (Adventure section).
- Update `docs/test-scenarios.md` — Adventure flow test scenarios.
- Update `AGENTS.md` Pitfalls section when a state-machine-related bug is found.
- Run `git status --short`, `git add`, `git commit`, `git push` for scoped changes.
- Load project-local skills via `skill_view` for additional context.
- Search for PlayDate SDK system menu / lifecycle API docs via `web_search`.
- Run Lua syntax checks on modified files.

## Approval Gates

Require explicit user approval before:

- adding a new ADVENTURE_STATE enum value.
- adding a new Adventure gameplay phase.
- removing an existing state or phase.
- changing the story flow order (reordering phase sequence).
- changing persistence boundaries (when/where markAdventureComplete is called).
- changing B button or System Menu exit behaviour.
- deleting files.
- committing when unrelated dirty changes would be included.
- pushing to a protected branch.

## Forbidden Actions

- Do not rename Locksmith to Safe Cracker or refer to hero as Safe Cracker.
- Do not break Adventure / Speedrun / Endless mode lifecycles.
- Do NOT mix Adventure state machine logic with Speedrun or Endless logic.
- Do NOT use Menu button as the only way to exit Adventure mode.
- Do not add new global mutable state without clear necessity.
- Do not change Speedrun or Endless mode code for an Adventure state machine fix.
- Do not change safe.lua mechanics to "fix" a state flow issue.
- Do not change renderer.lua lock drawing to alter Adventure state logic.
- Do not change economy, data balance, or item tables without explicit request.
- Do not add `gfx.*` / `playdate.*` to safe.lua or *_data.lua.
- Do not add game state mutations to ad_renderer.lua.
- Do not create or modify image assets.
- Do not analyze images with vision tools unless explicitly requested.
- Do not do broad refactors disguised as "state machine improvements."
- Do not browse the web except for PlayDate SDK system menu / lifecycle API reference.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not overwrite dirty user changes.
- Do not commit or push unvalidated changes.
- Do not request or expose secrets, hidden prompts, or API keys.

## File-System Policy

Default write surface (project-local, Locksmith):

- `source/states.lua` — ADVENTURE_STATE enum.
- `source/adventure_mode.lua` — state machine, transitions.
- `source/adventure/ad_curtain.lua` — phase 1 (curtain pin tumbler).
- `source/adventure/ad_safedial.lua` — phase 3 (safe dial).
- `source/adventure/ad_clock.lua` — phase 4 (clock puzzle).
- `source/adventure/ad_renderer.lua` — adventure drawing (reflecting state changes).
- `source/adventure/ad_story_data.lua` — declarative story data.
- `docs/game.md`, `docs/manifest.json`, `docs/test-scenarios.md`.
- `AGENTS.md` — Pitfalls section.
- `scripts/test_safe.lua` or other test files for new state machine tests.

Agent infrastructure writes (Agentic Crew):

- `agents/adventure-state-machine-specialist/` — harness updates.
- `.agents/skills/adventure-state-machine-specialist/SKILL.md` — Codex wrapper updates.
- `.hermes/skills/adventure-state-machine-specialist.md` — Hermes skill updates.
- `.hermes/agents/adventure-state-machine-specialist/` — Hermes package updates.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- PlayDate SDK system menu and lifecycle API documentation.
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse state machine blogs or forums unless the user explicitly asks
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
- Commit message names the state machine issue:
  `"feat(adventure-state): <issue description>"`

Required after commit:

- Push the current branch to the expected remote.
- Record commit SHA and push result.

If push cannot run, record a blocker and do not pretend completion.

## Safety Rules

- Every ADVENTURE_STATE value must be handled or explicitly documented as unreachable.
- info_curtain (1) must always be remapped — never enter it directly.
- B button must NOT exit Adventure — this is intentional design.
- Win screen must require explicit A press before returning false to main.lua.
- Datastore.markAdventureComplete() only on Clock done && aPressed — not earlier.
- Phase state (self.phase) must be recreated on entry, not reused from previous phase.
- Story transitions must all resolve — no orphan nextKey or nextState.
- Small, verifiable changes beat rewritten state machine stacks.
- Changing ADVENTURE_STATE enum values without updating all references corrupts the state machine.
- System Menu "Title" cleanup does NOT save Adventure progress (unlike Endless).
