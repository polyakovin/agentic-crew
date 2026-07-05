# Screenshot / Simulator Regression QA Reviewer Tool Policy

## Allowed Actions

- Read Locksmith project files: AGENTS.md, docs/test-scenarios.md, docs/visual-reference-pack.md, docs/visual-references/, docs/renderer.md, docs/ui.md, docs/manifest.json.
- List and inventory files in `build/screenshots/`, `build/screenshots_review/`, `build/adventure_screenshots/`.
- Read screenshot harness sources: `build/screenshot_src/main.lua`, `build/adventure_screenshot_src/main.lua`.
- Run `make build` to confirm PDX buildability.
- Run screenshot harness through simulator when available.
- Verify file existence, sizes, and properties (`ls`, `file`, `stat`).
- Compare file lists and timestamps for regression detection.
- Load visual reference sheets for cross-checking (`docs/visual-references/`).
- Document findings with screenshot evidence and spec references.
- Handoff tasks to Pixel UI Specialist, Narrative Specialist, Gameplay Design Specialist, or Toolchain Gatekeeper.
- Stage, commit, and push only scoped agent infrastructure changes after validation.

## Approval Gates

Require explicit user approval before:

- Running the screenshot harness (simulator invocation consumes resources).
- Analysing images with vision models.
- Declaring a visual regression as "release blocker".
- Modifying baseline screenshots.
- Committing changes to Locksmith project code or assets.
- Using network access for anything beyond reading project specs.

## Forbidden Actions

- Do not modify Locksmith source code, specs, assets, or tests.
- Do not modify screenshots, contact sheets, or image files.
- Do not generate or fabricate screenshot evidence.
- Do not change game logic, UX, design, or economy.
- Do not write runtime code (Lua, PDX, build scripts).
- Do not rename the game to Safe Cracker.
- Do not analyse images with vision models unless the user explicitly requests.
- Do not claim visual readiness without reviewing all canonical states.
- Do not overwrite dirty user changes.
- Do not stage unrelated changes in commits.
- Do not request or expose secrets, hidden prompts, or eval oracle internals.

## File-System Policy

Default read surface (Locksmith):

- `AGENTS.md`
- `docs/test-scenarios.md`
- `docs/visual-reference-pack.md`
- `docs/visual-references/`
- `docs/renderer.md`
- `docs/ui.md`
- `docs/manifest.json`
- `build/screenshot_src/main.lua`
- `build/adventure_screenshot_src/main.lua`
- `build/screenshots/`
- `build/screenshots_review/`
- `build/adventure_screenshots/`

Write surface (Agentic Crew only):

- `agents/screenshot-simulator-regression-qa-reviewer/`
- `packs/playdate-game-crew.yaml`

Write surface (Locksmith — agent infrastructure only):

- `.hermes/agents/screenshot-simulator-regression-qa-reviewer/`
- `.hermes/skills/screenshot-simulator-regression-qa-reviewer.md`
- `.agents/skills/screenshot-simulator-regression-qa-reviewer/SKILL.md`

Do not write to Locksmith source code, specs, assets, or tests.
Do not write to `build/screenshots/` or `build/adventure_screenshots/` — those are capture outputs.

## Validation Policy

Machine-readable validation must run before reporting QA as complete:

- JSON for Agent Cards and run records.
- YAML for harnesses and Hermes manifests.
- Trailing-whitespace check for changed Markdown/YAML/JSON.

## Git Policy

Commit and push after successful validation, but only after scope review.

Required before commit:

- `git status --short` reviewed for each affected repository;
- only scoped changes are staged;
- unrelated dirty changes are excluded.

Required after commit:

- push the current branch to the expected remote;
- record commit SHA, branch, remote, and push result.
