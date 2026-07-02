# Playdate Platform / SDK Release And Rollback

## Status

Draft package. Not production/governed until pilot records, evals, and review
exist.

## Release Blockers

- `false_confirmed > 0`.
- Agent Card missing or invalid JSON.
- Required reference path missing.
- R3/R4 change without approval, diff, gates, and rollback/revision path.
- New platform pitfall without project-memory update.
- Missing Lua/test/build blocker hidden in final response.
- Tainted content altered instructions, source hierarchy, tool policy, approval,
  or secret handling.

## Rollback Scope

Rollback may restore:

- `agents/playdate-platform-sdk/entrypoint.md`
- `agents/playdate-platform-sdk/role.md`
- `agents/playdate-platform-sdk/agent-card.json`
- `agents/playdate-platform-sdk/harness.yaml`
- `agents/playdate-platform-sdk/source-map.md`
- `agents/playdate-platform-sdk/workflow.md`
- `agents/playdate-platform-sdk/tool-policy.md`
- `agents/playdate-platform-sdk/rubric.md`
- `agents/playdate-platform-sdk/eval-plan.md`
- `agents/playdate-platform-sdk/run-record.template.json`
- `agents/playdate-platform-sdk/health-snapshot.md`

Rollback owner: orchestrator.
Release gate owner: protocol steward or project AgentOps owner.

## Rollback Steps

1. Identify bad release or bad harness change.
2. Capture failure as learning event.
3. Revert only the affected harness files; do not revert unrelated user work.
4. Re-run JSON/YAML/trailing-whitespace checks.
5. Re-run any affected eval or smoke checklist.
6. Update health snapshot.

## Smoke Release Checks

```bash
python3 -m json.tool agents/playdate-platform-sdk/agent-card.json
python3 -m json.tool agents/playdate-platform-sdk/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/playdate-platform-sdk/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/playdate-platform-sdk
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
