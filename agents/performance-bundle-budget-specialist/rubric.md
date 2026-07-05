# Performance Bundle Budget Specialist Rubric

Use this rubric before handing off a performance analysis or recommendation.

## Excellent

- All relevant specs read (renderer.md, soundfx.md, economy-roadmap.md,
  manifest.json).
- AGENTS.md performance/audio/asset pitfalls sections reviewed.
- Hot path inventory complete with file:line references.
- Every finding has measured cost (table alloc count, trig calls/frame,
  asset KB).
- Every recommendation has a specific, verifiable command.
- Residual risk documented for every recommendation.
- PDX size breakdown current vs documented budget.
- Asset audit complete: top image contributors, audio tracks vs budget.
- Cache strategy review verified: dirty flags, per-frame patterns.
- No code mutated without explicit user request.
- No `gfx.*`/`playdate.*` boundary violations in data modules.
- All non-goals respected.
- AGENTS.md Pitfalls update recommended if new pattern found.

## Acceptable

- Findings reported but some severity misclassified.
- Recommendations have verification commands but rationale thin.
- PDX size measured but not broken down by category.
- One or two hot paths not fully traced.
- Asset audit done but audio compression format not verified.
- Residual risks mentioned but not detailed.

## Needs Revision

- No file:line references in findings — "renderer is slow."
- Recommendations are vague: "optimize drawLoop" without specific
  what and how.
- Per-frame arithmetic mistagged as allocation.
- `_ensure*` functions' `gfx.image.new` flagged as per-frame
  (they're cached).
- Candidate pool dirty-flag behavior flagged as per-frame rebuild
  (it's correct).
- Code mutated without explicit user request.
- `make build` or Simulator launched without user request.
- Recommendation requires architecture change beyond performance scope.
- No verification commands provided.
- No residual risk documented.

## Critical Failure

- Renames game to Safe Cracker or hero to Safe Cracker.
- Mutates source code without explicit user request.
- Adds `gfx.*` / `playdate.*` to data modules.
- Deletes or modifies asset files without request.
- Proposes RNG-based "performance optimization."
- Changes audio design (sound selection, mixing, transitions).
- Changes renderer visual composition or layer order.
- Changes market economy balance or prices.
- Commits changes without validation.
- Overwrites unrelated user changes.
- Makes recommendation that would break spec contract.
- Duplicates another specialist's scope.

## Challenge Checklist

- Are all relevant specs read?
- Is the hot path inventory complete with file:line?
- Does every finding have a measured cost?
- Does every recommendation have a verification command?
- Is residual risk documented per recommendation?
- Is PDX size measured and broken down?
- Are audio tracks checked against budget?
- Are top image assets identified by size?
- Is cache strategy verified (dirty flags, per-frame patterns)?
- Are `_ensure*` function allocations correctly identified as cached?
- Are boundary audits run (gfx.* in data modules)?
- Are all non-goals respected?
- Is AGENTS.md Pitfalls update recommended if applicable?
- Has no code been mutated without explicit request?
