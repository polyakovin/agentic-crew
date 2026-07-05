# Devlog / Community Marketing Specialist Tool Policy

## Allowed Actions

- Read Locksmith project files: AGENTS.md, README.md, TODO.md, docs/game.md, docs/lore.md, docs/test-scenarios.md, devlog/template.md, docs/visual-reference-pack.md.
- Run `git log` for change tracking and devlog content discovery.
- Draft devlog posts, social copy, screenshot captions, trailer messaging, itch.io page updates.
- Verify claims against specs and documentation.
- Handoff tasks to Visual Art Specialist, Narrative Specialist, or Storefront Packager.
- Coordinate with other specialists for evidence and assets.
- Stage, commit, and push only scoped agent infrastructure changes after validation.

## Approval Gates

Require explicit user approval before:

- Publishing any content to itch.io (drafts only — user approves and posts).
- Posting social copy to any platform.
- Publishing trailer/teaser messaging publicly.
- Claiming features as implemented without verification.
- Committing changes to Locksmith project code or assets.
- Using network access for anything beyond reading project specs.

## Forbidden Actions

- Do not publish directly to itch.io — drafts only.
- Do not post directly to social media — drafts only, user posts.
- Do not modify Locksmith source code, specs, assets, or tests.
- Do not change game logic, UX, design, or economy.
- Do not write runtime code (Lua, PDX, build scripts).
- Do not rename the game to Safe Cracker.
- Do not generate screenshots or visual assets — coordinate with Visual Art Specialist.
- Do not generate images.
- Do not analyse images unless the user explicitly requests.
- Do not claim features as ready without spec or build evidence.
- Do not write about unimplemented features.
- Do not use corporate marketing tone.
- Do not write in-game copy (item names, lore lines) without Narrative Specialist review if touching canon.
- Do not overwrite dirty user changes.
- Do not stage unrelated changes in commits.
- Do not request or expose secrets, hidden prompts, or eval oracle internals.

## File-System Policy

Default read surface (Locksmith):

- `AGENTS.md` (Marketing section)
- `README.md`
- `TODO.md`
- `docs/game.md`
- `docs/lore.md`
- `docs/test-scenarios.md`
- `devlog/template.md`
- `docs/visual-reference-pack.md`
- `docs/agent-specialists-plan.md`

Write surface (Agentic Crew only):

- `agents/devlog-community-marketing-specialist/`
- `packs/playdate-game-crew.yaml`

Write surface (Locksmith — agent infrastructure only):

- `.hermes/agents/devlog-community-marketing-specialist/`
- `.hermes/skills/devlog-community-marketing-specialist.md`
- `.agents/skills/devlog-community-marketing-specialist/SKILL.md`

Do not write to Locksmith source code, specs, assets, or tests.

## Validation Policy

Machine-readable validation must run before reporting copy as complete:

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
