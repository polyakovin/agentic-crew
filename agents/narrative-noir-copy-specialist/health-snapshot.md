# Narrative / Noir Copy Specialist Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (NC-001 through NC-010).
- Run-record template with copy-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith text.
- Pack routing for narrative-noir-copy-specialist in `playdate-game-crew.yaml`.
- Codex wrapper at `.agents/skills/narrative-noir-copy-specialist/SKILL.md`.
- Hermes skill at `.hermes/skills/narrative-noir-copy-specialist.md`.
- Hermes package at `.hermes/agents/narrative-noir-copy-specialist/`.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- Promotion requires pilot run records and protocol-steward review.
- ASCII-safety audit must be verified on actual Playdate Simulator for
  width-fit edge cases.
- Devlog Marketing Specialist (handoff target for public launch copy) is
  created (draft status) — bidirectional handoff contract is now active.
- Canon-consistency audit tool (`rg` cross-file check) is manual; automated
  canon linting is a future improvement.

## Next Health Checks

- Run seed evals from `eval-plan.md`.
- Pilot one real copy audit/change on Locksmith.
- Verify ASCII-safety and width-fit on Playdate Simulator.
- Verify canon-consistency audit on all text sources.
- Review spec-update workflow on a real text change.
