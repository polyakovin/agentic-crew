# Playdate Pixel UI / Renderer Specialist

Designs, reviews, and implements pixel-perfect 1-bit UI, HUD, menus, renderer
layers, and visual feedback states for Playdate game projects.

## Status

`draft` — harness created, not yet piloted.

## Files

- `entrypoint.md` — session entrypoint and mission.
- `role.md` — mission, scope, non-goals, gates, handoff.
- `agent-card.json` — A2A Agent Card (JSON-RPC).
- `harness.yaml` — harness wiring: runtime, activation, context, gates.
- `source-map.md` — source hierarchy and truth rules.
- `workflow.md` — automation packs: draw-mode audit, boundary check, pixel QA,
  build verify, style audit.
- `tool-policy.md` — allowed/denied tools, risk matrix, injection boundary.
- `rubric.md` — quality levels and critical failure detectors.
- `eval-plan.md` — capability map and seed case families.
- `run-record.template.json` — structured run record with visual-specific fields.
- `release-rollback.md` — release blockers and rollback scope.
- `health-snapshot.md` — current status and missing items.

## Runtime Surfaces

- A2A: `agents/playdate-pixel-ui-renderer-specialist/`
- Codex wrapper: `.agents/skills/playdate-pixel-ui-renderer-specialist/SKILL.md`
- Hermes skill: `.hermes/skills/playdate-pixel-ui-renderer-specialist.md`
- Hermes package: `.hermes/agents/playdate-pixel-ui-renderer-specialist/`

## Pack Routing

This specialist is routed through `packs/playdate-game-crew.yaml`:

```yaml
  pixel-ui-renderer: playdate-pixel-ui-renderer-specialist
  renderer-audit: playdate-pixel-ui-renderer-specialist
  ui-review: playdate-pixel-ui-renderer-specialist
  visual-qa: playdate-pixel-ui-renderer-specialist
```

## Target Project

Locksmith (PlayDate game) at `/root/locksmith`.
