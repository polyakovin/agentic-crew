# Performance Bundle Budget Specialist Workflow

## Boot Probe

Run for any non-trivial performance analysis:

```bash
git status --short
cd /root/locksmith && du -sh Locksmith.pdx 2>/dev/null || echo "PDX not built"
cd /root/locksmith && du -sh source/images/ source/sounds/ 2>/dev/null
cd /root/locksmith && du -h Locksmith.pdx/sounds/*.pda 2>/dev/null | sort -rh
cd /root/locksmith && ls -laS source/images/*.png 2>/dev/null | head -20
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Missing PDX means no build evidence — note as blocker.
- Audio track sizes over ~120 KB per track ADPCM flag budget concern.
- Image sizes over ~50 KB source flag potential optimization targets.

## Core Workflow

1. **Read project context**: AGENTS.md (performance, audio, asset pitfalls
   sections) → `docs/manifest.json` → relevant specs (`docs/renderer.md`,
   `docs/soundfx.md`, `docs/economy-roadmap.md`).
2. **Run boot probe**: PDX size, asset directory sizes, audio track sizes.
3. **For allocation audit**: read hot path source files → grep for
   `gfx.image.new`, `table.insert`, `.new()`, `math.cos`, `math.sin`,
   `pcall` in draw/update functions → count calls → measure cost.
4. **For cache audit**: read `endless_mode.lua` → trace candidate pool
   logic → verify dirty flags → check timer bar/scrollbar per-frame patterns.
5. **For asset audit**: list `source/images/` PNGs with sizes → flag
   top contributors → recommend pixel format or dimension reductions.
6. **For audio audit**: list `source/sounds/` WAVs with metadata →
   verify ADPCM/PCM split → check track sizes against budget.
7. **For PDX audit**: measure total PDX size → break down by category →
   compare against documented budget → flag unexplained growth.
8. **Formulate findings**: file:line, current pattern, measured cost,
   recommendation, verification command, residual risk.
9. **Report**: evidence-first format.

## Allocation Hot Path Analysis Template

```markdown
## Allocation Hot Path Analysis

**Scope**: [which files/functions were audited]

## Hot Path Inventory

| Hot Path | File:Line | Per-Frame Calls | Allocation Pattern | Cost |
|----------|-----------|----------------|--------------------|------|
| drawLock | renderer.lua:46 | 1/frame | dispatches sub-drawers | N/A |
| _drawDynamicPinBackground | renderer.lua:122 | 1/frame | pcall(gfx.image.new) cached | 1 guard check |
| _drawPins | renderer.lua:297 | 1/frame | per-pin cos/sin | leverCount × cos/sin pairs |
| _drawPick (fallback) | renderer.lua:232-292 | 1/frame | 24 arc + 40 bezier = 64 × trig | 64 cos/sin per frame |
| _drawPickSprite | renderer.lua:163 | 1/frame | 2 trig for tilt | 2 cos/sin (acceptable) |

## Findings

### Critical
- [PERF-XXX] Finding description (file:line)
  - **Current pattern**: [what code does now]
  - **Measured cost**: [table alloc count, trig calls/frame, pool rebuild frequency]
  - **Recommendation**: [specific, with before/after]
  - **Verification command**: [how to confirm fix works]
  - **Residual risk**: [what could still regress]

### High
- [PERF-XXX] ...

### Medium
- [PERF-XXX] ...

### Low
- [PERF-XXX] ...

