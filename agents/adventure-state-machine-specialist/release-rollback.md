# Adventure State Machine Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- Missing state machine issue documentation for a transition change.
- Architectural boundary violated (state logic in renderer, etc.).
- `make check` fails or not run.
- Spec not updated when ADVENTURE_STATE enum or flow changed.
- Unreachable state introduced.
- Impossible transition introduced.
- Orphan story transition introduced (dangling nextKey/nextState).
- Phase state leak detected (self.phase not recreated/cleaned).
- B button exits Adventure (breaking core design constraint).
- Win screen exit missing or broken.
- Persistence boundary violated (markAdventureComplete called early).
- Coupling with Speedrun or Endless logic introduced.
- New global mutable state added without necessity.
- AGENTS.md Pitfalls not updated for discovered bugs.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/adventure-state-machine-specialist/` — harness files.
- `.agents/skills/adventure-state-machine-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/adventure-state-machine-specialist.md` — Hermes skill.
- `.hermes/agents/adventure-state-machine-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert `source/states.lua` — ADVENTURE_STATE enum changes.
- Revert `source/adventure_mode.lua` — state machine changes.
- Revert `source/adventure/ad_curtain.lua` — phase 1 changes.
- Revert `source/adventure/ad_safedial.lua` — phase 3 changes.
- Revert `source/adventure/ad_clock.lua` — phase 4 changes.
- Revert `source/adventure/ad_renderer.lua` — adventure drawing changes.
- Revert spec changes in `docs/game.md`, `docs/manifest.json`,
  `docs/test-scenarios.md`.
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

1. Identify the state machine change that regressed.
2. Revert the specific source file change (states.lua, adventure_mode.lua,
   adventure/*.lua).
3. Revert the spec change if the spec was updated for this change.
4. Run `make check` to confirm rollback.
5. Update AGENTS.md Pitfalls if the regression reveals a new rule.
6. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/adventure-state-machine-specialist/agent-card.json
python3 -m json.tool agents/adventure-state-machine-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/adventure-state-machine-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/adventure-state-machine-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make check
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
