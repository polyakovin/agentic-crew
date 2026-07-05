# Performance Bundle Budget Specialist Health Snapshot

Status: draft (newly created).
Version: 0.1.0.
Last validated: 2026-07-05.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (PERF-001 through PERF-010).
- Run-record template with performance-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith code.
- [x] Codex wrapper at `.agents/skills/performance-bundle-budget-specialist/SKILL.md`.
- [x] Hermes skill at `.hermes/skills/performance-bundle-budget-specialist.md`.
- [x] Hermes package at `.hermes/agents/performance-bundle-budget-specialist/`.
- [x] Pack routing (7 performance keys) in `playdate-game-crew.yaml`.

## Validation (2026-07-05)

- [x] JSON: agent-card.json, run-record.template.json — valid.
- [x] YAML: harness.yaml, playdate-game-crew.yaml — valid.
- [x] Path resolution: all 13 harness paths + 2 protocol paths resolve.
- [x] Hermes manifest paths: all 7 paths resolve from locksmith/.hermes/agents/.
- [x] quality_gates: 12 (>= 8 ✓) in harness.yaml — synced with role.md Pre-Change Checklist.
- [x] forbidden_actions: 24 (>= 6 ✓) in tool-policy.md, 14 (>= 6 ✓) in manifest.yaml.
- [x] Trailing whitespace: CLEAN.
- [x] Non-goals consistent across role.md, entrypoint.md, SKILL.md, Hermes skill.
- [x] Boot probe commands defined and executable.
- [x] Duplicate role check completed — no overlap with existing agents.
- [x] Agent Tester review: dispatched 2026-07-05; 2 HIGH, 4 MEDIUM, 4 LOW findings — HIGH fixed (H1 dangling ref, H2 SFX budget), MEDIUM fixed (M2 asset sizes, M3 instructions.md non-goals, M4 q-gates sync). M1 (peer handoff awareness) deferred to follow-up.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- Promotion requires pilot run records and protocol-steward review.
- PDX budget tracking requires `make build` — not run by this specialist.
- Hot path allocations can only be verified in Simulator, not in static analysis.
- Audio compression format verification depends on `pdc` build output.

## Next Health Checks

- [ ] Run seed evals from `eval-plan.md`.
- [ ] Pilot one real performance audit on Locksmith.
- [ ] Produce PDX budget breakdown with accurate measurements.
- [ ] Verify hot path allocation counts against source.
- [ ] Review cache strategy with dirty-flag trace.
- [ ] Test audio compression budget against actual `.pda` sizes.
- [ ] Review architectural boundary audit on data modules.
- [ ] Dispatch Agent Tester review.