## Allocation Count Summary
- `gfx.image.new` in draw/update: N (N cached outside draw)
- table creation in draw/update: N
- `cos`/`sin` per frame: N
- `pcall` in draw/update: N
```

## PDX Size Budget Checklist

Verify periodically and after any asset change:

1. **Total PDX size**: `du -sh Locksmith.pdx` → compare against last audit.
2. **Audio breakdown**: `du -h Locksmith.pdx/sounds/*.pda | sort -rh` →
   each track under ~120 KB ADPCM? Total playlist under ~500 KB?
3. **Image breakdown**: `ls -laS source/images/*.png` → top 5 largest.
   Any over 40 KB? Can pixel format or dimensions be reduced?
4. **Growth trend**: if PDX grew since last audit, which category grew?
   Is the growth justified by a spec change?
5. **Budget note**: for any proposed asset change, note PDX budget impact.

## Asset Size Audit

```bash
# Top image assets by size
ls -laS /root/locksmith/source/images/*.png | head -10

# Audio track sizes (compiled .pda)
du -h /root/locksmith/Locksmith.pdx/sounds/*.pda | sort -rh

# Images with python to get dimensions
python3 -c "from PIL import Image; [print(f'{p.name}: {Image.open(p).size}') for p in sorted(pathlib.Path('/root/locksmith/source/images').glob('*.png'))]" 2>/dev/null
```

Recommended budget per track type:

| Track Type | Format | Max Size | Current (source) | Current (.pda) |
|-----------|--------|----------|-----------------|----------------|
| Background loop | IMA ADPCM mono 11025 Hz | 120 KB | ~100 KB WAV | ~100 KB |
| Short SFX | PCM 16-bit mono 22050 Hz | ~15 KB (catch/miss/tick) | 1-7 KB WAV | 4-8 KB |
| Long SFX (win, gameover) | IMA ADPCM or PCM | ~50 KB | 5-17 KB WAV | 20-52 KB |

## Audio Compression Audit

```bash
# Get audio metadata
for f in /root/locksmith/source/sounds/*.wav; do
  ffprobe -v quiet -show_format -show_streams "$f" 2>/dev/null | grep -E "codec_name|sample_rate|channels|duration|bit_rate" | head -5
  echo "---"
done
```

Verify:
1. Background loops (noir, bazaar, rainRoom, trainyard) → IMA ADPCM mono
   11025 Hz.
2. Short SFX (catch, miss, tick) → PCM 16-bit mono 22050 Hz for crisp
   transients.
3. Win/gameover → can be ADPCM if under budget.
4. Total audio PDX size: ~492 KB current (4×100 KB ADPCM + 92 KB SFX).

## Cache Strategy Review

```bash
# Check endless_mode.lua for dirty flag patterns
rg -n "dirty\|Dirty\|rebuild\|Rebuild\|candidate.*pool\|refreshPool\|buildPool" /root/locksmith/source/endless_mode.lua

# Check if pools rebuild on update or on trigger
rg -n "function.*[Uu]pdate\|function.*[Tt]ick\|function.*[Dd]raw" /root/locksmith/source/endless_mode.lua
```

Verify:
1. Candidate pools rebuild only on dirty triggers (rank change,
   market refresh, money threshold).
2. Market timer bars advance per-frame but do not allocate — use
   pre-computed widths or simple math.
3. Scrollbar positions computed per-frame from pre-computed table
   indices — no table creation.
4. No `table.insert` or `{}` in any function named `update`, `tick`,
   or `draw`.

## Architecture Boundary Audit

Before any performance analysis, verify:

```bash
# Check endless_data.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." /root/locksmith/source/endless_data.lua && echo "VIOLATION" || echo "OK"

# Check music_data.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." /root/locksmith/source/music_data.lua && echo "VIOLATION" || echo "OK"

# Check safe.lua for gfx calls (should have NONE)
rg -n "gfx\." /root/locksmith/source/safe.lua && echo "VIOLATION: gfx in safe" || echo "OK"

# Count gfx.image.new in renderer.lua (should only be in _ensure* functions, not in draw)
rg -n "gfx\.image\.new" /root/locksmith/source/renderer.lua

# Count cos/sin in renderer.lua hot paths
rg -c "math\.cos\|math\.sin" /root/locksmith/source/renderer.lua
```

## Validation Pack

Run from Locksmith root:

```bash
# PDX size
du -sh Locksmith.pdx 2>/dev/null || echo "PDX not built"

# Total asset sizes
echo "Images (source): $(du -sh source/images/ | cut -f1)"
echo "Sounds (source): $(du -sh source/sounds/ | cut -f1)"

# Audio track sizes (compiled)
du -h Locksmith.pdx/sounds/*.pda 2>/dev/null | sort -rh

# Top image assets
ls -laS source/images/*.png 2>/dev/null | head -10

# Hot path allocation check
echo "gfx.image.new in renderer: $(rg -c 'gfx\.image\.new' source/renderer.lua)"
echo "cos/sin in renderer: $(rg -c 'math\.(cos|sin)' source/renderer.lua)"
echo "table.insert in endless_mode: $(rg -c 'table\.insert' source/endless_mode.lua)"
```

## Pitfalls

1. **Counting `gfx.image.new` in `_ensure*` functions as hot-path:**
   `_ensureDynamicPinBgAssets` and `_ensurePickSprite` load images ONCE
   with cached boolean flags. These are NOT per-frame allocation. Only
   `gfx.image.new` inside functions called from draw/update count.

2. **Missing `pcall` in draw path:** even if `pcall` doesn't allocate,
   it adds overhead. Check if any `pcall` in draw/update can be replaced
   with a cached boolean flag (like `dynamicPinBgReady`).

3. **Per-frame cos/sin in procedural fallback:** the 24-segment arc and
   40-segment cubic bezier use cos/sin per segment every frame. The
   procedural fallback only runs when the pick sprite is unavailable,
   but it's still worth flagging for when it does run.

4. **ADCMP vs PCM confusion:** the source `.wav` files are 100 KB each,
   but after `pdc` compilation they become `.pda` ADPCM at ~100 KB each.
   Don't flag source WAV size — flag compiled PDA size against budget.

5. **Timer bars that look like allocation:** market timer bars advancing
   per frame may compute widths from `elapsed/total * maxWidth` — this
   is arithmetic, not allocation. Verify before flagging.

6. **Candidate pool rebuild confusion:** pools rebuilt on dirty flags
   are correct behavior. Flag only pools that rebuild EVERY frame
   regardless of state change.

7. **Static analysis only — no Simulator:** this specialist does NOT
   run `make build` or launch the Simulator. Recommendations are based
   on code patterns and static measurement. Verification commands
   are for the user to run.

8. **Don't flag data modules that are already clean:** `endless_data.lua`
   (441 lines) and `music_data.lua` (26 lines) are declarative data
   and should already be free of `gfx.*` and `playdate.*`. Verify but
   don't assume violation.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped performance analysis changes.
3. Commit with atomic message: `"perf: <change description>"`.
4. Push the current branch.
5. Record commit SHA and push result.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- credentials or branch policy prevent push.
