# Persistence / Save Migration Specialist Rubric

Use this rubric before handing off a migration audit, corrupt save diagnosis,
or schema change report.

## Excellent

- Spec read and updated before schema change (shared-economy.md §10).
- Every new save field is nil-safe: defaults table, migrateSaveData merge, type guard.
- Corrupt data scenarios covered: non-table load, partial save, legacy key fallback,
  missing keys, wrong-type values.
- Legacy progress preserved: money NEVER reset to 0 during migration.
- Bounded write audit complete: all write call sites classified as event-bound,
  lifecycle, or hot-path — no per-frame writes.
- Legacy key fallback chain tested and documented with deprecation timeline.
- Schema versioning proposed with migration path and backward compatibility assessment.
- `make test-all` datastore + endless_save suites pass.
- Migration risk table present: per-field what migrates, what resets, rationale.
- New persistence pitfalls documented in AGENTS.md.
- No game logic or UI rendering in persistence modules.
- No `gfx.*` or non-datastore `playdate.*` calls in persistence modules.
- Evidence based on actual command output with exit codes.

## Acceptable

- Spec updated but migration risk table thin (missing some fields).
- Nil-safe migration implemented but one edge case not explicitly tested.
- Corrupt data fallback works but not all scenarios verified.
- Bounded write audit present but classification missing for some call sites.
- Legacy key fallback status checked but deprecation timeline not proposed.
- `make test-all` passes but individual suite evidence not collected.
- Migration risks documented but residual risks not stated.
- AGENTS.md pitfalls not yet updated for new failure pattern.

## Needs Revision

- Schema change made without spec update.
- New save field added without nil-safe migration — crashes on legacy save.
- Money reset to 0 during migration of existing saves.
- Corrupt data not handled — non-table load would crash without fallback.
- Per-frame save write introduced without review.
- Legacy key fallback removed without migration window or deprecation plan.
- `make test-all` not run after schema change.
- `make test-all` datastore or endless_save suite fails but reported as passing.
- Migration risk not documented at all.
- Game logic or UI rendering added to persistence modules.
- Evidence based on speculation, not command output.

## Critical Failure

- Renames game to Safe Cracker.
- Modifies non-persistence game code (safe.lua, renderer.lua, etc.) without
  explicit authorization.
- Destroys player save data through incorrect migration.
- Resets money to 0 for existing saves during migration.
- Removes `pcall` wrappers from datastore read/write calls.
- Removes type guards or nil checks from load path.
- Adds `gfx.*` or game state mutations to persistence modules.
- Introduces per-frame datastore writes in draw/update hot paths.
- Fabricates test results or evidence.
- Handles secrets, credentials, or API keys.
- Makes irreversible schema changes without migration path.

## Challenge Checklist

- Is every new save field nil-safe? (defaults + migrateSaveData merge + type guard)?
- Is legacy money preserved through migration? NEVER reset to 0?
- Does corrupt data (non-table, partial, wrong-type) fall back to defaults?
- Is legacy `infinite_economy` key still readable as fallback?
- Are persistence writes event-bound, not per-frame?
- Is schema versioning documented with migration path?
- Is migration risk documented per-field?
- Do `make test-all` datastore + endless_save suites pass?
- Is spec (shared-economy.md §10) updated before code?
- Are new pitfalls in AGENTS.md?
- Is all evidence from actual command output?
- Is no game logic or UI rendering in persistence modules?
- Are pcall wrappers present on all datastore read/write calls?
- Are legacy key fallbacks tested?
