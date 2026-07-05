# Persistence / Save Migration Specialist Workflow

## Boot Probe

Run for every persistence diagnostic session:

```bash
git status --short
cd /root/locksmith && make test-all 2>&1 | grep -E "datastore|endless_save|PASS|FAIL" | head -20
cd /root/locksmith && make lint 2>&1 | tail -5
cd /root/locksmith && make build 2>&1 | tail -5
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- `make test-all` must show datastore + endless_save suites passing.
- `make lint` exit 0 = clean static analysis, exit non-zero = issues to note.
- `make build` exit 0 = PDX compiles, exit non-zero = build blocked.

## Core Workflow

1. **Environment preflight**: `make test-all` → check datastore + endless_save
   suite status.
2. **Read source of truth**: shared-economy.md §10 → datastore.lua →
   endless_save.lua → main.lua.
3. **For schema change**: propose spec update → implement nil-safe migration →
   run tests → document risks.
4. **For corrupt save**: reproduce → diagnose root cause (missing pcall, type
   guard, fallback) → propose fix.
5. **For bounded write audit**: `rg` for write calls → classify by frequency →
   flag hot-path writes.
6. **For legacy fallback**: verify chain → assess deprecation readiness →
   propose timeline.
7. **Update health snapshot**: write findings to health-snapshot.md.
8. **Report**: concise migration audit with risk table.

## Migration Audit Template

```markdown
## Migration Audit: [change description]

### Schema Before
| Field | Type | Default | Notes |
|-------|------|---------|-------|
| money | number | 0 | Legacy preserved |
| ... | ... | ... | ... |

### Schema After
| Field | Type | Default | Added? | Nil-Safe? | Notes |
|-------|------|---------|--------|-----------|-------|
| newField | number | 0 | yes | yes — migrateSaveData merge | ... |
| ... | ... | ... | ... | ... | ... |

### Migration Risk Table
| Field | Migrates From | Preserved? | Risk | Rationale |
|-------|--------------|------------|------|-----------|
| money | old save | YES | — | Never reset to 0 |
| newField | defaults | N/A | LOW | New field, defaults to 0 |
| ... | ... | ... | ... | ... |

### Test Evidence
- `make test-all` — [PASS/FAIL]
- `lua scripts/test_datastore.lua` — [PASS/FAIL]
- `lua scripts/test_endless_save.lua` — [PASS/FAIL]
```

## Nil-Safe Migration Checklist

For every new save field:

1. [ ] Added to `defaults()` function with correct initial value.
2. [ ] `migrateSaveData()` handles missing key: `if data[key] == nil then data[key] = cloneValue(defaults[key]) end`.
3. [ ] Wrong-type guard: `if type(data[key]) ~= "table"` → clone default.
4. [ ] Legacy progress values (money, tier keys, stats) are NOT overwritten.
5. [ ] `lifetimeMaxBalance` is set from `money` on first migration if absent.
6. [ ] Corrupt data (non-table) triggers `return EndlessSave.defaults()`.
7. [ ] Partial save (only `{money = 999}`) migrates all missing fields to defaults.
8. [ ] Test case added or updated in `scripts/test_endless_save.lua`.
9. [ ] Spec updated in `docs/shared-economy.md` §10.
10. [ ] Migration risk documented.

## Schema Version Audit

```lua
-- Proposed schema versioning pattern for endless_save.lua
local SAVE_VERSION = 1  -- increment on schema change

local MIGRATIONS = {
    [0] = function(data)  -- legacy → v1
        -- ... existing migrateSaveData logic
        data.saveVersion = 1
        return data
    end,
    -- [1] = function(data) ... end  -- future v1 → v2
}
```

For each version bump:

1. [ ] New `MIGRATIONS[version]` function added.
2. [ ] Forward chain tested: legacy → v1 → v2.
3. [ ] `saveVersion` field added to `defaults()`.
4. [ ] Backward compatibility assessed (older build reading newer save).
5. [ ] Spec updated with version migration path.

## Corrupt Data Diagnosis

```bash
# Reproduce: what exact save data causes the crash?
cd /root/locksmith

# Check pcall coverage in load functions
rg "pcall.*datastore" source/datastore.lua source/endless_save.lua

# Check type guards
rg "type\(data\)" source/datastore.lua source/endless_save.lua

# Run specific test scenarios
lua scripts/test_datastore.lua 2>&1
lua scripts/test_endless_save.lua 2>&1
```

Diagnosis steps:

1. Identify which `pcall` is missing or which type guard is absent.
2. Classify corrupt state: non-table, missing key, wrong-type value, nil value.
3. Verify fallback path triggers: `return defaultData()` or `return EndlessSave.defaults()`.
4. Propose fix: add `pcall`, add type guard, add nil check.
5. Add or update test case.
6. Document pitfall in AGENTS.md.

## Legacy Fallback Review

```bash
cd /root/locksmith

