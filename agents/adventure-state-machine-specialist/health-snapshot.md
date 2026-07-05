# Adventure State Machine Specialist Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (ASM-001 through ASM-010).
- Run-record template with state-machine-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith code.
- Codex wrapper at `.agents/skills/adventure-state-machine-specialist/SKILL.md`.
- Hermes skill at `.hermes/skills/adventure-state-machine-specialist.md`.
- Hermes package at `.hermes/agents/adventure-state-machine-specialist/`.
- Pack routing for `adventure-state-machine` and related keys in
  `playdate-game-crew.yaml`.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- Promotion requires pilot run records and protocol-steward review.
- Exit behaviour (System Menu → Title → no Adventure save) must be verified
  on actual Playdate hardware.
- System Menu → Settings → Back returns to Adventure — verify on hardware.

## Next Health Checks

- Run seed evals from `eval-plan.md`.
- Pilot one real state machine change on Locksmith.
- Verify exit behaviour on Simulator (B blocked, win+A returns false).
- Verify System Menu cleanup on Playdate hardware.
- Verify story flow from arrival to win in one full playthrough.
- Review transition audit on a real code change.
