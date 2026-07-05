# Locksmith Orchestrator Release And Rollback

## Status
Draft-ready package. Not production until pilot evaluations and user acceptance
are recorded.

## Release Blockers
- Orchestrator routing table references a non-existent specialist.
- Required harness files missing (role.md, workflow.md, source-map.md,
  tool-policy.md, rubric.md, eval-plan.md, run-record template, agent-card.json).
- Hermes package/skill missing (required for Locksmith specialists).
- Pack routing entry missing in playdate-game-crew.yaml.
- Guardrails not enforced in role.md or workflow.md.
- SDD protocol not described in mandatory workflow.

## Rollback Scope
- `agents/locksmith-orchestrator/` (full directory)
- Entry in `packs/playdate-game-crew.yaml` for locksmith-orchestrator
- Hermes artifacts: `.hermes/skills/locksmith-orchestrator.md`,
  `.hermes/agents/locksmith-orchestrator/`

## Rollback Steps
1. Identify the failing orchestration route or broken routing entry.
2. Capture the failure as an eval case in eval-plan.md.
3. Revert only the affected orchestrator files or routing entry.
4. Do not revert unrelated user changes.
5. Re-run JSON/YAML/whitespace validation.
6. Update docs/agent-specialists-plan.md status if needed.

## Smoke Release Checks
```bash
python3 -m json.tool agents/locksmith-orchestrator/agent-card.json
python3 -m json.tool agents/locksmith-orchestrator/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/locksmith-orchestrator/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/locksmith-orchestrator packs
```
