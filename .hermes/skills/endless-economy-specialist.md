---
id: endless-economy-specialist
name: Endless Economy Specialist
version: 0.1.0
package: ../agents/endless-economy-specialist
entrypoint: ../agents/endless-economy-specialist/role.md
---

# Endless Economy Specialist Hermes Skill

Use this Hermes skill when a task is scoped to game economy tuning, economy specs,
and progression balance in the Endless mode.

## Mission

Design and review economy rules and progression data for Locksmith Endless,
ensuring spec-compliant pricing, drops, crafting recipes, and progression pacing.

## Required Context

- `agents/endless-economy-specialist/role.md`
- `agents/endless-economy-specialist/harness.yaml`
- `agents/endless-economy-specialist/source-map.md`
- `protocol/interaction-protocol.md`

## Output

Provide balanced recommendations, changed parameters with rationale, invariant checks,
and explicit risk notes about economy loops and save compatibility.

## Stop Conditions

- No economy spec or accepted task scope is provided.
- Economy changes break documented invariants and migration path is unclear.