# Check legacy key usage
rg "infinite_economy\|LEGACY_KEY" source/endless_save.lua

# Run legacy fallback test
lua scripts/test_endless_save.lua 2>&1 | grep -i "legacy"
```

Review steps:

1. Verify legacy key is read in `load()` fallback path.
2. Verify `reset()` clears both current and legacy keys.
3. Assess: are there still players on the legacy key? (no telemetry → assume yes)
4. Propose deprecation timeline: keep fallback for N months after all known
   builds migrated.
5. Document deprecation plan in spec.

## Bounded Write Audit

```bash
cd /root/locksmith

# Find all persistence write calls
rg -n "Datastore\.save\|playdate\.datastore\.write\|EndlessSave\.save" source/ --include "*.lua"

# Classify each call site
# Event-bound: gameover, settings toggle, purchase, claim, reset
# Hot-path: draw(), update(dt) — MUST NOT EXIST
# Lifecycle: mode exit, settings close
```

Classify each call site:

| File | Line | Call | Frequency | Risk |
|------|------|------|-----------|------|
| datastore.lua | 77 | Datastore.save | on addScore/markAdventureComplete/setMusic | LOW — event-bound |
| datastore.lua | 87 | Datastore.save | on markAdventureComplete | LOW — event-bound |
| endless_save.lua | 135 | EndlessSave.save | on purchase/claim/cycle/reset | LOW — event-bound |
| main.lua | 130 | EndlessSave.save | on Settings open while in Endless | LOW — lifecycle |
| ... | ... | ... | ... | ... |

## Validation Pack

```bash
# Full validation cycle
cd /root/locksmith

# 1. Run all tests — datastore + endless_save must pass
make test-all 2>&1 | grep -E "datastore|endless_save|PASS" | head -20

# 2. Run individual test suites
lua scripts/test_datastore.lua 2>&1 | tail -5
lua scripts/test_endless_save.lua 2>&1 | tail -5

# 3. Lint
make lint 2>&1 | tail -5

# 4. Build
make build 2>&1 | tail -5

# 5. Full check
make check 2>&1 | tail -5
```

## Pitfalls

1. **Never reset money to 0 during migration.** Money is the player's most
   emotionally invested stat. `STARTING_MONEY = 0` applies to NEW games only.
   Legacy saves preserve their `money` value.

2. **Nil is not the same as "missing."** `migrateSaveData()` must handle both
   `nil` (field absent) and wrong-type values (e.g., `components = "string"`
   instead of a table).

3. **Clone defaults, don't reference them.** `data[key] = defaults[key]` shares
   the same table reference — mutations to the loaded data will corrupt defaults.
   Always use `cloneValue(defaults[key])`.

4. **Test partial saves explicitly.** Test case 7 in `test_endless_save.lua`
   saves only `{money = 999}` and verifies all other fields migrate correctly.
   Any new field must be added to this test assertion.

5. **Legacy keys are not backward forever.** `infinite_economy` was renamed to
   `endless_economy`. The fallback read should have a documented deprecation plan.
   Don't silently remove it without a migration window.

6. **`pcall` is not optional on `playdate.datastore.read/write`.** The
   datastore API can fail (corrupt storage, full storage, permission error).
   Every read/write must be `pcall`-wrapped.

7. **`lifetimeMaxBalance` is tricky.** When migrating a legacy save, if the
   field is absent, set it to `money` (current balance). The player might have
   already passed their peak.

8. **Achievement baselines start from first display.** `achievementBaselines`
   stores the metric value when an achievement first appears in the reward
   window. During migration, it starts empty (`{}`) — old achievements that
   were already completed won't retroactively count.

9. **Seen tabs survive resets?** `seenTabs` tracks which UI tabs the player
   has seen. During `EndlessSave.reset()`, it goes to `{}` — intentional.

10. **Bounded writes = no per-frame saves.** Playdate datastore writes are
    slow. Writing every frame would destroy performance. All save calls must
    be event-bound (purchase, claim, toggle, gameover, reset) or lifecycle
    (mode exit, settings close).

## Commit And Push Gate

This specialist evaluates and modifies persistence code. It is allowed to
commit and push scoped changes as per the `Must Push Rule` in AGENTS.md.

Before committing:

- `make check` (or honest blocked-gate report).
- Git diff reviewed: only intended files changed.
- No dirty unrelated worktree files bundled.
- Migration risk documented.
