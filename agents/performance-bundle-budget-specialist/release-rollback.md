# Performance Bundle Budget Specialist Release And Rollback

## Status

Draft package. Not production/governed until pilot records, eval evidence,
and independent review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Hermes skill/package missing or invalid (when required).
- Codex wrapper missing (when required).
- Pack routing points to missing path.
- Recommendation made without file:line evidence.
- Recommendation made without measured cost.
- Recommendation made without verification command.
- Code mutated without explicit user request.
- Residual risk not documented.
- AGENTS.md Pitfalls not updated for discovered patterns.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/performance-bundle-budget-specialist/` — harness files.
- `.agents/skills/performance-bundle-budget-specialist/SKILL.md` — Codex wrapper.
- `.hermes/skills/performance-bundle-budget-specialist.md` — Hermes skill.
- `.hermes/agents/performance-bundle-budget-specialist/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert any code changes made as optimization patches.
- Revert any AGENTS.md Pitfalls additions.
- Restore any asset files if modified.

Rollback owner: orchestrator.
Release gate owner: protocol steward or AgentOps owner.

## Rollback Steps (Agent Infrastructure)

1. Identify the bad harness, wrapper, or routing change.
2. Capture the failure as a learning/eval case.
3. Revert only affected specialist files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/path/whitespace checks.
6. Update health snapshot.

## Rollback Steps (Locksmith Code Changes)

1. Identify the optimization patch that regressed.
2. Revert the specific source file change.
3. Revert any AGENTS.md Pitfalls entry if it was based on the bad change.
4. Re-measure PDX size and hot path allocation counts.
5. Update AGENTS.md Pitfalls if the regression reveals a new rule.
6. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/performance-bundle-budget-specialist/agent-card.json
python3 -m json.tool agents/performance-bundle-budget-specialist/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/performance-bundle-budget-specialist/harness.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/performance-bundle-budget-specialist packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
du -sh Locksmith.pdx 2>/dev/null || echo "PDX not built"
du -sh source/images/ source/sounds/
ls -laS source/images/*.png | head -10
du -h Locksmith.pdx/sounds/*.pda 2>/dev/null | sort -rh
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
