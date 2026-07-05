# Persistence / Save Migration Specialist Health Snapshot

Status: draft.
Version: 0.1.0.

## Present

- A2A Agent Card.
- Split operational docs: role, entrypoint, source-map, workflow, tool-policy.
- Rubric and eval plan with 10 seed cases (PM-001 through PM-010).
- Run-record template with migration-specific fields.
- Release/rollback checklist for both agent infrastructure and Locksmith persistence.
- Codex wrapper at `.agents/skills/persistence-save-migration-specialist/SKILL.md`.
- Hermes skill at `.hermes/skills/persistence-save-migration-specialist.md`.
- Hermes package at `.hermes/agents/persistence-save-migration-specialist/`.
- Pack routing for save-migration/schema-integrity/nil-safety/corrupt-recovery keys
  in `playdate-game-crew.yaml`.

## Known Gaps

- No live A2A runtime endpoint is deployed; endpoint URL is a placeholder.
- Hermes package shape is portable and generic; validate against concrete
  Hermes runtime before production use.
- No pilot run records exist.
- No eval seed cases have been executed.
- No real migration audit has been run on Locksmith save schema.
- Nil-safe migration coverage not yet verified for all 50+ fields.
- Corrupt data fallback scenarios not yet validated with actual datastore errors.
- Legacy key fallback (`infinite_economy` → `endless_economy`) not yet
  independently verified.
- Bounded write audit not yet completed on current codebase.
- Schema versioning not yet proposed or implemented.
- Promotion requires pilot run records and agent-tester review.

## Save Schema Health (not yet audited)

| Surface | Status | Notes |
|---------|--------|-------|
| Datastore (highscores) | not audited | 9 tests exist, not independently reviewed |
| Datastore (adventure flag) | not audited | nil-safe via normalize functions |
| Datastore (music prefs) | not audited | defaults to true |
| Datastore (dev mode) | not audited | gated, clears children on disable |
| EndlessSave (50+ tier keys) | not audited | migrateSaveData covers nil + type guard |
| EndlessSave (legacy fallback) | not audited | infinite_economy fallback tested |
| EndlessSave (corrupt recovery) | not audited | non-table → defaults |
| EndlessSave (partial save) | not audited | test case 7 validates |
| Main dispatch (save lifecycle) | not audited | savePausedEndlessMode on Settings open |

## Test Suite Health (not yet run)

| Suite | Tests | Status |
|-------|-------|--------|
| test_datastore.lua | 9 | not run |
| test_endless_save.lua | 9 | not run |

## Next Health Checks

- [ ] Run `make test-all` — verify datastore + endless_save suites pass.
- [ ] Run seed evals from `eval-plan.md` (PM-001 through PM-010).
- [ ] Run full nil-safe migration audit: all 50+ Endless save fields.
- [ ] Validate legacy key fallback chain.
- [ ] Validate corrupt data fallback with actual malformed saves.
- [ ] Complete bounded write audit: classify all write call sites.
- [ ] Propose schema versioning (`saveVersion` field + MIGRATIONS table).
- [ ] Dispatch Agent Tester review.
- [ ] Document any discovered persistence pitfalls in AGENTS.md.
