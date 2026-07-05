# Toolchain / Build / Sandbox Gatekeeper Release And Rollback

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
- Gate classification missing or incomplete.
- Exit 127 classified as `passed` or `failed` instead of `blocked`.
- Fallback evidence missing when Lua is absent.
- Pdc/SDK diagnostic not traced to actual paths.
- Githook assessment not based on file content.
- Game code modified by gatekeeper.
- Commit or push attempted by gatekeeper.
- Evidence based on speculation, not command output.
- New toolchain pitfalls not documented in AGENTS.md.

## Rollback Scope

Rollback may restore:

- `agents/toolchain-build-sandbox-gatekeeper/` — harness files.
- `.agents/skills/toolchain-build-sandbox-gatekeeper/SKILL.md` — Codex wrapper.
- `.hermes/skills/toolchain-build-sandbox-gatekeeper.md` — Hermes skill.
- `.hermes/agents/toolchain-build-sandbox-gatekeeper/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing entries.

Target project (Locksmith) rollbacks:

- Revert any accidentally modified source files (should be none — gatekeeper
  is evidence-only).
- Revert AGENTS.md pitfall entries if inaccurate.

Rollback owner: orchestrator.
Release gate owner: protocol steward or AgentOps owner.

## Rollback Steps (Agent Infrastructure)

1. Identify the bad harness, wrapper, or routing change.
2. Capture the failure as a learning/eval case.
3. Revert only affected specialist files or routing entries.
4. Do not revert unrelated dirty user changes.
5. Re-run JSON/YAML/trailing-whitespace checks.
6. Update health snapshot.

## Rollback Steps (Locksmith Toolchain)

1. Identify the accidentally modified file.
2. Revert the specific file change.
3. If Makefile or scripts were modified: restore from git.
4. If AGENTS.md pitfalls were added incorrectly: revert those entries.
5. Run `make doctor` to confirm toolchain state.
6. Record the rollback as a learning event.

## Smoke Release Checks (Agent Infrastructure)

```bash
python3 -m json.tool agents/toolchain-build-sandbox-gatekeeper/agent-card.json
python3 -m json.tool agents/toolchain-build-sandbox-gatekeeper/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/toolchain-build-sandbox-gatekeeper/harness.yaml')); print('yaml ok')"
rg -n "[[:blank:]]$" agents/toolchain-build-sandbox-gatekeeper packs
```

## Smoke Release Checks (Locksmith)

```bash
cd /root/locksmith
make doctor
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
