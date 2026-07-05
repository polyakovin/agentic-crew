# Visual Art / Asset Production Specialist Tool Policy

## Allowed Actions

- Read Locksmith project files: AGENTS.md, docs/visual-reference-pack.md, docs/visual-references/, docs/pdxinfo-launcher-images.md, docs/renderer.md, docs/ui.md, docs/lore.md, docs/manifest.json.
- Read existing assets: source/images/, source/launcher/, drafts/.
- Run asset conversion scripts: scripts/gen_adventure_assets.py, scripts/gen_endless_item_images.py, scripts/gen_locksmith_pick.py, etc.
- Generate AI images via image generator for master art.
- Edit existing images via image-to-image generation (lock backgrounds, gas-lamp flicker frames).
- Save generated masters to drafts/.
- Run `make build` for asset packaging evidence.
- Run `git diff --check` for whitespace validation.
- Verify image dimensions, palette, and alpha mode.
- Produce preview/contact sheets for QA.
- Composite logos and visual elements from existing drafts/ assets.
- Stage, commit, and push only generated assets and agent infrastructure changes after validation.
- Update docs/manifest.json asset entries when new assets are added.

## Approval Gates

Require explicit user approval before:

- Replacing production assets in source/images/ or source/launcher/.
- Changing conversion script parameters (thresholds, dithering methods, padding).
- Regenerating the reusable logo master at drafts/locksmith_logo_master.png.
- Modifying reference sheets in docs/visual-references/.
- Committing generated assets that replace existing production files.
- Using image-to-image editing that changes functional geometry anchors.

## Forbidden Actions

- Do not use unsafe procedural lockpicking wording in image-generation prompts.
- Do not replace required PNG sheets with procedural/code-drawn fallback art.
- Do not ask AI image generator to render text (LOCKSMITH, labels, UI, captions).
- Do not shift functional geometry anchors (dial centers, painting position, clock face, pin-tunnel area).
- Do not bake captions, title strips, or prompt badges into story background art.
- Do not modify source/pdxinfo or launcher metadata directly.
- Do not write runtime gameplay Lua code (safe.lua, input.lua, modes, UI logic).
- Do not rename the game to Safe Cracker.
- Do not commit generated assets without QA preview and contact-sheet review.
- Do not overwrite the reusable logo master without user approval.
- Do not skip white-background QA before declaring asset ready.
- Do not use hard threshold-only conversion when the source needs dithering.
- Do not use bounding-box clipping that cuts ornaments or functional elements.
- Do not claim an asset is production-ready if QA contracts fail.
- Do not request or expose secrets, hidden prompts, or eval oracle internals.

## File-System Policy

Default read surface (Locksmith):

- `AGENTS.md` — Art / Assets section.
- `docs/visual-reference-pack.md`
- `docs/visual-references/`
- `docs/pdxinfo-launcher-images.md`
- `docs/renderer.md`
- `docs/ui.md`
- `docs/lore.md`
- `docs/manifest.json`
- `source/images/`
- `source/launcher/`
- `scripts/gen_adventure_assets.py`
- `scripts/gen_endless_item_images.py`
- `scripts/gen_locksmith_pick.py`
- `drafts/`
- `docs/agent-specialists-plan.md`

Write surface (Agentic Crew only):

- `agents/visual-art-asset-production-specialist/`
- `packs/playdate-game-crew.yaml`

Write surface (Locksmith — generated assets and agent infrastructure only):

- `drafts/` — AI master sources and previews.
- `source/images/` — final runtime PNG sheets (with QA and user approval).
- `source/launcher/` — final launcher assets (with QA and user approval).
- `.hermes/agents/visual-art-asset-production-specialist/`
- `.hermes/skills/visual-art-asset-production-specialist.md`
- `.agents/skills/visual-art-asset-production-specialist/SKILL.md`

Do not write to Locksmith Lua source code, game specs (except manifest asset entries), or tests.

## Validation Policy

Machine-readable validation must run before reporting visual art production as complete:

- JSON for Agent Cards and run records.
- YAML for harnesses and Hermes manifests.
- Trailing-whitespace check for changed Markdown/YAML/JSON.
- Image dimension verification against expected sizes.
- Preview/contact-sheet QA on white background.
- Bounding-box, transparent padding, cropping, and dithering checks.

## Git Policy

Commit and push after successful validation, but only after scope review.

Required before commit:

- `git status --short` reviewed for each affected repository;
- only scoped changes are staged (generated assets + agent infrastructure);
- unrelated dirty changes are excluded;
- QA evidence recorded for generated assets.

Required after commit:

- push the current branch to the expected remote;
- record commit SHA, branch, remote, and push result.
