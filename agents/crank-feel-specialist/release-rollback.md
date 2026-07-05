# Crank Feel / Micro-Mechanics Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- Missing UX intent documentation for a feel change.
- Architectural boundary violated (gfx.* in safe.lua, etc.).
- `make check` fails or not run.
- Spec not updated when behaviour changed.
- RNG added to mechanics.
- Input lag introduced through excessive smoothing.
- Feedback timing not synchronous.
- Crank wrap-around (359° → 0°) not considered.
- AGENTS.md Pitfalls not updated for discovered bugs.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/crank-feel-specialist/` — harness files.
- `.agents/skills/crank-feel-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/crank-feel-specialist.md` — Hermes skill.
- `.hermes/agents/crank-feel-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert `source/input.lua`, `source/safe.lua`, `source/renderer.lua`,
  `source/soundfx.lua`, `source/*_data.lua` changes.
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

1. Identify the feel parameter that regressed.
2. Revert the specific source file change.
3. Revert the spec change if the spec was updated for this parameter.
4. Run `make check` to confirm rollback.
5. Update AGENTS.md Pitfalls if the regression reveals a new rule.
6. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/crank-feel-specialist/agent-card.json
python3 -m json.tool agents/crank-feel-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/crank-feel-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/crank-feel-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make check
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
