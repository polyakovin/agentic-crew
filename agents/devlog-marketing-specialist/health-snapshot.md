# Devlog / Community Marketing Specialist Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card with 3 skills.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (DM-001 through DM-010).
- Run-record template with marketing-specific fields.
- Release/rollback checklist.
- Boot probe with generalised known_failure_carveout (no stale test reference).
- Codex wrapper at `.agents/skills/devlog-marketing-specialist/SKILL.md`.
- Hermes skill at `.hermes/skills/devlog-marketing-specialist.md` (expanded — 85+ lines with full non-goals, tone, workflow, verification gates, pitfalls).
- Hermes package at `.hermes/agents/devlog-marketing-specialist/` (manifest.yaml + instructions.md).
- Pack routing for devlog-marketing-specialist in playdate-game-crew.yaml (7 routing keys).
- Agent Tester review recorded (`.agent-tester-review-2025-07-05.md`).

## Agent Tester Findings — Resolution Status

### HIGH (all resolved):
- **H1** (YAML colon quoting): fixed in `harness.yaml` and `manifest.yaml` — values with colon quoted.
- **H2** (Stale known_failure_carveout): replaced with general carveout — "Marketing copy does not modify code..."
- **H3** (Visual Art fallback): added fallback to `role.md`, `entrypoint.md`, `workflow.md` — document screenshot need, flag to user, no improvisation.

### MEDIUM (all resolved):
- **M1** (Missing quality_gates): added `no emoji in formal posts` and `visual asset needs documented, not improvised` to `manifest.yaml`.
- **M2** (Asymmetric forbidden/release): added 4 `forbidden_actions` to `manifest.yaml` (technical jargon, format mismatch, falsified evidence, skip git log); added 3 `release_blockers` to `harness.yaml` (change_game_logic_or_design, write_runtime_code, use_corporate_marketing_tone).
- **M3** (Narrative-noir stale refs): updated 3 files in `narrative-noir-copy-specialist/` — role.md, entrypoint.md, health-snapshot.md — to state devlog-marketing is created (draft), bidirectional handoff active.
- **M4** (Thin Hermes skill): expanded from 34 to 85+ lines — full non-goals, tone, workflow, verification gates, pitfalls.

### LOW (resolved):
- **L1** (Dangling cross-reference): replaced "Harness Capability Inventory" section in `source-map.md` with "Capability Review" that references the actual three-layer enforcement.
- **L2-L5**: acknowledged, not yet resolved (consolidation, URL placeholder, protocol subfolder, seed eval execution — suitable for pilot iteration).

## Known Gaps

- No live A2A runtime endpoint deployed; URL is placeholder.
- No pilot run records exist.
- No eval seed cases have been executed.
- Promotion requires pilot run records and protocol-steward review.
- Visual Art / Asset Production Specialist (handoff target for screenshots) is planned but not yet created — **fallback documented**: document screenshot need concretely, flag to user, no improvisation.
- Narrative/Noir Copy Specialist is created (draft) — handoff for in-game tone is available.
- `What to read` list still duplicated across 5 files (L2) — consolidation deferred to pilot iteration.
- Agent-card.json URL is placeholder (L3) — needs real endpoint before pilot.
- Seed evals not executed (L5) — schedule before pilot promotion.

## Next Health Checks

- Run seed evals from `eval-plan.md`.
- Pilot one real devlog post or release notes draft on Locksmith.
- Verify claim-verification workflow on a real git log.
- Verify handoff to Narrative Noir Copy Specialist on a tone-sensitive task.
- Verify fallback workflow for missing Visual Art Specialist.
- Consolidate "what to read" list into single source of truth (source-map.md).
