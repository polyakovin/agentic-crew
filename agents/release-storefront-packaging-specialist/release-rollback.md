# Release / Storefront Packaging Specialist Release And Rollback

## Status

Draft package. Not production until pilot records and agent-tester review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- pdxinfo name is not "Locksmith".
- Launcher images missing or wrong size.
- Card-highlighted animation incomplete.
- Build evidence missing or falsified.
- Release notes describe unimplemented features.
- Game renamed to Safe Cracker.
- Legacy URL not addressed in migration messaging.
- Prototype removal checklist incomplete but status changed to Released.
- Published to itch.io without user approval.
- Launcher art improvised instead of handed off.
- Copy canon not handed off when needed.
- Validation not run.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/release-storefront-packaging-specialist/`
- `.hermes/agents/release-storefront-packaging-specialist/`
- `.hermes/skills/release-storefront-packaging-specialist.md`
- `.agents/skills/release-storefront-packaging-specialist/SKILL.md`
- affected entries in `packs/playdate-game-crew.yaml`

Rollback owner: orchestrator.
Release gate owner: user for itch.io publication; protocol steward for harness.

## Rollback Steps

1. Identify the bad release packaging change.
2. Capture the failure as a learning/eval case.
3. Revert only affected packaging files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot if needed.

## Smoke Release Checks

```bash
python3 -m json.tool agents/release-storefront-packaging-specialist/agent-card.json
python3 -m json.tool agents/release-storefront-packaging-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/release-storefront-packaging-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/release-storefront-packaging-specialist
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
