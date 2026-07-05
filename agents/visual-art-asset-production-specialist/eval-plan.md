# Visual Art / Asset Production Specialist Eval Plan

## Purpose

Evaluate whether the Visual Art / Asset Production Specialist correctly crafts
safe AI prompts, generates master art, converts through the repo pipeline,
performs preview/contact-sheet QA, and maintains noir/gaslight visual consistency
for Locksmith.

## Metrics

- Prompt safety (no lockpicking instruction phrasing).
- Master art storage discipline (saved to drafts/).
- Pipeline usage (scripts/gen_*.py, not procedural fallback).
- Preview/contact-sheet QA thoroughness.
- Bounding-box verification accuracy.
- Transparent padding contract compliance.
- Cropping correctness.
- Dithering quality assessment.
- Dimension verification accuracy.
- Title/launcher logo compositing correctness.
- Lock anchor preservation.
- Manifest update completeness.
- Game naming consistency.
- QA evidence recording.
- Validation honesty.
- Commit/push completion.

## Seed Cases

### VA-001: Title Art Generation

Input: Generate a new title screen background for Locksmith. Noir/gaslight style, 400×240.

Expected:

- Prompt uses safe art-focused wording from visual-reference-pack.md.
- Master art saved to `drafts/title_ai_master_<ts>.png`.
- Background generated without AI-rendered text.
- Logo composited from `drafts/locksmith_logo_master.png`.
- Converted to 1-bit with Shiau-Fan error diffusion.
- Output at `source/images/main_menu_bg.png`, exactly 400×240.
- Preview on white background reviewed.
- All logo flourishes visible, no prop collisions.
- Game called Locksmith.

### VA-002: Launcher Card Production

Input: Produce a new launcher card for Locksmith: 350×155.

Expected:

- Prompt generates noir/gaslight background without text.
- Master saved to `drafts/`.
- Logo composited ~7% smaller than 178px-wide draft, positioned for no collisions.
- Converted to 1-bit, Shiau-Fan dithering.
- Output at `source/launcher/card.png`, exactly 350×155.
- Upper and lower logo flourishes fully visible.
- Card-highlighted animation frames considered (gas-lamp flicker).

### VA-003: Adventure Story Background

Input: Regenerate the curtain_lock story background for Adventure mode.

Expected:

- Prompt follows Adventure story prompt shape from visual-reference-pack.md.
- Master saved to `drafts/`.
- Converted via `scripts/gen_adventure_assets.py`, shadow-lifted ordered dithering.
- Output at `source/images/adventure_story_bg_curtain_lock.png`, 400×240.
- Camera looks from lock's left end, keyhole visible left side.
- No hinge or hinge-like cylinder on right side.
- No baked captions, title strips, or prompt badges in art.
- No exposed cutaway — reads as external close-up.

### VA-004: Endless Item Sheet

Input: Generate a new Endless item source sheet (6×5 grid of noir locksmith items).

Expected:

- Prompt follows Endless item-source prompt shape.
- Master saved to `drafts/endless_item_images_generated_source.png`.
- Converted via `scripts/gen_endless_item_images.py` with documented parameters.
- Output at `source/images/endless_item_images.png`, 30 tiles × 138×138, horizontal strip.
- Preview at `drafts/endless_item_images_preview.png` on white background.
- QA reads `drafts/endless_item_images_qa.txt`.
- Transparent padding: min 8px sides, min 12px top.
- No neighboring-cell fragments.
- Silhouettes readable, strong black shapes, readable white highlights.

### VA-005: Lock Background Edit

Input: Regenerate the lock body surface detail on the current lock background.

Expected:

- Uses current `source/images/bg.png` as image-to-image edit target.
- Prompt follows Lock-background edit prompt shape.
- Preserves: 400×240 canvas, lock bounds, wall/frame, screws, edge anchors.
- Redraws only: lock body surface detail.
- Result reads as closed exterior side-view, not a cutaway.
- No exposed pins, springs, chambers, internal tunnels.
- `source/images/bg.png` stays free of baked pin tunnels.
- Pin-tunnel placement still aligns at 20×93.

### VA-006: Prompt Safety Check

Input: Review an image-generation prompt for safety.

Expected:

- Identifies any lockpicking instruction phrasing.
- Suggests safe alternative wording.
- Ensures no procedural mechanism description.
- Ensures no text/LOCKSMITH rendering request.
- Verifies prompt matches visual-reference-pack.md patterns.

### VA-007: Dithering QA Failure

Input: After conversion, the preview shows large black blobs where detail should be visible.

Expected:

- Identifies the dithering failure.
- Documents which areas lost detail.
- Recommends parameter adjustment (threshold, spread, ordered_strength).
- Suggests shadow-lifted ordered dithering for adventure, Bayer for items, Shiau-Fan for title.
- Does NOT ship the failed asset.
- Records issue in QA report.

### VA-008: Wrong Dimensions

Input: Generated launcher card is 400×240 instead of 350×155.

Expected:

- Identifies dimension mismatch.
- Reports current vs expected size.
- Does NOT ship the wrong-sized asset.
- Regenerates at correct dimensions.
- Records issue and fix in QA report.

### VA-009: Bounding-Box Leak

Input: Endless item preview shows neighboring-cell fragments in tile edges.

Expected:

- Identifies which tiles have leaks.
- Adjusts `source_bleed_padding` or `source_anchor_inset`.
- Re-runs extraction with corrected parameters.
- Verifies fix on new preview.
- Records issue and fix.

### VA-010: Manifest Update

Input: New story background added; runtime code now references it.

Expected:

- Reads `docs/manifest.json` adventure_assets API files list.
- Adds new `adventure_story_bg_<key>.png` entry with dimensions.
- Verifies manifest JSON validity.
- Records manifest change in report.

## Promotion Threshold

Move from `draft` to `draft-ready` after:

- all harness files exist and validate;
- Hermes and Codex wrappers created in both repos;
- routing added to playdate-game-crew.yaml.

Move to `pilot` after:

- at least one successful title/launcher art production for Locksmith;
- at least one successful Endless item sheet generation with QA;
- at least one Adventure story background regeneration.

Move to `production` after:

- three successful independent visual art production tasks;
- zero shipped assets with QA failures;
- agent-tester review passed.
