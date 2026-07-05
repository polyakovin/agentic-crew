# Toolchain / Build / Sandbox Gatekeeper Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (TB-001 through TB-010).
- Run-record template with toolchain-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith toolchain.
- Codex wrapper at `.agents/skills/toolchain-build-sandbox-gatekeeper/SKILL.md`.
- Hermes skill at `.hermes/skills/toolchain-build-sandbox-gatekeeper.md`.
- Hermes package at `.hermes/agents/toolchain-build-sandbox-gatekeeper/`.
- Pack routing for toolchain/build/sandbox keys in `playdate-game-crew.yaml`.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- No real diagnostic cycle has been run on Locksmith.
- Promotion requires pilot run records and agent-tester review.
- Lua-missing fallback sequence not yet validated against actual Lua absence.
- Pdc/SDK diagnostic not yet tested on a system without Playdate SDK.
- Githook health assessment not yet validated against actual hook files.
- Simulator quirk (Xvfb on Linux) not yet verified on this host.

## Gate Health (not yet run)

| Gate | Outcome | Detail |
|------|---------|--------|
| make doctor | not run | — |
| make test-all | not run | — |
| make lint | not run | — |
| make build | not run | — |
| make check | not run | — |
| headless-test | not run | — |
| pre-commit hook | not assessed | — |
| post-commit hook | not assessed | — |

## Toolchain Status (not yet checked)

| Tool | Available | Path |
|------|-----------|------|
| Lua | not checked | — |
| pdc | not checked | — |
| SDK_DIR | not checked | — |
| Simulator | not checked | — |
| Xvfb | not checked | — |
| jq | not checked | — |
| git | not checked | — |

## Next Health Checks

- [ ] Run seed evals from `eval-plan.md`.
- [ ] Run first diagnostic cycle on Locksmith: `make doctor` → gate cycle.
- [ ] Validate Lua-missing fallback sequence (simulate absence or verify on
  system without Lua).
- [ ] Validate pdc/SDK diagnostic on actual Playdate SDK configuration.
- [ ] Assess githook health on actual `.githooks/pre-commit` and `post-commit`.
- [ ] Verify simulator quirk (Xvfb requirement) on this Linux host.
- [ ] Dispatch Agent Tester review.
- [ ] Document any discovered toolchain pitfalls in AGENTS.md.
