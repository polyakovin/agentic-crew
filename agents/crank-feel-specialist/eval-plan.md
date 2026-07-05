# Crank Feel / Micro-Mechanics Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly analyzes, proposes, and implements
crank feel and micro-mechanics changes while respecting architectural
boundaries, spec-driven development, and precision-based skill design.

## Metrics

- task success;
- UX intent clarity;
- parameter change correctness;
- boundary respect (safe/input/renderer/data separation);
- spec update completeness;
- `make check` pass rate;
- feedback timing sync;
- manual verification adequacy;
- risk documentation;
- RNG avoidance.

## Seed Cases

### CF-001: Crank Responsiveness Tuning

Input: "Crank feels too sluggish — improve pick responsiveness at low crank speeds."

Expected:

- Reads AGENTS.md → `docs/manifest.json` → `docs/input.md` → `docs/safe.md`.
- Formulates UX intent: "reduce dead zone and increase sensitivity at low crank speeds."
- Identifies parameters: dead zone constant, acceleration curve, base sensitivity.
- Updates spec (`docs/input.md` or `docs/safe.md`) if parameters change.
- Modifies `source/input.lua` (crank capture) or `source/safe.lua` (mapping constants).
- Does NOT touch renderer.lua, ui.lua, or game mode logic.
- `make check` passes.
- Manual verification: "rotate crank slowly — pick should move with less initial resistance."

### CF-002: MISS Directional Feedback

Input: "MISS feedback doesn't tell me if I was too high or too low — add directional hint."

Expected:

- Reads AGENTS.md → `docs/manifest.json` → `docs/safe.md` → `docs/renderer.md`.
- Proposes directional MISS: track missed-pin height vs target, report direction.
- Updates `docs/safe.md` with MISS direction field in tryCatch result.
- Updates `docs/renderer.md` with MISS direction rendering spec.
- Modifies `source/safe.lua` to compute direction (too high / too low).
- Modifies `source/renderer.lua` to draw directional indicator.
- Modifies `source/soundfx.lua` if different sounds for high/low miss.
- `make check` passes.
- Manual verification: "deliberately aim above pin, then below — feedback differs."

### CF-003: Difficulty Feel Progression

Input: "First lock in Adventure feels no different from late Endless locks — add feel progression."

Expected:

- Reads AGENTS.md → `docs/game.md` → `docs/safe.md`.
- Analyzes tolerance values across Adventure phases and Endless tiers.
- Proposes feel progression: wider tolerance + more snap assist at low difficulty;
  tighter tolerance + less snap at high difficulty.
- Updates `docs/game.md` with feel progression table.
- Updates tolerance values in `source/*_data.lua` or relevant constants in `source/safe.lua`.
- Ensures progression is precision-based, not RNG.
- `make check` passes.
- Manual verification: "play Adventure phase 1 lock, then Endless tier-5 lock — feel difference."

### CF-004: D-Pad Smoothing

Input: "D-pad micro-adjustments feel jerky — smooth the D-pad delta interpolation."

Expected:

- Reads `docs/input.md` → `source/input.lua`.
- Proposes D-pad delta interpolation: lerp between steps instead of discrete jumps.
- Updates `docs/input.md` with smoothing parameters.
- Modifies `source/input.lua` D-pad handling.
- Ensures smoothing does NOT add perceptible input lag.
- `make check` passes.

### CF-005: Snap Radius Tuning

Input: "Snap-to-target feels magnetic — reduce snap radius."

Expected:

- Reads `docs/safe.md` snap-to-target section.
- Identifies snap radius constant.
- Proposes smaller value with rationale.
- Updates `docs/safe.md` with new value.
- Modifies `source/safe.lua`.
- Ensures snap radius < tolerance to preserve precision.
- `make check` passes.

### CF-006: Feedback Timing Sync

Input: "Miss sound plays before visual — sync them."

Expected:

- Reads `source/soundfx.lua` and `source/renderer.lua`.
- Identifies audio trigger vs visual trigger timing.
- Adjusts one to match the other.
- Tests on Simulator to verify sync.
- Documents pitfall in AGENTS.md if timing issue was a platform quirk.

### CF-007: Boundary Violation Prevention

Input: "Improve pin friction feel" — agent proposes adding `gfx.setColor()` in safe.lua.

Expected:

- Agent catches the boundary violation.
- Refuses to add `gfx.*` to safe.lua.
- Proposes alternative: visual friction shows via renderer.lua reading safe state.
- Explains boundary rule to user.

### CF-008: Parameter Range Validation

Input: User asks to set tolerance = 0 (impossible precision).

Expected:

- Agent warns: tolerance = 0 makes locks practically impossible.
- Proposes reasonable minimum (e.g., 0.01 or 0.02 at max difficulty).
- Explains skill ceiling through precision, not impossibility.

### CF-009: RNG Avoidance

Input: "Make crank sometimes 'slip' randomly for realism."

Expected:

- Agent refuses: RNG undermines skill ceiling.
- Proposes alternatives: intentional resistance via pin type, difficulty-based friction.
- Explains that feel should be learnable, not random.

### CF-010: Spec-First Workflow

Input: User asks to change a parameter without mentioning spec.

Expected:

- Agent reads spec first.
- Identifies the parameter in the spec.
- Proposes spec update before code change.
- Explains SDD workflow to user if they resist.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real feel change is implemented, tested, and deployed.
- No boundary violations in any run.

Move to `production` only after:

- Pilot evidence from 3+ feel changes.
- Spec updates are consistent and complete.
- Pitfalls are documented in AGENTS.md.
- No RNG regressions.
