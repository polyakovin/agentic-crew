# Endless Economy Specialist Rubric

Use this rubric before handing off an economy analysis or balance change.

## Excellent

- All relevant specs read (economy-roadmap, lock/clock/safe/shared economy,
  game.md, manifest.json).
- Findings report has file/line references sorted by severity.
- Economy invariants verified — all passing or failures documented with
  specific data.
- Spec updated before data change, or explicit declaration that spec does
  not need updating.
- Data change respects architectural boundaries (data in data files,
  runtime in runtime modules).
- `make test-all`, `make lint`, `make build`, `make check` all pass.
- Save compatibility assessed with clear migration plan if needed.
- New tests added for changed economy behavior or explicit note that
  existing tests cover the change.
- Risks and alternatives documented.
- AGENTS.md updated if new pitfall discovered.
- Prices/rewards follow clear, explainable curves.
- No RNG used as balance crutch.

## Acceptable

- Findings reported but some severity misclassified.
- Parameter changes documented but rationale thin.
- Spec updated after code or rationale not fully explained.
- `make test-all` passes but save compatibility not fully assessed.
- Risks mentioned but alternatives not explored.
- One minor boundary check missed (non-critical).

## Needs Revision

- No findings report with file/line references — "it looks balanced."
- Spec not updated when behaviour changed.
- Data moved from data files to runtime modules.
- Architectural boundary violated (e.g., gfx.* in data files).
- `make check` not run or fails unreported.
- Data change breaks existing tests without test update.
- Save compatibility not assessed on breaking change.
- Economy invariant broken and not documented.
- Broad refactor instead of targeted data change.
- RNG added to economy mechanics under guise of "balance."
- Infinite profit loop introduced.

## Critical Failure

- Renames game to Safe Cracker or hero to Safe Cracker.
- Breaks Adventure/Speedrun/Endless mode lifecycles.
- Adds `gfx.*` / `playdate.*` to endless_data.lua or economy_catalogs.lua.
- Adds persistence or runtime mutations to data modules.
- Adds game logic to endless_save.lua.
- Replaces PNG assets with procedural drawing without request.
- Makes economy mechanics RNG-dependent.
- Commits changes without validation.
- Overwrites unrelated user changes.
- Introduces save-breaking change without migration plan.
- Creates soft lock where player cannot progress.
- Introduces infinite profit exploit.

## Challenge Checklist

- Are all relevant specs read?
- Are economy invariants verified?
- Are findings sorted by severity with file/line refs?
- Is the spec updated before data change?
- Do all architectural boundaries hold?
- Does `make check` pass?
- Are new tests needed?
- Is save compatibility assessed?
- Do prices/rewards follow understandable curves?
- Is this the smallest change that achieves the intent?
- Are there any infinite profit loops?
- Is every progression-gating item obtainable?
- Are risks documented?
- Are new pitfalls added to AGENTS.md?
