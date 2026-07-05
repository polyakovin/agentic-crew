# Adventure State Machine Specialist

Draft portable harness for Adventure mode state machine design, review,
and implementation for the Playdate game Locksmith.

Status: draft.

This specialist owns:

- Adventure mode state machine transitions and guards.
- ADVENTURE_STATE enum maintenance and consistency.
- Phase lifecycle: enter/update/draw/exit/reset.
- Story flow (STORY_TRANSITIONS, showStory/advanceStory).
- Persistence boundaries (Datastore.markAdventureComplete).
- Exit behaviour (B blocked, win screen → title via A, System Menu cleanup).
- Transition reachability and impossibility detection.
- Spec/code/enum synchronization (docs/game.md ↔ states.lua ↔ adventure_mode.lua).
- Adventure test scenario design.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, ownership boundaries, state machine map.
- `workflow.md`: state machine analysis workflow, transition change template,
  boundary audit, validation commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for state machine change runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/adventure-state-machine-specialist/SKILL.md` (in Locksmith project)
- Hermes skill: `.hermes/skills/adventure-state-machine-specialist.md` (in Locksmith project)
- Hermes package: `.hermes/agents/adventure-state-machine-specialist/` (in Locksmith project)

## Routing

Pack routing keys in `packs/playdate-game-crew.yaml`:
- `adventure-state-machine: adventure-state-machine-specialist`
- `adventure-phase-lifecycle: adventure-state-machine-specialist`
- `adventure-state-transitions: adventure-state-machine-specialist`
- `story-flow: adventure-state-machine-specialist`
- `ad-state-machine: adventure-state-machine-specialist`
