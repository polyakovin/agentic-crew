# Endless Economy Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- Missing spec update for a behaviour change.
- Architectural boundary violated (gfx.* in data files, etc.).
- `make check` fails or not run.
- Economy invariant broken and not documented.
- Save compatibility broken without migration plan.
- Infinite profit loop introduced.
- RNG added to economy mechanics.
- AGENTS.md Pitfalls not updated for discovered bugs.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/endless-economy-specialist/` — harness files.
- `.agents/skills/endless-economy-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/endless-economy-specialist.md` — Hermes skill.
- `.hermes/agents/endless-economy-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert `source/endless_data.lua`, `source/economy_catalogs.lua`,
  `source/endless_save.lua` changes.
- Revert spec changes in `docs/*.md`.
- Revert Pitfalls additions in `AGENTS.md`.

Rollback owner: orchestrator.
Release gate owner: protocol steward or AgentOps owner.

## Rollback Steps (Agent Infrastructure)

1. Identify the bad harness, wrapper, or routing change.
2. Capture the failure as a learning/eval case.
3. Revert only affected specialist files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot.

## Rollback Steps (Locksmith Code Changes)

1. Identify the economy parameter that regressed.
2. Revert the specific source file change.
3. Revert the spec change if the spec was updated for this parameter.
4. Run `make check` to confirm rollback.
5. If save migration was applied: verify rollback migration or document
   that saves from the bad version need manual fix.
6. Update AGENTS.md Pitfalls if the regression reveals a new rule.
7. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/endless-economy-specialist/agent-card.json
python3 -m json.tool agents/endless-economy-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/endless-economy-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/endless-economy-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make check
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
