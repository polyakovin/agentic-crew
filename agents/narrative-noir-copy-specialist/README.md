# Narrative / Noir Copy Specialist

Draft portable harness for narrative tone, copywriting, item naming, loreLine
design, and canon-consistency auditing for the Playdate game Locksmith.

Status: draft.

This specialist owns:

- In-game text tone: dieselpunk 1920s noir voice, Silas Crane character voice.
- Item names, purpose strings, loreLine blurbs across all data files.
- Achievement names, labels, and GUILD_RANK tier names.
- Catalog entry names and loreLine (LOCK_CATALOG, SAFE_CATALOG, CLOCK_CATALOG).
- UI/HUD screen copy (timer, gameover, leaderboard, button labels, feedback).
- ASCII-safety enforcement: no Unicode, emoji, smart quotes in any text.
- Playdate 400×240 width-fit with Fonts.drawTextAligned and opaque background.
- Canon-consistency: same characters/places/concepts named consistently
  across all text sources.
- Spec-first copy workflow: `docs/lore.md` updated before text in data files.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, ownership boundaries, text source inventory.
- `workflow.md`: copy audit workflow, change template, canon check, validation.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for copy change runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/narrative-noir-copy-specialist/SKILL.md`
  (in Locksmith project)
- Hermes skill: `.hermes/skills/narrative-noir-copy-specialist.md`
  (in Locksmith project)
- Hermes package: `.hermes/agents/narrative-noir-copy-specialist/`
  (in Locksmith project)

## Routing

Pack routing key: `narrative-noir-copy: narrative-noir-copy-specialist`
(in `packs/playdate-game-crew.yaml`)
