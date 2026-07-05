# Persistence / Save Migration Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly designs and audits nil-safe migration,
diagnoses corrupt/partial save issues, maintains legacy key fallbacks, ensures
bounded writes, and documents schema versioning — without destroying player
progress or introducing game logic into persistence modules.

## Metrics

- nil-safe migration coverage (defaults + merge + type guard per field);
- legacy progress preservation (money, tier values, stats);
- corrupt data fallback correctness;
- legacy key fallback chain completeness;
- bounded write classification accuracy;
- schema versioning documentation;
- spec-before-code adherence;
- `make test-all` datastore + endless_save suite pass rate;
- migration risk documentation completeness;
- no-game-logic-in-persistence rule adherence.

## Seed Cases

### PM-001: Nil-Safe Migration Audit — All Fields

Input: "Audit the current save schema — are all 50+ Endless save fields nil-safe?"

Expected:

- Reads `endless_save.lua` `defaults()` and `migrateSaveData()`.
- Produces per-field nil-safety table: field name, type, default, nil-safe? evidence.
- Identifies any gaps: fields without default, fields without merge logic,
  fields without type guard.
- Classifies risk per gap.
- Recommends fixes for any nil-unsafe fields.
- Runs `make test-all` to verify current state.

### PM-002: Legacy Progress Preservation

Input: "A player has a save from 3 versions ago with only money=500,
lifetimeEarnings=2000, cyclesCompleted=3. What happens when they load
with the current code?"

Expected:

- Traces through `EndlessSave.load()` → `migrateSaveData()`.
- Lists exactly which fields are preserved (money, lifetimeEarnings, cyclesCompleted).
- Lists which fields get default values (all 50+ tier keys, stats, achievements).
- Verifies money is NOT reset to 0.
- Verifies lifetimeMaxBalance is set from money (first migration).
- Runs test_endless_save.lua test case 7 (partial save) as validation.
- Documents residual risk: "Player loses achievementBaselines progress
  (expected — baselines start when achievement first appears)."

### PM-003: Corrupt Save — Non-Table Data

Input: "The playdate.datastore.read returns a string instead of a table.
What happens?"

Expected:

- Traces through `EndlessSave.load()`: `pcall` succeeds but `type(data) ~= "table"` branch.
- Verifies fallback: `return EndlessSave.defaults()`.
- Does the same for `Datastore.load()`: `not ok or not data or type(data) ~= "table"` → `return defaultData()`.
- Flags any missing type guard.
- Verifies no crash, clean fallback.
- Recommends test case if missing.

### PM-004: Legacy Key Fallback

Input: "Verify that loading a save from the old 'infinite_economy' key still works."

Expected:

- Reads `endless_save.lua` lines 123-126: `playdate.datastore.read(LEGACY_KEY)`.
- Confirms `LEGACY_KEY = "infinite_economy"`.
- Runs test_endless_save.lua test case 3 (legacy infinite save fallback).
- Verifies migration applies to legacy data: `migrateSaveData(legacyData)`.
- Assesses: is the fallback chain complete? (primary → legacy → defaults).
- Proposes deprecation timeline with rationale.

### PM-005: Bounded Write Audit

Input: "Are there any datastore writes happening every frame?"

Expected:

- Runs `rg -n "Datastore\.save\|playdate\.datastore\.write\|EndlessSave\.save" source/`.
- Classifies each call site:
  - Event-bound: addScore, markAdventureComplete, setMusic/setDevMode, purchase,
    claim, cycle, reset.
  - Lifecycle: Settings open → save Endless, mode exit.
  - Hot-path: NONE FOUND (expected).
- Produces classification table: file, line, call, frequency, risk.
- Flags if any write in draw() or per-frame update().
- Recommends pitfall documentation if new pattern found.

### PM-006: Schema Versioning Proposal

Input: "Propose a save format version marker for the current Endless save schema."

Expected:

- Proposes `saveVersion` field in `defaults()` with value 1.
- Proposes `MIGRATIONS` table with version-keyed functions.
- Shows migration chain: legacy → v1 (current migrateSaveData logic).
- Discusses backward compatibility: older build reading v1 save (ignores unknown
  keys — Lua table tolerance).
- Updates spec: shared-economy.md §10.
- Updates test_endless_save.lua with version field assertions.
- Documents risk: "If app is downgraded, v1 fields persist in datastore but
  won't be read by older code — safe, just ignored."

### PM-007: New Field Addition

Input: "Add a 'lastPlayedTimestamp' field to the Endless save. Propose the
migration plan."

Expected:

- Updates spec first: shared-economy.md §10.
- Adds field to `defaults()`: `lastPlayedTimestamp = 0`.
- Updates `migrateSaveData()`: nil-safe merge.
- Updates test_endless_save.lua: add assertion for new default.
- Runs `make test-all` → all suites pass.
- Documents migration risk: LOW — new field, defaults to 0.
- Updates health-snapshot.md.

### PM-008: Cross-Module Save Contract

Input: "main.lua calls EndlessSave.save() on Settings open. Verify this is
safe and consistent with the schema contract."

Expected:

- Reads `main.lua` lines 124-132: `savePausedEndlessMode()`.
- Verifies: `mode:flushMoney()` before save — ensures in-memory money is synced.
- Checks: `EndlessSave.save(mode.saveData)` — passes the full data table.
- Verifies: called only when `globalState == GLOBAL_STATE.endless` and mode exists.
- Classifies: lifecycle write, not hot-path. Safe.
- Checks for other save call sites in main.lua: none found beyond Settings/reset.
- Documents any gaps found (e.g., save not called on device sleep).

### PM-009: Datastore Reset Coverage

Input: "Verify that EndlessSave.reset() properly clears both current and
legacy save keys."

Expected:

- Reads `endless_save.lua` `reset()` function (lines 155-162).
- Verifies: `playdate.datastore.write(defaults, KEY)` — writes defaults to `endless_economy`.
- Verifies: `playdate.datastore.delete(LEGACY_KEY)` — removes `infinite_economy`.
- Verifies: pcall wrappers on both write and delete.
- Runs test_endless_save.lua test case 9 (reset Endless progress).
- Confirms test writes to legacy key before reset and verifies it's cleared.

### PM-010: No Game Logic in Persistence

Input: Review datastore.lua and endless_save.lua for any game logic or UI
rendering violations.

Expected:

- Reads both files fully.
- Verifies: no `gfx.*` calls.
- Verifies: no UI rendering or screen management.
- Verifies: no game state transitions (only data read/write).
- Verifies: no imports of game modules (safe, renderer, ui, mode modules).
- Verifies: only `playdate.datastore.*` and `playdate.getTime()` calls.
- Any violation is flagged as critical finding.
- Confirms architectural boundary: persistence modules are pure data I/O.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real migration audit completed on Locksmith save schema.
- Corrupt save fallback validated with actual datastore error scenarios.
- Legacy key fallback validated.
- Bounded write audit completed and all write sites classified.
- No game logic violations in persistence modules in any run.

Move to `production` only after:

- Pilot evidence from 3+ migration or diagnosis cycles.
- Schema versioning proposal implemented and tested.
- Nil-safe migration validated against real legacy saves.
- New persistence pitfalls documented in AGENTS.md.
- No data loss incidents in any run.
