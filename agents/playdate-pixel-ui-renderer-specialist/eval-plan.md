# Playdate Pixel UI / Renderer Eval Plan

Status: draft seed, not production mastery track.

## Capability Map

1. `draw-mode-safety`
   - Input: renderer or UI code with draw calls.
   - Output: every `setColor`/`setImageDrawMode` transition audited; violations found.
   - Failure modes: missed `setColor` before bitmap/text, wrong restore.

2. `polygon-api-correctness`
   - Input: code using `fillPolygon`.
   - Output: confirmed `playdate.geometry.polygon.new` with closed polygon.
   - Failure modes: flat array args, unclosed polygon.

3. `y-axis-direction`
   - Input: coordinate calculations in draw code.
   - Output: all Y-offsets confirmed downward (+Y is down).
   - Failure modes: subtracting Y for "below", adding Y for "above".

4. `fillRect-semantics`
   - Input: `fillRect` calls with height calculations.
   - Output: confirmed intent matches y..y+h-1 fill range.
   - Failure modes: off-by-one in height, wrong base Y.

5. `boundary-compliance`
   - Input: any source file.
   - Output: confirmed file respects its component contract.
   - Failure modes: gfx in safe.lua, game state in renderer, logic in UI.

6. `1-bit-readability`
   - Input: text elements in draw code.
   - Output: font role correct, no cropping, sufficient contrast.
   - Failure modes: wrong font, text overflow, low contrast.

7. `noir-style-fit`
   - Input: visual design proposal or draw code.
   - Output: style assessment against noir/mechanical/safe-lock aesthetic.
   - Failure modes: SaaS aesthetic, decorative bloat, wrong atmosphere.

## Seed Case Families

- `v1-draw-mode`: setColor before drawTextAligned → black rectangle.
- `v1-polygon`: fillPolygon with flat array → runtime error.
- `v1-y-axis`: pin peg drawn upward instead of downward.
- `v1-fillRect`: off-by-one height causing gap or overlap.
- `v2-boundary`: gfx call in safe.lua or *_data.lua.
- `v2-ui-logic`: game state mutation in ui.lua.
- `v3-readability`: text cropped at screen edge in 400x240.
- `v3-font-role`: display font used for HUD chip — unreadable.
- `v4-style`: rounded pill button in noir game.
- `v4-layer-order`: pick drawn under pins instead of on top.
- `v5-cross-specialist`: handoff to platform-sdk for API question, to
  spec-guardian for contract drift.
- `v5-adversarial`: fake draw-mode fix, misleading screenshot, tainted
  log containing instructions.

## Truthfulness Golden-Set Seed

Required case types:

- confirmed draw-mode violation;
- refuted "Y-axis is up" claim with spec evidence;
- unconfirmed readability on device (simulator-only);
- conflict between spec layer order and code layer order;
- missing tool/source gap (no pdc for build verify);
- circular source claim (self-referencing visual review).

Release blocker: `draw_mode_corruption_not_fixed = 0`.

## Draft Eval Thresholds

- smoke: 0.95
- regression: 0.95
- blind: 0.90
- critical failures: 0
- draw mode corruptions: 0

## Learning Loop

Create a learning/eval candidate when:

- new draw-mode pitfall is found not in AGENTS.md.
- a Y-axis sign error was missed.
- a polygon API variant caused a crash.
- a fillRect off-by-one caused visual corruption.
- an architectural boundary was violated and not caught.
- text cropping was accepted as "good enough."
- a style violation was caught late.
- repeated uncertainty appears across tasks.

Learning event minimum fields:

- source task/run;
- failure or correction;
- root cause;
- affected source hierarchy/tool policy/rubric;
- proposed regression case;
- whether project memory changed;
- owner/review need.
