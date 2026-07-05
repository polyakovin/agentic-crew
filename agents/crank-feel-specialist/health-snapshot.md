# Crank Feel / Micro-Mechanics Specialist Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (CF-001 through CF-010).
- Run-record template with feel-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith code.
- Codex wrapper at `.agents/skills/crank-feel-specialist/SKILL.md`.
- Hermes skill at `.hermes/skills/crank-feel-specialist.md`.
- Hermes package at `.hermes/agents/crank-feel-specialist/`.
- Pack routing for `crank-feel-micro-mechanics` in `playdate-game-crew.yaml`.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- Promotion requires pilot run records and protocol-steward review.
- Crank wrap-around (359° → 0°) behaviour must be verified on actual
  Playdate hardware.

## Next Health Checks

- Run seed evals from `eval-plan.md`.
- Pilot one real crank feel change on Locksmith.
- Verify crank wrap-around on Playdate hardware.
- Verify feedback timing sync on Simulator and device.
- Review architectural boundary audit on a real code change.
