# Toolchain / Build / Sandbox Gatekeeper

Draft portable harness for toolchain diagnostics, build gate integrity,
sandbox verification, CI/push blocker diagnosis, and fallback evidence
collection for the Playdate game Locksmith.

Status: draft.

This specialist owns:

- `make test-all`, `make lint`, `make build`, `make check` execution and diagnosis.
- Lua-missing gate: exit 127 classification, fallback evidence collection.
- pdc/SDK configuration verification.
- Githook health assessment (pre-commit, post-commit).
- CI/push blocker diagnosis (dirty worktree, credentials, branch policy).
- Simulator quirk awareness (Xvfb on Linux).
- Gate outcome classification: passed / failed / blocked / skipped.
- Gate health snapshot production and maintenance.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, boot probe,
  classify_missing_lua_as policy.
- `source-map.md`: source hierarchy, toolchain surface map, Lua-missing
  gate diagram, duplicate role check.
- `workflow.md`: diagnostic workflow, gate classification, Lua-missing
  fallback sequence, pdc/SDK diagnostic, githook health check, push blocker
  diagnosis, simulator quirks.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases (TB-001 through TB-010) and
  promotion thresholds.
- `run-record.template.json`: structured record for diagnostic runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/toolchain-build-sandbox-gatekeeper/SKILL.md`
  (in Locksmith project)
- Hermes skill: `.hermes/skills/toolchain-build-sandbox-gatekeeper.md`
  (in Locksmith project)
- Hermes package: `.hermes/agents/toolchain-build-sandbox-gatekeeper/`
  (in Locksmith project)

## Routing

Pack routing keys in `packs/playdate-game-crew.yaml`:
- `toolchain`, `build`, `sandbox`, `make-gates`, `lua-gate`, `pdc-gate`,
  `sdk-diagnostic`, `githooks`, `ci-blockers`, `fallback-evidence`
