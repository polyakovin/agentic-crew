# Visual Art / Asset Production Specialist Workflow

## Boot Probe

Run before any visual art task:

```bash
git status --short
ls source/images/ source/launcher/ drafts/ 2>&1
python3 -c "from PIL import Image; print('Pillow available')" 2>&1 || echo "Pillow missing — image ops limited"
```

Interpretation:

- Dirty worktrees: note but do not block art production (we modify assets, not code).
- Missing Pillow: image conversion scripts may fail; document as blocker for pipeline tasks.
- Missing `source/images/` or `drafts/`: flag as blocker for tasks needing those sources.

## Core Workflow

1. Read `docs/visual-reference-pack.md` and relevant `docs/visual-references/` sheet.
2. Identify asset type and required dimensions from specs.
3. Craft safe, art-focused prompt (per prompt patterns in visual-reference-pack.md).
4. Generate master art via image generator → save to `drafts/`.
5. Run appropriate conversion script (`scripts/gen_*.py`) with documented parameters.
6. Produce preview/contact sheet for QA.
7. Review on white background: silhouettes, bounding boxes, padding, cropping, dithering.
8. Verify dimensions against expected sizes.
9. Run available non-Lua gates: `make build`, `git diff --check`.
10. Update `docs/manifest.json` or spec entries if new asset is referenced at runtime.
11. Provide visual QA report with evidence.
12. Handoff to Release / Storefront Packaging Specialist if launcher assets need integration.

## Title / Launcher Production

When producing title or launcher art:

### Prompt Crafting

Use the `Title/card background prompt shape` from `docs/visual-reference-pack.md`:

```text
Fictional Playdate title/launcher background for Locksmith, antique mechanical lock
atmosphere, detective noir, gaslit locksmith workbench, one old gas lamp, framed wall art,
high-contrast monochrome engraved ink illustration, clean calm area reserved for a
separately composited logo, no text, no watermark, no modern flashlight, no extra characters.
```

### Generation And Compositing

1. Generate noir/gaslight background only — do NOT ask AI to render `LOCKSMITH` text.
2. Save master to `drafts/title_ai_master_<timestamp>.png`.
3. Composite logo: use `drafts/locksmith_logo_master.png`, about 7% smaller than 178px-wide draft, positioned with full upper/lower flourishes visible.
4. Convert to 1-bit: Shiau-Fan error diffusion, no protected black safe-area masks.
5. Produce sizes:
   - `source/images/main_menu_bg.png` — 400×240 (title screen background).
   - `source/launcher/card.png` — 350×155 (launcher card).
   - `source/launcher/launch-image.png` — 400×240 (launch screen).
   - `source/launcher/icon.png` — 32×32.
6. Preview on white background, verify logo not clipped, ornaments not colliding.

### Card-Highlighted Animation

1. Use `drafts/card_ai_variation_*` masters for exploration before replacing packaged frames.
2. Generate gas-lamp flicker frames: use the gas-lamp flicker prompt shape.
3. Each frame: 350×155, 1-bit, only flame/local light changes.
4. Curate frames: drop any with brightness/geometry/dither texture breaks.
5. Package in `source/launcher/card-highlighted/` with `animation.txt`.
6. Sequence must not end on same frame it starts with.

## Adventure Story Production

When producing Adventure story backgrounds:

### Prompt Crafting

Use the `Adventure story prompt shape` from `docs/visual-reference-pack.md`:

```text
400x240 Playdate story background, 1-bit noir/gaslight Hale mansion visual language,
strong silhouettes, engraved hatching, no text or UI panels. Preserve the described camera
angle and story object focus; leave room for small runtime line-chip captions.
```

### Pipeline

1. Generate high-resolution master → `drafts/adventure_ai_<scene>_master.png`.
2. Run `scripts/gen_adventure_assets.py` with shadow-lifted ordered dithering.
3. Parameters: threshold ~98–100, spread ~76–78 for story backgrounds.
4. Outputs: `source/images/adventure_story_bg_<key>.png` at 400×240.
5. Verify: story slides must not bake captions, title strips, or prompt badges into art.
6. For safe_dial scene: stethoscope diegetic cue visible, hand holding stethoscope near door.
7. For curtain_lock scene: camera from lock's left end, keyhole visible left side, no hinge on right.

## Lock Background Editing

When regenerating lock background art:

### Prompt Crafting

Use the `Lock-background edit prompt shape` from `docs/visual-reference-pack.md`:

```text
Use the current lock background as the edit target. Preserve canvas size, lock bounds,
wall/frame/background, screws, edge anchors, and gameplay-safe zones. Redraw only the
lock body surface detail in place. It must read as a closed exterior side-view antique
lock, not a cutaway. No exposed pins, springs, chambers, internal tunnels, visible key,
new room, text, handle mechanism, character, or unrelated props.
```

### Pipeline

