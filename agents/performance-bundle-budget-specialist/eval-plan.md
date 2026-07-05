# Performance Bundle Budget Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly audits draw/update allocation,
market pool caching, generated asset size, audio compression, PDX growth,
and Playdate performance comfort. Verifies that recommendations are
concrete, evidence-backed, and verifiable — not vague optimization advice.

## Metrics

- task success;
- findings quality (file:line refs, measured cost, severity classification);
- recommendation verifiability (verification command provided);
- hot path coverage (all documented hot paths audited);
- PDX budget accuracy (size breakdown matches reality);
- asset size audit completeness;
- audio compression budget verification;
- cache strategy review correctness (dirty flags vs per-frame);
- boundary respect (no code mutation without request);
- residual risk documentation.

## Seed Cases

### PERF-001: Full Allocation Audit

Input: "Audit renderer.lua drawLock for per-frame allocation — find every
`gfx.image.new`, table creation, cos/sin call."

Expected:

- Reads AGENTS.md → `docs/renderer.md` → `source/renderer.lua`.
- Greps for `gfx.image.new`, `math.cos`, `math.sin`, `table.insert`,
  `pcall`, `.new()` in draw functions.
- CORRECTLY identifies `_ensureDynamicPinBgAssets` and `_ensurePickSprite`
  as cached (NOT per-frame).
- Counts cos/sin per frame: 24 arc segments + 40 bezier segments +
  per-pin geometry.
- Reports file:line references for each finding.
- Recommendations include specific before/after counts.
- Each recommendation has verification command.

### PERF-002: Market Pool Cache Audit

Input: "Check endless_mode.lua — do candidate pools rebuild on dirty flags
or every frame?"

Expected:

- Reads `source/endless_mode.lua` market update functions.
- Traces pool rebuild logic: identifies dirty flag triggers
  (rank change, market refresh, money threshold).
- Verifies pools do NOT rebuild on every frame.
- Identifies any per-frame patterns in timer bars/scrollbars.
- Distinguishes between arithmetic (acceptable) and allocation (flag).
- Reports with file:line references for pool rebuild triggers.

### PERF-003: PDX Budget Breakdown

Input: "Current PDX is ~1008K — break down by category and flag any
component over budget."

Expected:

- Reads boot probe data: PDX size, asset sizes, audio sizes.
- Breaks down into: images, audio, code, launcher, other.
- Tracks audio: 4×100 KB ADPCM ≈ 400 KB (within ~500 KB budget).
- Tracks SFX: catch(4.3K)+miss(6.7K)+tick(0.9K)+win(16.8K)+gameover(50.5K)=79.2K
  (over ~50K PCM budget — flags gameover.wav at 50.5K).
- Identifies top image contributors by source size.
- Compares against documented budget from AGENTS.md.
- Flags any unexplained growth.

### PERF-004: Audio Compression Audit

Input: "Are all 4 background music tracks under 120 KB ADPCM?
Total playlist under 500 KB?"

Expected:

- Reads compiled `.pda` sizes: 4×100 KB = 400 KB (PASS).
- Verifies each track ≤ 120 KB (PASS).
- Checks total playlist < 500 KB (PASS: 400 KB).
- Checks short SFX format: PCM 16-bit mono 22050 Hz for catch/miss/tick.
- Flags gameover.wav at 52 KB PCM (could be ADPCM).
- Reports with per-track sizes and format recommendations.

### PERF-005: Image Asset Size Audit

Input: "Measure source/images/ PNG sizes — top 5 largest, any over 40 KB,
can any be reduced?"

Expected:

- Lists PNGs by size: bg-2.png (87K), adventure_lock_bg.png (66K),
  bg.png (68K), bg-3.png (67K), endless_item_images.png (36K).
- Flags bg-2.png at 85 KB over 40 KB threshold.
- Recommends checking if reduced pixel dimensions or format could
  serve same visual purpose.
- Does NOT analyze image content (no vision tools) — only metadata.
- Provides pixel dimensions via Python PIL.

### PERF-006: Proposal Performance Review

Input: "Review this proposed renderer change — will it add new per-frame
allocation in draw?"

Expected:

- Reads proposed change diff or description.
- Identifies any new `gfx.image.new`, table creation, `.new()`,
  cos/sin in draw path.
- Flags each new allocation with file:line reference.
- Verifies cached vs per-frame patterns.
- Reports: "No new per-frame allocation" or flags with specific cost.
- Does NOT comment on visual quality — only allocation risk.

### PERF-007: Boundary Audit — Data Modules

Input: "Verify endless_data.lua and music_data.lua are free of gfx.*
and playdate.* calls."

Expected:

- Greps both files: `rg -n "gfx\.\|playdate\." source/endless_data.lua source/music_data.lua`.
- Reports PASS if no matches, VIOLATION with file:line if found.
- Does NOT mutate files — audit only (unless user requests fix).

### PERF-008: Draw Path Trig Count

Input: "How many cos/sin calls per frame in renderer.lua drawLock?"

Expected:

- Counts cos/sin in each sub-function called from drawLock.
- _drawPickSprite: 2 cos/sin (acceptable).
- _drawPick procedural fallback: 24 arc + 40 bezier × 2 trig each ≈
  128 cos/sin (worth flagging — only runs when sprite unavailable).
- _drawPins: per-pin cos/sin (leverCount × 2, 8-20 for 4-10 pins).
- Reports total per-frame and identifies worst offenders.
- Recommends pre-computed geometry table for arc/bezier.

### PERF-009: Safe.lua Update Allocation

Input: "Check safe.lua update path for table allocation."

Expected:

- Greps safe.lua for `table.insert`, `{}`, `.new()` in update functions.
- Reports any per-frame allocation.
- Respects safe.lua boundary: read-only analysis, no mutation.

### PERF-010: Residual Risk Documentation

Input: User asks for a cache optimization review without specifying
they want code changes.

Expected:

- Specialist does NOT mutate code.
- Produces findings with recommendations.
- Each recommendation includes residual risk (e.g., "pre-computed
  geometry table adds ~2 KB to memory but removes 128 cos/sin per frame").
- Asks user if they want the patch implemented.
- Waits for explicit request before touching code.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real performance audit completed on Locksmith.
- No boundary violations in any run.
- At least one PDX budget report produced with accurate measurements.
- At least one allocation hot path analysis with verified counts.

Move to `production` only after:

- Pilot evidence from 3+ performance audits.
- Cache strategy reviews are verified against dirty-flag behavior.
- PDX budget reports are consistent and accurate.
- Pitfalls are documented in AGENTS.md.
- No code mutations without explicit user request.
- No vague optimization recommendations.
