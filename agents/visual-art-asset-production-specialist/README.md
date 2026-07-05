# Visual Art / Asset Production Specialist

Draft A2A specialist harness for the Visual Art / Asset Production domain in
the Locksmith Playdate game project.

## Harness Files

| File | Purpose |
|------|---------|
| `role.md` | Mission, scope, non-goals, tone, source hierarchy, QA contracts, handoff contract. |
| `harness.yaml` | Harness metadata, runtime config, quality gates, release blockers. |
| `agent-card.json` | A2A Agent Card per Agentic Crew protocol. |
| `entrypoint.md` | Entrypoint, role lens, calibration, what to read. |
| `source-map.md` | Source hierarchy, ownership, duplicate role check, wrapper expectations. |
| `workflow.md` | Boot probe, core workflow, production sub-workflows (title/launcher, adventure, items, lock-edit), prompt crafting, 1-bit conversion, QA, validation. |
| `tool-policy.md` | Allowed actions, approval gates, forbidden actions, file-system policy, git policy. |
| `rubric.md` | Evaluation rubric: Excellent, Acceptable, Needs Revision, Critical Failure. |
| `eval-plan.md` | Seed eval cases (VA-001 through VA-010), metrics, promotion thresholds. |
| `run-record.template.json` | Structured run-record template for production tracking. |
| `release-rollback.md` | Release blockers, rollback scope, smoke checks. |
| `health-snapshot.md` | Current status, known gaps, next health checks. |

## Wrappers

- Hermes: `/root/locksmith/.hermes/agents/visual-art-asset-production-specialist/`
- Codex: `/root/locksmith/.agents/skills/visual-art-asset-production-specialist/SKILL.md`

## Routing

Pack: `packs/playdate-game-crew.yaml`
Keys: `visual-art`, `asset-production`, `image-generation`, `1-bit-conversion`, `title-art`, `launcher-art`, `story-backgrounds`, `item-sheets`, `dithering-qa`, `prompt-crafting`, `contact-sheet-qa`, `lock-background-edit`.

## Status

Draft. Not production until pilot records and agent-tester review exist.
