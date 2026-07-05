# Playdate Pixel UI / Renderer Health Snapshot

Date: 2026-07-05
Status: draft, not production-certified.

## Present

- A2A Agent Card.
- A2A payload contract through the shared protocol.
- Entrypoint.
- Role instructions.
- Source map.
- Workflow and automation packs.
- Tool policy.
- Role rubric and challenge checklist.
- Draft eval plan.
- Run-record template.
- Release/rollback notes.

## Missing Before Pilot Promotion

- 3-5 real pilot run records on Locksmith renderer/UI tasks.
- Smoke eval cases.
- Regression cases from real rendering/UI failures.
- Validator/schema integration.
- Human operating-model review.
- Device screen readability validation case.

## Known Drift To Monitor

- Project AGENTS.md Pitfalls section — new rendering bugs.
- `docs/renderer.md` and `docs/ui.md` spec freshness.
- Playdate SDK graphics API changes (polygon, draw modes, fonts).
- Font role assignments in `source/fonts.lua`.
- Layer order changes in renderer.lua drawLock.
