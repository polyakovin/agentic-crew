# Screenshot / Simulator Regression QA Reviewer Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs (role, entrypoint, source-map, workflow, tool-policy).
- Rubric and eval plan.
- Run-record template.
- Release/rollback checklist.
- Duplicate role review completed (adapts generic qa-reviewer; complements Pixel UI, Narrative, Gameplay Design, Toolchain, Release Packaging).
- Hermes skill in Locksmith (`.hermes/skills/`).
- Hermes package in Locksmith (`.hermes/agents/`).
- Codex wrapper in Locksmith (`.agents/skills/`).
- Pack routing updated (`packs/playdate-game-crew.yaml`).

## Known Gaps

- No live A2A runtime endpoint is deployed.
- Agent Tester review not yet run.
- Promotion requires pilot run records (real Locksmith screenshot sweep).
- Screenshot harness scripts may not exist yet in Locksmith `build/` — gap to document, not a harness failure.

## Next Health Checks

- [ ] Run validation.
- [ ] Run seed evals from `eval-plan.md`.
- [ ] Execute one real Locksmith screenshot sweep.
- [ ] Perform one visual regression comparison with baseline.
- [ ] Triage at least one simulator crash.
- [ ] Request agent-tester review.
