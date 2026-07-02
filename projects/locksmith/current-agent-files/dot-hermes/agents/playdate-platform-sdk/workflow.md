# Playdate Platform / SDK Workflow

## Boot Probe

Run for platform-sensitive work unless explicitly forbidden:

```bash
git status --short
command -v pdc
pdc --version
ls -l "$(command -v pdc)"
command -v lua
command -v lua5.4
command -v lua5.3
command -v luajit
```

Interpretation:

- `pdc` present: `make build` can validate PDX packaging/syntax.
- Lua missing: `make test-all`, `make lint`, `make check` are blocked before
  tests/lint run.
- Manifest SDK differs from `pdc`: SDK drift must be recorded.

## Surface Classifier

- `input`: buttons, crank, accelerometer, Menu, Lock, microphone proposals.
- `graphics`: `gfx.*`, draw modes, fonts, images, dither, display refresh.
- `datastore`: save/load/delete, corrupt/legacy/partial saves.
- `sound`: sampleplayer/fileplayer/synth, audio loading, nil safety.
- `lifecycle`: update, sleep, terminate, pause/resume, lock/unlock.
- `simulator`: Simulator launch, screenshots, sampler, stale PDX, device-only.
- `build`: `pdc`, Lua test gate, Makefile, hooks.
- `lua`: Lua version, `<const>`, import, globals, constructors.
- `perf`: FPS, memory, GC, redraw regions, hot allocations.
- `sdk-version`: local SDK vs manifest vs official latest.

## Core Workflow

1. Classify surface and risk tier.
2. Read routed sources.
3. Build evidence map with truth_status.
4. Compare options for R2/R3/R4 or justify single safe path.
5. Act within allowed scope.
6. Run relevant automation pack.
7. Update `AGENTS.md` for new pitfalls.
8. Produce handoff and run record if required.

## Local SDK Verification Pack

```bash
SDK_BIN="$(command -v pdc)"
SDK_ROOT="$(cd "$(dirname "$SDK_BIN")/.." && pwd)"
rg -n "startAccelerometer|readAccelerometer|getCrankPosition|getCrankChange|getSystemMenu" "$SDK_ROOT/CoreLibs"
rg -n "datastore\\.(read|write|delete|writeImage|readImage)" "$SDK_ROOT/CoreLibs"
rg -n "fillPolygon|fillTriangle|drawTextAligned|getTextSize|setImageDrawMode" "$SDK_ROOT/CoreLibs"
rg -n "font:(drawTextAligned|getTextWidth)|inputHandlers|setRefreshRate|setMinimumGCTime|setGCScaling" "$SDK_ROOT/CoreLibs"
```

If variable expansion is impractical, use the concrete SDK path from
`ls -l "$(command -v pdc)"`.

## Platform Static Audit

```bash
rg -n "playdate\\.|gfx\\.|datastore|accelerometer|crank|System Menu|setImageDrawMode|setColor|pdc|Simulator|lua" AGENTS.md docs source scripts Makefile .hermes
rg -n "buttonJustPressed\\([^\\n]*kButtonMenu|kButtonMenu" source scripts docs AGENTS.md
rg -n "gfx\\.fillPolygon\\(|gfx\\.fillTriangle\\(|playdate\\.geometry\\.polygon\\.new" source scripts docs AGENTS.md
rg -n "gfx\\.image\\.new\\(|imagetable\\.new\\(|sampleplayer\\.new\\(|setColor\\(|drawTextAligned\\(|image:draw|:draw\\(" source scripts docs .hermes
rg -n "<const>" source scripts docs AGENTS.md
rg -n "playdate\\.|gfx\\." source/safe.lua source/*_data.lua source/*_catalogs.lua
```

Expected:

- `safe.lua` has no `playdate.*` or `gfx.*`.
- data/catalog files have no Playdate, graphics, persistence, lifecycle, or
  runtime mutation APIs.
- Menu polling is suspicious outside `main.lua` ownership.
- hot asset creation is suspicious.
- `setColor()` before image/text requires draw mode reset.

## Build/Test Gates

Preferred:

```bash
make check
```

If Lua is missing and gate evidence is required:

```bash
make test-all
make check
make build
```

Report:

- `make test-all`/`make check` blocked at `check-lua`: missing runtime, not
  failing tests;
- passing `make build`: PDX syntax/package validated, tests/lint not validated.

## Simulator Gate

Use only when simulator evidence matters:

```bash
make build
pdc build/screenshot_src build/ScreenshotHarness.pdx
```

macOS Simulator crashpad/bootstrap/MachPort errors are simulator sandbox issues,
not PDX failures. Request approval for GUI/unsandboxed launch or ask the user to
run hardware validation.

## Performance Pack

```bash
rg -n "gfx\\.image\\.new\\(|imagetable\\.new\\(|sampleplayer\\.new\\(|string\\.format\\(|table\\.insert\\(|collectgarbage\\(|print\\(" source scripts
rg -n "drawFPS|getFPS|getStats|setMinimumGCTime|setGCScaling|setRefreshRate|sprite\\.update|setBackgroundDrawingCallback" source scripts docs
```

Collect if relevant:

- target refresh rate;
- measured FPS;
- Simulator Sampler notes;
- Memory/Malloc notes;
- hardware validation status;
- redraw region scope;
- per-frame allocations/repeated asset loads.

## Surface Checklists

Input:

- `Input:update()` returns events only.
- crank wrap normalized if absolute position used.
- accelerometer start/read/stop lifecycle correct.
- Menu handled by System Menu.
- project spec supports proposed hardware behavior.

Graphics:

- draw mode restored before image/text.
- images/fonts not loaded in hot paths.
- polygons closed and SDK-safe.
- text through `Fonts` helpers where required.
- dither/flashing considered.

Datastore:

- `pcall` around read/write/delete.
- clean/legacy/partial/corrupt/reset shapes considered.
- save-on-sleep/terminate considered.

Build/Simulator:

- local SDK version recorded.
- missing Lua classified correctly.
- stale PDX ruled out when relevant.
- hardware validation need stated.
