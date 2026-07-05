# Devlog / Community Marketing Specialist Release & Rollback

## Release Checklist (Agent Infrastructure)

- [ ] All 13 A2A harness files present and valid
- [ ] JSON/YAML/trailing whitespace checks pass
- [ ] Pack routing updated with all routing keys
- [ ] Codex wrapper exists (SKILL.md with non-goals)
- [ ] Hermes skill exists (non-goals section present)
- [ ] Hermes package exists (manifest.yaml + instructions.md)
- [ ] manifest.yaml quality_gates >= 8, forbidden_actions >= 6
- [ ] manifest.yaml paths resolve (4 levels up)
- [ ] Agent Tester review recorded
- [ ] Agent Tester HIGH findings resolved
- [ ] Git committed and pushed (agentic-crew + locksmith)

## Release Checklist (Copy Artifact)

- [ ] Devlog post matches devlog/template.md format
- [ ] Every claim backed by spec reference and build evidence
- [ ] No technical jargon or variable names
- [ ] Game = Locksmith, hero = Silas Crane
- [ ] Legacy URL handled as migration, not rename
- [ ] In-game tone/copy handed to Narrative Noir Copy Specialist
- [ ] Visual asset needs documented for Visual Art Specialist
- [ ] User has approved draft before any posting
- [ ] Residual risks documented (unverified claims, simulator-needed checks)

## Rollback Plan (Agent Infrastructure)

1. Revert commit in agentic-crew: `git revert <hash>`
2. Revert commit in locksmith: `git revert <hash>`
3. Remove routing keys from playdate-game-crew.yaml
4. Run validation: JSON/YAML/path checks
5. If deployed: remove Hermes package from .hermes/agents/

## Rollback Plan (Copy Artifact)

1. Revert data/spec changes if any were made (unlikely for marketing-only copy)
2. Save draft to drafts/ for future use
3. Notify user: draft available but not published
