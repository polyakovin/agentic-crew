# Persistence / Save Migration Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- health-snapshot.md not updated after diagnostic cycle.
- New save field not nil-safe (missing from defaults or migrateSaveData).
- Legacy progress destroyed by migration (money reset to 0).
- Corrupt data crashes instead of falling back to defaults.
- Legacy key fallback removed without documented migration window.
- Per-frame save write introduced.
- `make test-all` datastore or endless_save suite fails after change.
- Schema change without spec update (shared-economy.md §10).
- Migration risk not documented.
- Game logic or UI rendering added to persistence modules.
- Evidence based on speculation, not command output.
- New persistence pitfalls not documented in AGENTS.md.

## Rollback Scope

Rollback may restore:

- `agents/persistence-save-migration-specialist/` — harness files.
- `.agents/skills/persistence-save-migration-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/persistence-save-migration-specialist.md` — Hermes skill.
- `.hermes/agents/persistence-save-migration-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert `source/datastore.lua` or `source/endless_save.lua` schema changes.
- Revert `docs/shared-economy.md` §10 spec changes.
- Revert AGENTS.md pitfall entries if inaccurate.
- **Critical**: if save schema was deployed to players and then rolled back,
  previously-saved data with new fields will still be in the datastore.
  The rollback code must handle unknown fields gracefully (Lua table tolerance
  should make this safe, but verify).

Rollback owner: orchestrator.
Release gate owner: protocol steward or AgentOps owner.

## Rollback Steps (Agent Infrastructure)

1. Identify the bad harness, wrapper, or routing change.
2. Capture the failure as a learning/eval case.
3. Revert only affected specialist files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot.

## Rollback Steps (Locksmith Persistence)

1. Identify the schema change to revert.
2. Assess: has the new schema been saved to any player datastores?
   - If not deployed: safe to revert code and spec.
   - If deployed: revert code but KEEP the nil-safe migration for the new fields
     (players may have saved with new keys). Mark fields as deprecated, not removed.
3. Revert `endless_save.lua` `defaults()` and `migrateSaveData()`.
4. Revert spec: `shared-economy.md` §10.
5. Re-run `make test-all` datastore + endless_save suites.
6. Verify legacy saves still load correctly.
7. Document the rollback as a learning event.
8. Update health snapshot.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/persistence-save-migration-specialist/agent-card.json
python3 -m json.tool agents/persistence-save-migration-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/persistence-save-migration-specialist/harness.yaml')); print('yaml ok')"
rg -n "[[:blank:]]$" agents/persistence-save-migration-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make test-all 2>&1 | grep -E "datastore|endless_save|PASS"
lua scripts/test_datastore.lua 2>&1 | tail -5
lua scripts/test_endless_save.lua 2>&1 | tail -5
make lint 2>&1 | tail -2
make build 2>&1 | tail -2
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
