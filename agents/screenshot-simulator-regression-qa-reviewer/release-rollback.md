# Screenshot / Simulator Regression QA Reviewer Release And Rollback

## Status

Draft package. Not production until pilot records and agent-tester review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- QA report falsified or fabricated.
- Critical overlap found but not reported.
- Simulator crash not classified — left as "it crashed".
- Game renamed to Safe Cracker.
- Visual readiness claimed without screenshot evidence.
- Vision-model analysis of images without user request.
- Validation not run.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/screenshot-simulator-regression-qa-reviewer/`
- `.hermes/agents/screenshot-simulator-regression-qa-reviewer/`
- `.hermes/skills/screenshot-simulator-regression-qa-reviewer.md`
- `.agents/skills/screenshot-simulator-regression-qa-reviewer/SKILL.md`
- affected entries in `packs/playdate-game-crew.yaml`

Rollback owner: orchestrator.
Release gate owner: user for QA decisions; protocol steward for harness.

## Rollback Steps

1. Identify the bad QA review or regression report.
2. Capture the failure as a learning/eval case.
3. Revert only affected QA files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot if needed.

## Smoke Release Checks

```bash
python3 -m json.tool agents/screenshot-simulator-regression-qa-reviewer/agent-card.json
python3 -m json.tool agents/screenshot-simulator-regression-qa-reviewer/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/screenshot-simulator-regression-qa-reviewer/harness.yaml'))"
rg -n "[[:blank:]]$" agents/screenshot-simulator-regression-qa-reviewer
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
