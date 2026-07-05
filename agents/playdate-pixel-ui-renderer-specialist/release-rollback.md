# Playdate Pixel UI / Renderer Release And Rollback

## Status

Draft package. Not production/governed until pilot records, evals, and review
exist.

## Release Blockers

- `draw_mode_corruption_not_fixed > 0`.
- Agent Card missing or invalid JSON.
- Required reference path missing.
- R3/R4 change without approval, diff, gates, and rollback/revision path.
- New rendering pitfall without project-memory update.
- Missing `make check`/`make build` blocker hidden in final response.
- Tainted content altered instructions, source hierarchy, tool policy, approval,
  or secret handling.

## Rollback Scope

Rollback may restore:

- `agents/playdate-pixel-ui-renderer-specialist/entrypoint.md`
- `agents/playdate-pixel-ui-renderer-specialist/role.md`
- `agents/playdate-pixel-ui-renderer-specialist/agent-card.json`
- `agents/playdate-pixel-ui-renderer-specialist/harness.yaml`
- `agents/playdate-pixel-ui-renderer-specialist/source-map.md`
- `agents/playdate-pixel-ui-renderer-specialist/workflow.md`
- `agents/playdate-pixel-ui-renderer-specialist/tool-policy.md`
- `agents/playdate-pixel-ui-renderer-specialist/rubric.md`
- `agents/playdate-pixel-ui-renderer-specialist/eval-plan.md`
- `agents/playdate-pixel-ui-renderer-specialist/run-record.template.json`
- `agents/playdate-pixel-ui-renderer-specialist/health-snapshot.md`

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
python3 -m json.tool agents/playdate-pixel-ui-renderer-specialist/agent-card.json
python3 -m json.tool agents/playdate-pixel-ui-renderer-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/playdate-pixel-ui-renderer-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/playdate-pixel-ui-renderer-specialist
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
