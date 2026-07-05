# Endless Economy Specialist

Draft portable harness for Endless mode economy analysis, design,
balancing, and invariant verification for the Locksmith Playdate game.

Status: draft.

This specialist owns:

- Economy analysis: item catalogs, pricing, recipes, drops, scarcity,
  progression pacing, upgrade curves, player loops, grind intensity,
  soft locks, exploits.
- Balance changes: pricing adjustments, drop rate tuning, recipe cost
  rebalancing, progression curve smoothing.
- Invariant verification: no impossible recipes, no items without sources,
  no infinite profit, starting economy viable, late-game multi-path,
  prices/rewards follow curves.
- Save compatibility: migration planning, backward compatibility
  assessment.
- Testability: unit/integration tests for economy tables, lint/test/build
  gate verification.
- Spec-first workflow: docs first, data second, verification third.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, ownership boundaries, economy data map.
- `workflow.md`: economy analysis workflow, invariant checklist, balance
  patch template, save compatibility audit, validation commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for economy change runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/endless-economy-specialist/SKILL.md` (in Locksmith project)
- Hermes skill: `.hermes/skills/endless-economy-specialist.md` (in Locksmith project)
- Hermes package: `.hermes/agents/endless-economy-specialist/` (in Locksmith project)

## Routing

Pack routing keys:
- `endless-economy: endless-economy-specialist`
- `economy-balance: endless-economy-specialist`
- `item-pricing: endless-economy-specialist`
- `drop-tables: endless-economy-specialist`
- `crafting-recipes: endless-economy-specialist`
- `progression-economy: endless-economy-specialist`
- `save-compatibility: endless-economy-specialist`
- `economy-invariants: endless-economy-specialist`
- `endless-economy-audit: endless-economy-specialist`

(in `packs/playdate-game-crew.yaml`)