1. Use current `source/images/bg.png` as edit target via image-to-image generation.
2. Preserve: 400×240 canvas, lock bounds, screws, edge anchors, gameplay-safe zones.
3. Redraw only: lock body surface detail.
4. Result must read as closed exterior side-view, not a cutaway.
5. `source/images/bg.png` must stay free of baked pin tunnels.
6. `source/images/adventure_lock_bg.png` must match the same contract.
7. Verify pin-tunnel placement still aligns: `source/images/pin_tunnel.png` cropped from `source/images/pin-tunnel.png` at 20×93.

## Endless Item Production

When producing Endless item images:

### Prompt Crafting

Use the `Endless item-source prompt shape` from `docs/visual-reference-pack.md`:

```text
Create a clean contact sheet of isolated noir locksmith/economy item illustrations on a
white background, arranged as a 6-column by 5-row grid. Each cell contains one grounded
world object with generous top padding, strong black silhouette, readable white highlights,
and no overlapping neighboring-cell fragments. Style: monochrome engraved Playdate item art.
```

### Pipeline

1. Generate master → `drafts/endless_item_images_generated_source.png`.
2. Run `scripts/gen_endless_item_images.py`:
   - `threshold=176`, `trim_threshold=210`, `tile_padding=8`.
   - `min_transparent_padding=8`, `min_top_padding=12`.
   - Bayer dithering, `ordered_strength=36`.
   - `source_bleed_padding=24`, `source_anchor_inset=24`.
3. Output: `source/images/endless_item_images.png` — horizontal transparent strip, 30 tiles × 138×138.
4. Preview: `drafts/endless_item_images_preview.png` on white background.
5. QA: `drafts/endless_item_images_qa.txt` — read for issues.
6. Verify: no neighboring-cell fragments, transparent padding, top-edge safety.

## Prompt Crafting Rules

1. Always check `docs/visual-reference-pack.md` prompt patterns before prompting.
2. Use safe, art-focused wording — avoid lockpicking instruction phrasing.
3. Do not ask AI to render text (`LOCKSMITH`, labels, UI, captions).
4. Do not describe internal lock mechanisms as instructions.
5. Reference specific visual style: 1-bit, noir, gaslight, engraved hatching, strong silhouettes.
6. Specify canvas constraints: 400×240, 350×155, or grid dimensions.
7. Request clean white background for item sheets, transparent for pick sprites.
8. Save every AI master to `drafts/` with descriptive filename before conversion.

## 1-Bit Conversion

Pipeline gates for every conversion:

1. Source: high-resolution master in `drafts/`.
2. Script: appropriate `scripts/gen_*.py` with documented parameters.
3. Dithering: shadow-lifted ordered (adventure), Bayer ordered (items), Shiau-Fan error diffusion (title/launcher).
4. Palette: 1-bit for runtime backgrounds, transparent RGBA black ink for items/sprites.
5. Post-conversion: preview on white background.
6. Size verification: exact dimensions per asset contract.

## Preview / Contact-Sheet QA

For every generated asset, before declaring ready:

1. Open the generated image on a white background.
2. Check silhouettes: are objects readable as their intended shapes?
3. Check bounding boxes: any neighboring-cell fragments leaking into tiles?
4. Check transparent padding: min 8px sides, min 12px top for items.
5. Check cropping: any ornaments, letters, or functional elements clipped?
6. Check dithering: any large non-informative black blobs where detail should be?
7. Check dimensions: correct size for asset type.
8. For lock backgrounds: verify lock bounds, screws, edge anchors, safe zones.
9. For title/launcher: verify logo fully visible, no prop collisions.
10. Record issues in QA report.

## Handoff Rules

When launcher assets need packaging integration:

- Handoff to `release-storefront-packaging-specialist` with:
  - Asset paths and dimensions.
  - QA evidence (preview passed).
  - Any integration notes (animation.txt updates, pdxinfo impacts).

When visual defects found in screenshots:

- Receive from `screenshot-simulator-regression-qa-reviewer` with:
  - Screenshot path and specific defect.
  - Affected asset and dimension.
  - Regeneration scope.

When pipeline/script failures occur:

- Handoff to `toolchain-build-sandbox-gatekeeper` with:
  - Script name and error output.
  - Python/Pillow availability.
  - Whether this blocks art production.

## Validation Pack

Run from the Agentic Crew repository root:

```bash
python3 -m json.tool agents/visual-art-asset-production-specialist/agent-card.json
python3 -m json.tool agents/visual-art-asset-production-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/visual-art-asset-production-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/visual-art-asset-production-specialist
```

Also verify from Locksmith root when Hermes wrappers exist:

```bash
cd /root/locksmith && python3 -c "import yaml; yaml.safe_load(open('.hermes/agents/visual-art-asset-production-specialist/manifest.yaml'))"
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state in each affected repository.
2. Stage only scoped changes.
3. Commit with an atomic message that names the agent.
4. Push the current branch.
5. Record commit SHA, branch, remote, and push result.

Block instead of committing when validation failed or unrelated dirty files would be staged.
