# Crank Feel / Micro-Mechanics Specialist Rubric

Use this rubric before handing off a feel analysis or parameter change.

## Excellent

- UX intent is clearly stated in one line.
- All parameter changes documented: old → new, rationale, file, boundary.
- Spec updated before code change, or explicit declaration that spec does not
  need updating.
- Code change respects all architectural boundaries (safe.lua pure logic,
  renderer.lua pure drawing, input.lua capture only).
- `make test-all`, `make lint`, `make build`, `make check` all pass.
- New tests added for changed feel parameters or explicit note that existing
  tests cover the change.
- Manual verification steps provided for Simulator/Playdate.
- Risks and alternatives documented.
- AGENTS.md updated if new pitfall discovered.
- Feedback timing checked (sound ↔ visual sync).
- Crank wrap-around (359° → 0°) considered if relevant.
- Small incremental change, not rewritten stack.

## Acceptable

- UX intent stated but vague.
- Parameter changes documented but rationale thin.
- Spec updated after code or rationale not fully explained.
- `make test-all` passes but no manual verification steps.
- Risks mentioned but alternatives not explored.
- One minor boundary check missed (non-critical).

## Needs Revision

- No UX intent stated — "changed X from 0.5 to 0.3" with no why.
- Spec not updated when behaviour changed.
- Architectural boundary violated (e.g., gfx.* in safe.lua).
- `make check` not run or fails unreported.
- Parameter change breaks existing tests without test update.
- No manual verification guidance.
- Broad refactor instead of targeted parameter change.
- RNG added to mechanics under guise of "feel improvement."
- Feedback timing not checked.

## Critical Failure

- Renames game to Safe Cracker.
- Breaks Adventure/Speedrun/Endless mode lifecycle.
- Adds `gfx.*` / `playdate.*` to safe.lua.
- Adds game state mutations to renderer.lua.
- Adds game logic to input.lua.
- Replaces PNG assets with procedural drawing without request.
- Makes mechanics RNG-dependent.
- Commits changes without validation.
- Overwrites unrelated user changes.
- Claims "feel improvement" but adds input lag or removes precision.

## Challenge Checklist

- What is the UX intent in one line?
- Which parameter(s) changed, and why?
- Is the spec updated before code?
- Do all architectural boundaries hold?
- Does `make check` pass?
- Are new tests needed?
- Is feedback timing synchronous?
- Is crank wrap-around handled?
- Is this the smallest change that achieves the intent?
- Would a player notice the difference?
- Is precision preserved or improved?
- Is input lag introduced?
- Are risks documented?
- Are manual verification steps clear?
