# Audio / Music Feedback Specialist

## Overview

Audio-owner for the Playdate game Locksmith: SFX design, background music playlist lifecycle, volume mixing, audio compression, crash safety. Senior audio designer for Playdate embedded constraints — brief, parametric, player-feel-driven.

## Agent Card

See `agent-card.json` for A2A descriptor (3 skills: SFX cue design, music playlist lifecycle, audio compression & PDX size analysis).

## Key Files

| File | Purpose |
|------|---------|
| `role.md` | Full role: mission, use cases, non-goals, workflow, handoff contracts |
| `harness.yaml` | Harness metadata: quality gates, release blockers, boot probe, routing |
| `entrypoint.md` | Activation triggers, calibration, A2A handoff contract |
| `source-map.md` | Source hierarchy, audio asset inventory, duplicate role check |
| `workflow.md` | Phase workflow + audio-specific templates (volume mixing, PDX size, crash safety, shuffled bag) |
| `tool-policy.md` | Tool usage rules for audio design |
| `rubric.md` | Quality evaluation rubric |
| `eval-plan.md` | 10 seed cases (AM-001 through AM-010) |
| `run-record.template.json` | Audio-specific run record fields |
| `release-rollback.md` | Release and rollback checklists |

## Routing Keys (playdate-game-crew.yaml)

`audio`, `sound-effects`, `music`, `sfx`, `playlist`, `volume-mixing`, `audio-lifecycle`

## Handoff Destinations

- Game feel/pacing → `gameplay-design-player-experience-specialist`
- Crank/input feel → `crank-feel-specialist`
- Economy sound triggers → `endless-economy-specialist`
- UI sound triggers → `playdate-pixel-ui-renderer-specialist`
- Adventure mode audio cues → `adventure-state-machine-specialist`
- In-game audio settings text → `narrative-noir-copy-specialist`
- SDK API questions → `playdate-platform-sdk`
