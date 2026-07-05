# Endless Economy Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly analyzes, proposes, and implements
economy balance changes while respecting architectural boundaries,
spec-driven development, invariants, save compatibility, and
predictability-over-RNG principles.

## Metrics

- task success;
- findings quality (file/line refs, severity classification);
- invariant verification completeness;
- save compatibility assessment;
- boundary respect (data vs runtime separation);
- spec update completeness;
- `make check` pass rate;
- pricing curve consistency;
- drop rate fairness;
- RNG avoidance.

## Seed Cases

### ECO-001: Full Economy Audit

Input: "Analyze the current Endless economy — are there any soft locks or
infinite profit loops?"

Expected:

- Reads AGENTS.md → `docs/manifest.json` → economy specs → data files.
- Runs all economy invariant checks.
- Produces findings sorted by severity with file/line references.
- Identifies: impossible recipes, missing drop sources, infinite profit
  loops, soft locks, pricing inconsistencies.
- Does NOT touch code — analysis only (unless user asks for fixes).
- Report format: findings first, invariant status table, recommendations.

### ECO-002: Pricing Curve Review

Input: "Review pricing across all lock/clock/safe catalogs — is progression
fair?"

Expected:

- Reads economy specs (lock/clock/safe economy) and `source/economy_catalogs.lua`.
- Extracts all catalog prices, maps against qualityId/tier.
- Checks monotonicity: higher tier = higher price (or justified exceptions).
- Checks reward formula produces profit after reasonable attempts.
- Reports anomalies with catalog reference, tier, model name, current price,
  suggested price.

### ECO-003: Recipe Dependency Check

Input: "Check if any CRAFT_RECIPES have impossible ingredient requirements."

Expected:

- Reads `source/endless_data.lua` CRAFT_RECIPES and COMPONENT_ITEMS.
- Cross-references every ingredientId against COMPONENT_ITEMS.
- Verifies every required component has a drop source at the rank where
  the recipe becomes available.
- Flags: missing ingredients (no component with that ID), ingredients
  unavailable at required rank.

### ECO-004: Drop Pool Coverage Audit

Input: "Component drops are too scarce at early ranks — check drop pool
coverage for Rank 1-3."

Expected:

- Reads COMPONENT_DROP_POOLS for ranks 1-3.
- Identifies which components gate progression at those ranks.
- Verifies every gating component appears in at least one drop pool at
  or below that rank.
- Reports gaps with component name, required rank, actual availability.

### ECO-005: Infinite Profit Loop Detection

Input: "Check for infinite profit exploits in the buy → craft → sell cycle."

Expected:

- Reads SHOP_ITEMS for buyable components/ingredients.
- Reads CRAFT_RECIPES for craftable products.
- For each recipe: computes (sellPrice - sum(buyCost of ingredients)).
- Flags any positive-profit loop without cooldown/limit.
- Suggests: add cooldown, adjust prices, or limit per-period crafting.

### ECO-006: Save Compatibility Assessment

Input: "We're adding 3 new COMPONENT_ITEMS. Assess save compatibility."

Expected:

- Checks `source/endless_save.lua` for references to COMPONENT_ITEMS
  by index or count.
- Identifies if new items are appended (safe) or inserted (unsafe).
- If unsafe: proposes migration plan with version bump.
- If safe: confirms append-only, no migration needed.
- Documents assessment in report.

### ECO-007: Boundary Violation Prevention

Input: "Add `playdate.timer.new()` to endless_data.lua to track market
refresh timing."

Expected:

- Agent catches the boundary violation.
- Refuses to add `playdate.*` to data-only module.
- Proposes alternative: timer logic goes in endless_mode.lua, reading
  market window data from endless_data.lua.
- Explains boundary rule to user.

### ECO-008: Starting Economy Viability

Input: "Check if a new player with starting gold can actually make
progress."

Expected:

- Reads endless_save.lua for starting gold default.
- Reads cheapest available locks/clocks/safes at Rank 1.
- Computes profit after buying cheapest item + successful hack.
- If profit is zero or negative, flags as soft lock.
- Suggests: adjust starting gold, reduce cheapest item price, or ensure
  beginner lock has guaranteed profit.

### ECO-009: Late-Game Multi-Path Check

Input: "Does late-game (Rank 6+) collapse into one optimal strategy?"

Expected:

- Identifies all Rank 6+ available items, recipes, and markets.
- Computes expected profit/hour for each viable strategy.
- Flags if one strategy dominates by >50% over alternatives.
- Suggests: buff underperforming paths or nerf dominant one.

### ECO-010: Spec-First Workflow

Input: User asks to change a price in LOCK_CATALOG without mentioning spec.

Expected:

- Agent reads spec first (`docs/lock-economy-spec.md`).
- Identifies the price in the spec.
- Proposes spec update before code change.
- Explains SDD workflow to user if they resist.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real economy change is implemented, tested, and deployed.
- No boundary violations in any run.
- At least one save compatibility assessment completed.

Move to `production` only after:

- Pilot evidence from 3+ economy changes.
- Spec updates are consistent and complete.
- Pitfalls are documented in AGENTS.md.
- No RNG regressions.
- No save-breaking changes without migration.
