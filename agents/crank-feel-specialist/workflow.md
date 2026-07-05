# Crank Feel / Micro-Mechanics Specialist Workflow

## Boot Probe

Run for non-trivial feel analysis or parameter tuning:

```bash
git status --short
cd /root/locksmith && make test-all 2>&1 | tail -5
cd /root/locksmith && make lint 2>&1 | tail -5
cd /root/locksmith && make build 2>&1 | tail -5
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Failing `make test-all` before changes means fix first, then tune feel.
- Missing `pdc` means blocked build gate — cannot verify rendering impacts.

## Core Workflow

1. **Read project context**: AGENTS.md → `docs/manifest.json` → relevant specs
   (`docs/input.md`, `docs/safe.md`, `docs/game.md`, `docs/renderer.md`).
2. **Formulate UX intent**: one sentence describing what changes in player feel.
3. **Identify parameters**: which constants, curves, mappings, tolerances, or
   timing values to change.
4. **Baseline measurement**: record old values, run `make test-all` to confirm
   current state.
5. **Update spec first**: if the change affects API, contract, or documented
   behaviour, update the relevant `docs/*.md` or `docs/manifest.json`.
6. **Implement**: make the smallest code change respecting architectural
   boundaries.
7. **Verify**: `make test-all` → `make lint` → `make build` → `make check`.
8. **Report**: write concise report with UX intent, parameter changes,
   build evidence, risks, and manual verification steps.

## Parameter Change Template

When proposing or making a feel change, use this template:

```
UX Intent: [one line]

Parameters:
  [parameter_name]:
    old: [old_value]
    new: [new_value]
    rationale: [why this change improves feel]
    file: [source_file]
    boundary: [safe.lua / input.lua / *_data.lua / renderer.lua]

Specs updated:
  [spec_file]: [description of change]

Code files touched:
  [source_file]: [description of change]

Tests added/updated:
  [test_file]: [description]

Manual verification:
  1. [step to verify on Simulator/Playdate]

Risks:
  - [risk description]

Alternatives:
  - [alternative approach if applicable]
```

## Feel Parameter Categories

### Crank-to-Pick Mapping

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| Sin mapping | crank_angle → pick_height via sin() | input.lua → safe.lua |
| Linearity | mapping curve shape | safe.lua |
| Dead zone | min crank angle before pick moves | input.lua constant |
| Acceleration | sensitivity multiplier at higher crank speeds | input.lua |
| Deceleration | sensitivity reduction at lower crank speeds | input.lua |
| Inertia | continued motion after crank release | input.lua |
| Smoothing | anti-aliasing of crank delta | input.lua |
| Max pick height | max height of pick from sin(crank_angle) | safe.lua constant |

### Pin Micro-Mechanics

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| Pin resistance | visual/audio feedback intensity per pin type | safe.lua + renderer.lua + soundfx.lua |
| Pin bounce | settle animation duration after miss | safe.lua |
| Pin settle timing | frames to settle after tryCatch | safe.lua constant |
| Pin hardness | resistance scaling per difficulty | *_data.lua |
| Friction simulation | visual wobble + soundfx timing during lift | renderer.lua + soundfx.lua |

### Snap-to-Target

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| Snap radius | distance from target to trigger snap | safe.lua constant |
| Snap speed | frames to snap into place | safe.lua constant |
| Snap easing | linear / ease-in / ease-out curve | safe.lua |

### MISS Feedback

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| Direction hint | "too high" vs "too low" detection | safe.lua |
| Direction rendering | visual indicator of direction | renderer.lua |
| Timing sync | soundfx delay to match visual | soundfx.lua + renderer.lua |
| MISS tolerance | distance from target to qualify as "close miss" | safe.lua constant |

### Tolerance Scaling

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| Base tolerance | tolerance at difficulty=0 | *_data.lua |
| Tolerance curve | how tolerance shrinks with difficulty | *_data.lua or safe.lua |
| Phase-specific tolerance | Adventure phase 1–4 tolerance values | adventure_data or game.md |
| Endless tier tolerance | Endless tier progression | endless_data.lua |

### D-Pad Micro-Adjustments

| Sub-category | Parameters | Location |
|-------------|------------|----------|
| D-pad delta | how much pick moves per D-pad press | input.lua |
| D-pad smoothing | interpolation between D-pad steps | input.lua |
| D-pad repeat rate | hold-to-repeat speed | input.lua |

## Architecture Boundary Audit

Before any code change, verify:

```bash
# Check safe.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." source/safe.lua && echo "VIOLATION: safe.lua has gfx/playdate" || echo "OK"

# Check renderer.lua for state mutations (should only draw)
rg -n "safe\.\|input\." source/renderer.lua && echo "VIOLATION: renderer.lua refs safe/input" || echo "OK"

# Check input.lua for game logic
rg -n "safe\.\|renderer\.\|ui\." source/input.lua && echo "VIOLATION: input.lua refs safe/renderer/ui" || echo "OK"
```

## Validation Pack

Run from Locksmith root:

```bash
make test-all && echo "Tests: PASS" || echo "Tests: FAIL"
make lint && echo "Lint: PASS" || echo "Lint: FAIL"
make build && echo "Build: PASS" || echo "Build: FAIL"
# Full cycle:
make check && echo "Check: PASS" || echo "Check: FAIL"
```

If tests fail after a parameter change:
1. Check that parameter values are within expected ranges.
2. Ensure declarative data tables are syntactically correct.
3. Update test assertions if the parameter change is intentional.
4. Never change tests to "make them pass" — tests validate contracts.

## Feedback Timing Sync Checklist

When adjusting MISS/CATCH/HIT feedback timing:

1. Identify the audio trigger point in `source/soundfx.lua`.
2. Identify the visual trigger point in `source/renderer.lua`.
3. Measure frames between audio and visual trigger.
4. Adjust one to match the other.
5. Verify on Simulator with `make build` → run.
6. Document any discovered timing pitfall in AGENTS.md.

## Pitfalls

1. **Smoothing = input lag**: every frame of smoothing is one frame of input lag.
   At 20 fps, even 2 frames is 100 ms — noticeable. Smooth with caution.

2. **Crank wrap-around**: `playdate.getCrankChange()` wraps at 360°. Test the
   359° → 0° transition explicitly.

3. **Inertia vs responsiveness**: inertia helps fluidity but makes precise
   stops harder. Tune per difficulty — less inertia on hard locks.

4. **Snap radius too large**: if the snap radius is wider than tolerance,
   the player never needs precision. Snap should assist, not replace.

5. **Missing directional feedback**: a MISS that just says "MISS" without
   direction is a wasted learning opportunity.

6. **Silent parameter changes**: changing a safe.lua constant without updating
   the spec means the spec decays. Always spec-first.

7. **Crank speed vs screen refresh**: the crank generates events at a higher
   rate than the 20 fps screen. Accumulate crank deltas; don't sample per-frame.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped feel-specialist changes.
3. Commit with atomic message: `"feat(crank-feel): <UX intent>"`
4. Push the current branch.
5. Record commit SHA and push result.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- credentials or branch policy prevent push.
