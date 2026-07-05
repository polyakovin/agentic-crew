# Devlog / Community Marketing Specialist Release And Rollback

## Status

Draft package. Not production until pilot records and agent-tester review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Devlog post template not followed (devlog/template.md).
- Game renamed to Safe Cracker in public copy.
- Hero named incorrectly (not Silas Crane).
- Claims made about unimplemented features.
- Corporate marketing tone detected in copy.
- Copy published without user approval.
- Canon issues not handed off to Narrative Specialist.
- Screenshots improvised instead of coordinated with Visual Art Specialist.
- Visual assets generated instead of coordinated.
- Validation not run.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/devlog-community-marketing-specialist/`
- `.hermes/agents/devlog-community-marketing-specialist/`
- `.hermes/skills/devlog-community-marketing-specialist.md`
- `.agents/skills/devlog-community-marketing-specialist/SKILL.md`
- affected entries in `packs/playdate-game-crew.yaml`

Rollback owner: orchestrator.
Release gate owner: user for itch.io and social media publication; protocol steward for harness.

## Rollback Steps

1. Identify the bad copy or agent infrastructure change.
2. Capture the failure as a learning/eval case.
3. Revert only affected copy files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot if needed.

## Smoke Release Checks

```bash
python3 -m json.tool agents/devlog-community-marketing-specialist/agent-card.json
python3 -m json.tool agents/devlog-community-marketing-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/devlog-community-marketing-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/devlog-community-marketing-specialist
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
