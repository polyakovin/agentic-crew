# Agent Architect / Crew Builder Release And Rollback

## Status

Draft-ready package. Not production/governed until pilot records, eval evidence,
and independent protocol review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes required but `.hermes` skill/package missing or invalid.
- Codex required but Codex wrapper missing.
- Pack routing points to a missing path.
- Creation scope decision missing.
- Existing reusable harness ignored without rationale.
- Harness capability inventory missing, incomplete, or reduced to "use
  everything" without role-specific rationale.
- Agent Tester post-creation review missing for a created or materially updated
  specialist.
- Agent Tester critical findings unresolved.
- Agent Tester follow-up tasks missing from the task-specified todo or backlog
  artifact.
- Agent Tester follow-up tasks not handed to `agent-tuner` for execution.
- Duplicate role review skipped.
- Specialist-only agent обвязка left duplicated in shared project space without
  an intentional-shared rationale.
- Product specs/source/assets/tests moved during agent packaging without an
  explicit product-architecture request.
- Target project private paths or snapshots copied into reusable package.
- Commit/push result missing or falsely reported.
- Production status claimed without pilot/eval/review evidence.

## Rollback Scope

Rollback may restore:

- `agents/agent-architect-crew-builder/`
- `.agents/skills/agent-architect-crew-builder/`
- `.hermes/skills/agent-architect-crew-builder.md`
- `.hermes/agents/agent-architect-crew-builder/`
- affected entries in `packs/software-development-crew.yaml`
- affected entries in `packs/playdate-game-crew.yaml`

Rollback owner: orchestrator.
Release gate owner: protocol steward or AgentOps owner.

## Rollback Steps

1. Identify the bad harness or routing change.
2. Capture the failure as a learning/eval case.
3. Revert only affected Crew Builder files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot and target-project status if needed.

## Smoke Release Checks

```bash
python3 -m json.tool agents/agent-architect-crew-builder/agent-card.json
python3 -m json.tool agents/agent-architect-crew-builder/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-architect-crew-builder/harness.yaml"); YAML.load_file(".hermes/agents/agent-architect-crew-builder/manifest.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/agent-architect-crew-builder .agents/skills/agent-architect-crew-builder .hermes/skills/agent-architect-crew-builder.md .hermes/agents/agent-architect-crew-builder packs
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
