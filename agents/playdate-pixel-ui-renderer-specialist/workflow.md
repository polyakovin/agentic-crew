# Playdate Pixel UI / Renderer Workflow

## Boot Probe

Run for visual-sensitive work unless explicitly forbidden:

```bash
git status --short
command -v pdc
pdc --version 2>/dev/null || echo "pdc not found"
```

Interpretation:

- `pdc` present: PDX packaging/build can be validated.
- `pdc` missing: `make build` blocked; report honestly.

## Surface Classifier

- `renderer-layer`: draw calls in `source/renderer.lua` — lock anatomy, pins,
  springs, pick, shear line, crank animation.
- `ui-screen`: static screens in `source/ui.lua` — title, settings, gameover,
  HUD chips, top/bottom bars, feedback.
- `mode-draw`: mode-specific draw in `source/adventure_mode.lua`,
  `source/speedrun_mode.lua`, `source/endless_mode.lua` and their
  adventure/ sub-modules.
- `hud`: top bar, bottom controls, feedback chip, timer display.
- `menu`: title buttons, settings rows, mode selector.
- `feedback`: MISS, NO COINS, LOCK OPENED, and other transient messages.
- `boundary`: architectural separation — does this file violate its component role?
- `style`: noir/mechanical/safe-lock visual fit.
- `readability`: text cropping, overlap, font choice, 1-bit contrast.
- `coordinate`: Y-axis sign, fillRect semantics, polygon vertex ordering.

## Core Workflow

1. Classify surface and risk tier.
2. Read routed sources from `taskBrief.whatToRead`.
3. Read relevant spec (`docs/renderer.md` or `docs/ui.md`).
4. Check `AGENTS.md` Pitfalls for related known bugs.
5. Inspect affected source files.
6. Classify the issue and build evidence map.
7. Act within allowed scope: recommend fix, implement fix, or flag for other specialist.
8. Run relevant automation pack.
9. Update project memory for new pitfalls.
10. Produce A2A handoff and run record when required.

## Draw-Mode Audit

```bash
rg -n "setColor|setImageDrawMode|drawTextAligned|image:draw|:draw\\(" source/renderer.lua source/ui.lua source/*mode*.lua source/adventure/*.lua
```

Check that:

- Every `setColor(kColorBlack)` or `setColor(kColorWhite)` is followed by
  `setImageDrawMode(kDrawModeCopy)` before the next `image:draw()` or
  `drawTextAligned()`.
- No bitmap/text draw happens while `setColor` is still the active draw state.
- The pattern is: `setColor(...)` → draw primitives → `setImageDrawMode(kDrawModeCopy)` → bitmap/text.

## Boundary Check

```bash
rg -n "playdate\\.|gfx\\." source/safe.lua source/*_data.lua source/*_catalogs.lua
rg -n "Safe\\.|safe\\." source/renderer.lua  # allowed: read-only
rg -n "Safe\\.|safe\\." source/input.lua     # forbidden
rg -n "playdate\\." source/ui.lua            # allowed: gfx for drawing only
```

Expected:

- `safe.lua`: zero `playdate.` or `gfx.` calls.
- `*_data.lua`, `*_catalogs.lua`: zero `playdate.`, `gfx.`, persistence, lifecycle calls.
- `input.lua`: zero `Safe.` or `safe.` references.
- `renderer.lua`: `Safe.` read-only access only; no mutations.
- `ui.lua`: `gfx.*` for drawing only; no game logic.

## Pixel QA

```bash
rg -n "fillRect|fillPolygon|fillTriangle|drawText|getTextSize|drawTextAligned" source/renderer.lua source/ui.lua source/*mode*.lua
rg -n "geometry\\.polygon\\.new" source/renderer.lua source/ui.lua source/*mode*.lua
```

Check:

- `fillPolygon` uses `playdate.geometry.polygon.new(...)` with closed polygon
  (last point == first point).
- `fillTriangle` uses flat arguments: `gfx.fillTriangle(x1,y1,x2,y2,x3,y3)`.
- `fillRect(x, y, w, h)` — confirm intent: fills y to y+h-1.
- Coordinate calculations: +Y is down. "Above" = smaller Y. "Below" = larger Y.
- Text `getTextSize` used to prevent cropping/overflow.
- Font role from `Fonts` registry used, not direct `gfx.font.new`.

## Build Verify

Preferred project gate:

```bash
make check
```

If Lua is missing and gate evidence is required:

```bash
make build
```

Report:

- `make check` blocked at Lua/runtime check: missing runtime, not failing tests.
- `make build` passing: PDX syntax/package validated, tests/lint not validated.

## Style Audit

Visual style checklist:

- Noir atmosphere: high contrast, deep blacks, mechanical precision.
- Tactile feel: elements convey physicality (engraved, embossed, mechanical).
- Safe-lock focus: visual hierarchy centers on the lock mechanism.
- No SaaS aesthetic: no rounded pill buttons, no pastels, no modern minimalism.
- 1-bit readability: dither patterns instead of grayscale, clear silhouettes.
- Font choice: `primary` (m5x7) for UI/HUD, `small` (m3x6) for secondary text,
  `display` (Jersey 10) for headings only.
- Text legibility: sufficient contrast against background, no clipping at edges.

## Surface Checklists

Renderer:

- drawLock: layer order correct (pin-tunnel → pins → crank → pick → shear line).
- drawCrank: contrast-safe on dark backgrounds.
- drawAButton/drawBButton: 3D button visuals correct.
- drawTutorial: crank + A button layout clear.
- No game state mutations.
- dtMs used for animation, not game timing.
- `images/pin_tunnel` placed at correct offset.

UI:

- drawTitle: background only, no title text embedded in draw.
- drawTitleButtons: selected index highlighted, centered vertically.
- drawSettings: top-5 scores, Music toggle, Dev Mode sub-toggles.
- drawTopBar/drawBottomControls: opaque bars, correct text placement.
- drawHudChip: compact, opaque, readable.
- drawFeedback: centered transient chip.
- drawTimer: M:SS format, top bar compatible.
- drawGameover: score + high scores + "B: Back".
- Font fallbacks handled without crash.

Mode Draw:

- Adventure: no topBar/bottomBar (scene-embedded hints).
- Speedrun: timer, HUD, lock rendering via renderer.
- Endless: economy UI, item cards, HUD chips.
- Each mode owns its background image draw.
