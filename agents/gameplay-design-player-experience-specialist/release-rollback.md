# Release and Rollback — Gameplay Design / Player Experience Specialist

## Release Checklist

- [ ] agent-card.json valid JSON
- [ ] harness.yaml valid YAML
- [ ] All 14 A2A files present and non-empty
- [ ] Codex wrapper SKILL.md valid YAML frontmatter
- [ ] Hermes skill valid YAML frontmatter
- [ ] Hermes package instructions.md and manifest.yaml present
- [ ] playdate-game-crew.yaml routing updated
- [ ] No naming violations (game name is Locksmith, hero is Silas Crane)
- [ ] No image analysis references

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1.0 | 2026-07-05 | Initial draft creation |

## Current Status

**Draft** — harness files exist, wrappers exist, routing configured.
Not production until:
- Pilot run records (≥3 design proposals with user acceptance)
- Eval evidence (≥7/10 eval score)
- Independent agent-tester review

## Rollback

To rollback:
1. Remove routing keys from playdate-game-crew.yaml
2. Delete /root/agentic-crew/agents/gameplay-design-player-experience-specialist/
3. Delete project-local wrappers:
   - /root/locksmith/.agents/skills/gameplay-design-player-experience-specialist/
   - /root/locksmith/.hermes/skills/gameplay-design-player-experience-specialist.md
   - /root/locksmith/.hermes/agents/gameplay-design-player-experience-specialist/
4. Revert playdate-game-crew.yaml via git checkout if committed
