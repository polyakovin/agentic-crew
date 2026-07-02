# Playdate Platform / SDK Release And Rollback

## Status

Draft package. Not production/governed until pilot records, evals, and review
exist.

## Release Blockers

- `false_confirmed > 0`.
- Agent Card missing or invalid JSON.
- Required reference path missing.
- R3/R4 change without approval, diff, gates, and rollback/revision path.
- New platform pitfall without `AGENTS.md` update.
- Missing Lua/test/build blocker hidden in final response.
- Tainted content altered instructions, source hierarchy, tool policy, approval,
  or secret handling.

## Rollback Scope

Rollback may restore:

- `.hermes/skills/playdate-platform-sdk.md`
- `.hermes/agents/playdate-platform-sdk/*`
- `.codex/agents/playdate-platform-sdk.*`

Rollback owner: Locksmith Orchestrator.
Release gate owner: Locksmith AgentOps.

## Rollback Steps

1. Identify bad release or bad harness change.
2. Capture failure as learning event.
3. Revert only the affected harness files; do not revert user unrelated work.
4. Re-run JSON/TOML/trailing-whitespace checks.
5. Re-run any affected eval or smoke checklist.
6. Update release record and health snapshot.

## Smoke Release Checks

```bash
python3 -m json.tool .hermes/agents/playdate-platform-sdk/agent-card.json
python3 -m json.tool .hermes/agents/playdate-platform-sdk/run-record.template.json
python3 -c "import pathlib,tomllib; tomllib.loads(pathlib.Path('.codex/agents/playdate-platform-sdk.toml').read_text()); print('toml ok')"
rg -n "[[:blank:]]$" .hermes/skills/playdate-platform-sdk.md .hermes/agents/playdate-platform-sdk .codex/agents/playdate-platform-sdk.toml .codex/agents/playdate-platform-sdk.instructions.md
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
