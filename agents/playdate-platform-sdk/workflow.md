# Playdate Platform / SDK Workflow

## Boot Probe

Run for platform-sensitive work unless explicitly forbidden:

```bash
git status --short
command -v pdc
pdc --version
ls -l "$(command -v pdc)"
python3 -c 'import os,sys; p=sys.argv[1]; print(os.path.realpath(p))' "$(command -v pdc)"
command -v lua
command -v lua5.4
command -v lua5.3
command -v luajit
```

Interpretation:

- `pdc` present: PDX packaging/syntax can be validated.
- Record both the shell path and resolved path for `pdc`; symlinked installs
  must derive the SDK root from the resolved binary path.
- Lua missing: unit/lint/check gates may be blocked before tests/lint run.
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
2. Read routed sources from `taskBrief.whatToRead`.
3. Build evidence map with truth status.
4. Compare options for R2/R3/R4 or justify a single safe path.
5. Act within allowed scope.
6. Run relevant automation pack.
7. Update project memory for new pitfalls.
8. Produce A2A handoff and run record when required.

## Local SDK Verification Pack

```bash
SDK_BIN="$(command -v pdc)"
SDK_BIN_RESOLVED="$(python3 -c 'import os,sys; print(os.path.realpath(sys.argv[1]))' "$SDK_BIN")"
SDK_ROOT="$(cd "$(dirname "$SDK_BIN_RESOLVED")/.." && pwd)"
CORELIBS_DIR="$SDK_ROOT/CoreLibs"
printf 'pdc=%s\npdc_resolved=%s\nsdk_root=%s\ncorelibs=%s\n' \
  "$SDK_BIN" "$SDK_BIN_RESOLVED" "$SDK_ROOT" "$CORELIBS_DIR"
test -d "$CORELIBS_DIR" || {
  printf 'missing CoreLibs search scope: %s\n' "$CORELIBS_DIR" >&2
  exit 2
}
rg -n "startAccelerometer|readAccelerometer|getCrankPosition|getCrankChange|getSystemMenu" "$CORELIBS_DIR"
rg -n "datastore\\.(read|write|delete|writeImage|readImage)" "$CORELIBS_DIR"
rg -n "fillPolygon|fillTriangle|drawTextAligned|getTextSize|setImageDrawMode" "$CORELIBS_DIR"
rg -n "font:(drawTextAligned|getTextWidth)|inputHandlers|setRefreshRate|setMinimumGCTime|setGCScaling" "$CORELIBS_DIR"
```

Record `pdc`, `pdc_resolved`, `sdk_root`, and `corelibs` in the evidence map.
If `pdc` cannot be resolved or `CoreLibs` is absent under the resolved SDK root,
classify local SDK grounding as a `missing-source` blocker with the exact
CoreLibs search scope. Do not silently downgrade API claims to unconfirmed
without naming the missing source.

## Platform Static Audit

```bash
rg -n "playdate\\.|gfx\\.|datastore|accelerometer|crank|System Menu|setImageDrawMode|setColor|pdc|Simulator|lua" AGENTS.md docs source scripts Makefile
rg -n "buttonJustPressed\\([^\\n]*kButtonMenu|kButtonMenu" source scripts docs AGENTS.md
rg -n "gfx\\.fillPolygon\\(|gfx\\.fillTriangle\\(|playdate\\.geometry\\.polygon\\.new" source scripts docs AGENTS.md
rg -n "gfx\\.image\\.new\\(|imagetable\\.new\\(|sampleplayer\\.new\\(|setColor\\(|drawTextAligned\\(|image:draw|:draw\\(" source scripts docs
rg -n "<const>" source scripts docs AGENTS.md
rg -n "playdate\\.|gfx\\." source/*_data.lua source/*_catalogs.lua
```

Expected:

- pure logic files have no `playdate.*` or `gfx.*`;
- data/catalog files have no Playdate, graphics, persistence, lifecycle, or
  runtime mutation APIs;
- Menu polling is suspicious outside main/system-menu ownership;
- hot asset creation is suspicious;
- `setColor()` before image/text requires draw-mode reset.

## Build/Test Gates

Preferred project gate:

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

- `make test-all`/`make check` blocked at Lua/runtime check: missing runtime,
  not failing tests;
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

- input module returns events only;
- crank wrap normalized if absolute position is used;
- accelerometer start/read/stop lifecycle is correct;
- Menu handled by System Menu, not as frequent gameplay input;
- project spec supports proposed hardware behavior.

Graphics:

- draw mode restored before image/text;
- images/fonts not loaded in hot paths;
- polygons closed and SDK-safe;
- text uses project font/UI helpers where required;
- dither/flashing considered for 1-bit display.

Datastore:

- `pcall` around read/write/delete;
- clean/legacy/partial/corrupt/reset save shapes considered;
- save-on-sleep/terminate considered when relevant.

Build/Simulator:

- local SDK version recorded;
- missing Lua classified correctly;
- stale PDX ruled out when relevant;
- hardware validation need stated.
