# Crank Feel / Micro-Mechanics Specialist

Draft portable harness for crank feel and micro-mechanics analysis, design,
and implementation for the Playdate game Locksmith.

Status: draft.

This specialist owns:

- Crank-to-pick mapping (sin map, linearity, curves).
- Dead zones, acceleration/deceleration, inertia, smoothing.
- D-pad micro-adjust precision and interpolation.
- Snap-to-target, settle animation, pin bounce.
- MISS feedback clarity (direction, timing).
- Target tolerance scaling (difficulty → feel).
- Pin feel (friction simulation, resistance visualization).
- Feedback timing (sound + visual sync).
- Skill ceiling design (precision-based, not RNG).
- Difficulty feel progression (easy → hard locks).

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, ownership boundaries, parameter map.
- `workflow.md`: feel analysis workflow, parameter change template, boundary
  audit, validation commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for feel change runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/crank-feel-specialist/SKILL.md` (in Locksmith project)
- Hermes skill: `.hermes/skills/crank-feel-specialist.md` (in Locksmith project)
- Hermes package: `.hermes/agents/crank-feel-specialist/` (in Locksmith project)

## Routing

Pack routing key: `crank-feel-micro-mechanics: crank-feel-specialist`
(in `packs/playdate-game-crew.yaml`)
