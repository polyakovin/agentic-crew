# Locksmith Orchestrator Health Snapshot

Status: draft-ready.
Version: 0.1.0.

## Present
- role.md with mission, scope, routing rules, mandatory workflow,
  failure-handling protocol, and guardrails.
- source-map.md with context economy contract.
- workflow.md with boot probe, core workflow, routing heuristic, coordinator
  protocol, failure-handling workflow, and quality gates.
- tool-policy.md with allowed/forbidden actions, approval gates, and validation
  policy.
- rubric.md and eval-plan.md with seed cases.
- run-record.template.json with task, routing, verification, and conflict
  tracking fields.
- release-rollback.md with rollback scope and steps.
- agent-card.json with A2A Agent Card.
- harness.yaml wired to all harness docs.

## Known Gaps
- No live A2A runtime endpoint deployed.
- Promotion requires pilot run records from actual use.
- Agent Tester review completed — no critical findings. See backlog in
  `eval-plan.md` for P1/P2 items.
- 7 of 11 routed specialists do not exist yet (Lua Gameplay Engineer, Endless
  Economy, Audio/Music, Adventure State Machine, Devlog/Marketing, Visual Art,
  Toolchain/Build). Failure protocol handles this, but routing completeness
  improves as specialists are created.

## Next Health Checks
- Create Hermes wrapper files (manifest.yaml, instructions.md, SKILL.md).
- Request Agent Tester review.
- Route at least one real user task through the orchestrator.
- Confirm pack routing entry in playdate-game-crew.yaml is correct.
