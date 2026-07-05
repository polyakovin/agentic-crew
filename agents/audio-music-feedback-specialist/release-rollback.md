# Audio / Music Feedback Specialist Release & Rollback

## Release Checklist (Agent Infrastructure)

- [ ] All 13 A2A harness files present and valid
- [ ] JSON/YAML/trailing whitespace checks pass
- [ ] Pack routing updated with all routing keys
- [ ] Codex wrapper exists (SKILL.md with non-goals)
- [ ] Hermes skill exists (non-goals section present)
- [ ] Hermes package exists (manifest.yaml + instructions.md)
- [ ] manifest.yaml quality_gates >= 8, forbidden_actions >= 6
- [ ] manifest.yaml paths resolve (4 levels up for agentic-crew, 2 levels up for skills)
- [ ] Agent Tester review recorded
- [ ] Agent Tester HIGH findings resolved
- [ ] Git committed and pushed (agentic-crew + locksmith)

## Release Checklist (Audio Artifact)

- [ ] Audio problem is specific, measurable, player-relevant
- [ ] Every parameter change has old→new values with rationale
- [ ] soundfx.lua boundary audited: audio-only (no game rules, UI, persistence, modes)
- [ ] music_data.lua boundary audited: declarative-only (no gfx.*, playdate.*, persistence, state)
- [ ] Crash safety verified: nil checks on all sampleplayer/fileplayer usage
- [ ] Volume mixing verified: SFX > music in all modes
- [ ] PDX size impact documented if assets changed
- [ ] Spec updated before code when API/contract/behaviour changed
- [ ] `make check` evidence attached
- [ ] AGENTS.md Pitfalls updated for audio errors
- [ ] Game = Locksmith, hero = Silas Crane

## Rollback Plan (Agent Infrastructure)

1. Revert commit in agentic-crew: `git revert <hash>`
2. Revert commit in locksmith: `git revert <hash>`
3. Remove routing keys from playdate-game-crew.yaml
4. Run validation: JSON/YAML/path checks
5. If deployed: remove Hermes package from .hermes/agents/

## Rollback Plan (Audio Artifact)

1. Revert `source/soundfx.lua` to previous version: `git checkout <hash> -- source/soundfx.lua`
2. Revert `source/music_data.lua` if changed.
3. Revert `docs/soundfx.md` if changed.
4. Restore audio assets if replaced: `git checkout <hash> -- source/sounds/`
5. Rebuild: `make clean && make build`
6. Verify: `make check`
7. Document reason for rollback in AGENTS.md Pitfalls.
