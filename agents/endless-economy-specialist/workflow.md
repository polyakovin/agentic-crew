# Endless Economy Specialist Workflow

## Boot Probe

Run for non-trivial economy analysis or balance change:

```bash
git status --short
cd /root/locksmith && make test-all 2>&1 | tail -5
cd /root/locksmith && make lint 2>&1 | tail -5
cd /root/locksmith && make build 2>&1 | tail -5
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Failing `make test-all` before changes means fix first, then balance.
- Missing `pdc` means blocked build gate — cannot verify impacts.

## Core Workflow

1. **Read project context**: AGENTS.md → `docs/manifest.json` → relevant
   specs (`docs/game.md`, `docs/economy-roadmap.md`, lock/clock/safe/shared
   economy specs).
2. **Read data files**: `source/endless_data.lua`, `source/economy_catalogs.lua`.
3. **Read runtime context**: `source/endless_mode.lua`, `source/endless_save.lua`
   (read-only, for understanding how data is consumed).
4. **Run economy analysis** (if review requested): check all invariants,
   scan for exploits, soft locks, impossible recipes, unbalanced pricing.
5. **Formulate findings**: file/line references, severity (Critical/High/
   Medium/Low), proposed fix.
6. **For balance changes**: update spec first if API/contract/behaviour
   changes → implement data change → verify invariants → `make check`.
7. **Report**: findings first, then recommendations, then patch plan,
   then save compatibility risks.

## Economy Analysis Template

```markdown
## Economy Analysis Summary

**Scope**: [what was analyzed]

## Findings

### Critical
- [ECO-XXX] Finding description (file:line)
  - Impact: [what breaks]
  - Fix: [proposed fix]

### High
- [ECO-XXX] ...

### Medium
- [ECO-XXX] ...

### Low
- [ECO-XXX] ...

## Invariant Status

| Invariant | Status | Note |
|-----------|--------|------|
| No impossible recipes | PASS/FAIL | ... |
| Items have sources | PASS/FAIL | ... |
| No infinite profit | PASS/FAIL | ... |
| Starting economy viable | PASS/FAIL | ... |
| Late-game multi-path | PASS/FAIL | ... |
| Prices follow curve | PASS/FAIL | ... |
| Save compatible | PASS/FAIL | ... |

## Recommendations
1. ...
2. ...
```

## Economy Invariant Checklist

Verify before and after every data change:

1. **No impossible recipes**: every CRAFT_RECIPES entry has ingredients that
   exist in COMPONENT_ITEMS or SHOP_ITEMS. No recipe requires items the
   player cannot obtain.
2. **Items have sources (if needed for progression)**: every component needed
   for a recipe that gates progression must have a drop source or shop
   availability at the appropriate rank.
3. **No infinite profit without limit**: for every market {buy → craft → sell}
   loop, verify that sell price ≤ buy price of ingredients × safety factor.
4. **Starting economy doesn't block**: with starting gold and Rank 1
   availability, the player can purchase at least one lock/clock/safe and
   make a profit (after a reasonable number of attempts).
5. **Late-game doesn't collapse**: at max rank, there are at least 3 viable,
   comparably profitable strategies. No single item or recipe dominates.
6. **Prices/rewards have understandable curves**: pricing and reward formulas
   follow monotonic, explainable functions. No arbitrary spikes.
7. **Save compatibility**: adding/removing/reordering tables that save
   references (SHOP_ITEMS arrays, COMPONENT_ITEMS count) requires migration
   in endless_save.lua.

## Balance Patch Template

```markdown
## Balance Patch: [patch name]

**UX Intent**: [one line — what changes for the player]

### Spec Changes
- [spec file]: [description of change]

### Data Changes
[parameter_name]:
  old: [old_value]
  new: [new_value]
  rationale: [why this improves balance]
  file: [source_file]
  boundary: [endless_data.lua / economy_catalogs.lua]

### Invariant Re-verification
| Invariant | Before | After |
|-----------|--------|-------|
| ... | PASS | PASS |

### Save Compatibility
- [x] No migration needed / [ ] Migration required: [plan]
```

## Save Compatibility Audit

When changing data tables that affect save state:

1. **Check save references**: does `endless_save.lua` reference table indices
   (e.g., `inventory[itemId]`)? If items are added in the middle of arrays,
   indices shift — migration needed.
2. **Check save defaults**: does `endless_save.lua` have default values that
   depend on table sizes or constants? Update defaults.
3. **Check migration path**: if migration is needed, update
   `endless_save.lua` migration function with version bump.
4. **Test save/load cycle**: verify that a save from the previous version
   loads correctly after migration.

## Validation Pack

Run from Locksmith root:

```bash
make test-all && echo "Tests: PASS" || echo "Tests: FAIL"
make lint && echo "Lint: PASS" || echo "Lint: FAIL"
make build && echo "Build: PASS" || echo "Build: FAIL"
# Full cycle:
make check && echo "Check: PASS" || echo "Check: FAIL"
```

If tests fail after a data change:
1. Check that table entries are syntactically correct Lua.
2. Ensure table shapes (field names, types) match what runtime expects.
3. Update test assertions if the data change is intentional.
4. Never change tests to "make them pass" — tests validate contracts.

## Architecture Boundary Audit

Before any data change, verify:

```bash
# Check endless_data.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." source/endless_data.lua && echo "VIOLATION" || echo "OK"

# Check economy_catalogs.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." source/economy_catalogs.lua && echo "VIOLATION" || echo "OK"

# Check endless_save.lua for game logic (should only persist)
rg -n "function.*[Uu]pdate\|function.*[Dd]raw\|function.*[Tt]ick" source/endless_save.lua && echo "VIOLATION: game logic in save" || echo "OK"
```

## Pitfalls

1. **Array index shift on insert**: inserting a new SHOP_ITEM or
   COMPONENT_ITEM in the middle of the array shifts all subsequent indices.
   Save files store item IDs by index. Migration required.

2. **Recipe ingredient count mismatch**: if a CRAFT_RECIPES entry lists
   more ingredients than exist in COMPONENT_ITEMS (by ID), the recipe is
   impossible. Verify every ingredientId exists.

3. **Drop pool coverage gap**: if a component is required at Rank N but
   only appears in drop pools at Rank N+2 or higher, the player is soft
   locked. Check rank availability.

4. **Infinite profit via crafting**: if {buy ingredients → craft → sell
   product} loop has (sellPrice - buyCost) > 0 and no limit (e.g., once
   per day), it's an exploit. Add cooldown or adjust prices.

5. **Silent data changes**: changing pricing without updating the spec
   means the spec decays. Always spec-first.

6. **Save incompatibility on data reshape**: changing the structure of
   tables that save data references (e.g., renaming fields) breaks
   existing saves. Always migration-plan first.

7. **Guild rank gate mismatch**: if a recipe requires Rank 5 to craft but
   its ingredients only drop at Rank 3, check intentionality. Document
   intentional gates, fix unintentional ones.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped economy changes.
3. Commit with atomic message: `"feat(economy): <change description>"`
4. Push the current branch.
5. Record commit SHA and push result.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- credentials or branch policy prevent push.
