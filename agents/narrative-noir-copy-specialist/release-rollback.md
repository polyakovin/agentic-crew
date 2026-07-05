# Narrative / Noir Copy Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- Missing copy problem documentation for a text change.
- Spec not updated when terminology/convention changed.
- `make check` fails or not run.
- Unicode/emoji detected in text strings.
- ASCII safety check not run.
- Width fit for Playdate 400×240 not verified.
- Tone justification missing (not referenced to `docs/lore.md`).
- Canon drift introduced (same thing named differently in two files).
- Silas Crane voice broken (cheerful, chatty, heroic).
- Marketing tone in in-game strings.
- AGENTS.md Pitfalls not updated for discovered bugs.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/narrative-noir-copy-specialist/` — harness files.
- `.agents/skills/narrative-noir-copy-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/narrative-noir-copy-specialist.md` — Hermes skill.
- `.hermes/agents/narrative-noir-copy-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert `source/endless_data.lua`, `source/economy_catalogs.lua`,
  `source/ui.lua`, `source/endless_mode.lua` text changes.
- Revert spec changes in `docs/lore.md`, `docs/ui.md`.
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

## Rollback Steps (Locksmith Text Changes)

1. Identify the text string that regressed (tone, length, canon drift).
2. Revert the specific data file change.
3. Revert the spec change if the spec was updated for this string.
4. Run `make check` to confirm rollback.
5. Update AGENTS.md Pitfalls if the regression reveals a new rule.
6. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/narrative-noir-copy-specialist/agent-card.json
python3 -m json.tool agents/narrative-noir-copy-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/narrative-noir-copy-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/narrative-noir-copy-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make check
rg -n "[^\x00-\x7F]" source/endless_data.lua source/economy_catalogs.lua source/ui.lua source/endless_mode.lua
```

`rg` exit code 1 for both trailing-whitespace and non-ASCII searches means no
matches and is a pass.
