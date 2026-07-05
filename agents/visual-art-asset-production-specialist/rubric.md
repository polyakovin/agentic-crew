# Visual Art / Asset Production Specialist Rubric

Use this rubric before handing off a visual art production report.

## Excellent

- Prompt uses safe, art-focused wording — no lockpicking instruction phrasing.
- Master art saved to `drafts/` with descriptive filename before conversion.
- Conversion flows through the correct repo asset pipeline (`scripts/gen_*.py`).
- Preview/contact sheet generated and reviewed on white background.
- Bounding boxes clean: no neighboring-cell fragments leaking into tiles.
- Transparent padding meets contract: min 8px sides, min 12px top for items.
- Cropping verified: no clipped ornaments, letters, or functional elements.
- Dithering preserves detail: no large non-informative black blobs.
- Final dimensions match expected sizes exactly.
- Palette/alpha correct: 1-bit for launcher/title, transparent RGBA black ink for items.
- Title/launcher art composites reusable logo from `drafts/locksmith_logo_master.png`.
- Lock background preserves canvas, lock bounds, screws, edge anchors.
- Functional geometry anchors unchanged.
- Affected manifest/spec entries updated before runtime references.
- QA evidence recorded (preview paths, dimension checks, dither quality).
- Game consistently called Locksmith (hero Silas Crane).
- Validation evidence recorded.
- Scoped changes committed and pushed.

## Acceptable

- Prompt is safe but could be more specific to Locksmith style.
- Master art saved but filename not descriptive.
- Conversion pipeline used with correct parameters.
- Preview reviewed but some minor bounding-box issues noted.
- Padding close to contract but not pixel-perfect.
- Cropping mostly correct with minor edge concerns.
- Dithering mostly preserves detail.
- Dimensions correct.
- Palette correct.
- Logo composited but positioning could be improved.
- Lock anchors preserved but some surface detail lost.
- Manifest impact documented but not yet applied.
- Game named correctly.
- Minor validation gaps.

## Needs Revision

- Prompt contains borderline lockpicking phrasing.
- Master art not saved to `drafts/`.
- Pipeline bypassed — procedural fallback used instead of script conversion.
- No preview/contact sheet generated.
- Bounding boxes show clear neighbor-cell leaks.
- Transparent padding significantly below contract.
- Ornaments or functional elements clipped.
- Dithering produces large non-informative black blobs.
- Wrong dimensions for asset type.
- Title/launcher art has AI-rendered text instead of composited logo.
- Lock background shifts lock bounds, screws, or anchors.
- Geometry anchors moved without documentation.
- Manifest not updated despite new asset references.
- Game called Safe Cracker.
- No QA evidence provided.

## Critical Failure

- Used unsafe procedural lockpicking wording in prompt.
- Replaced required PNG sheet with procedural fallback without user approval.
- Generated art with baked-in text (LOCKSMITH, labels, UI).
- Shifted functional geometry anchors causing gameplay misalignment.
- Overwrote production assets in `source/images/` or `source/launcher/` without QA and user approval.
- Committed generated assets without preview/contact-sheet review.
- Modified `source/pdxinfo` or launcher metadata.
- Wrote runtime Lua gameplay code.
- Renamed game to Safe Cracker.
- Falsified QA evidence or dimension checks.
- Overwrote `drafts/locksmith_logo_master.png` without user approval.
- Used hard threshold-only conversion destroying halftone/linework detail.
- Skipped validation and claimed complete.
- Committed or pushed unrelated dirty worktree changes.

## Challenge Checklist

- Is the prompt safe (no lockpicking instruction phrasing)?
- Is master art saved to `drafts/` before conversion?
- Is conversion using the repo asset pipeline (not procedural fallback)?
- Is preview/contact sheet reviewed on white background?
- Are bounding boxes clean (no neighbor-cell leaks)?
- Is transparent padding meeting contract?
- Is cropping correct (no clipped elements)?
- Does dithering preserve detail?
- Are dimensions exactly correct for asset type?
- Is palette/alpha correct?
- Is title/launcher art using composited logo (not AI-rendered text)?
- Are lock background anchors and safe zones preserved?
- Are functional geometry anchors unchanged?
- Is `docs/manifest.json` updated before runtime references?
- Is game called Locksmith?
- Is QA evidence recorded?
- Is validation run?
- Are scoped changes committed and pushed?
