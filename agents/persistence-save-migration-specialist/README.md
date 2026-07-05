# Persistence / Save Migration Specialist

Draft A2A harness for the Locksmith persistence and save migration specialist.

## Quick Reference

- **Mission**: Own datastore and save-schema integrity. Nil-safe migration,
  corrupt/partial save recovery, legacy key fallbacks, bounded writes, schema
  versioning.
- **Agent Card**: `agent-card.json`
- **Role**: `role.md`
- **Workflow**: `workflow.md`
- **Eval Plan**: `eval-plan.md` (10 seed cases: PM-001 through PM-010)
- **Status**: draft

## Files

| File | Purpose |
|------|---------|
| harness.yaml | A2A harness config |
| agent-card.json | A2A Agent Card |
| role.md | Full role definition |
| entrypoint.md | Activation entrypoint |
| source-map.md | Source hierarchy and boundaries |
| workflow.md | Workflow, templates, checklists |
| tool-policy.md | Allowed/forbidden actions |
| rubric.md | Self-review rubric |
| eval-plan.md | 10 seed evaluation cases |
| run-record.template.json | Run record template |
| release-rollback.md | Release and rollback procedures |
| health-snapshot.md | Agent health and known gaps |

## Wrappers

- **Codex**: `/root/locksmith/.agents/skills/persistence-save-migration-specialist/SKILL.md`
- **Hermes**: `/root/locksmith/.hermes/agents/persistence-save-migration-specialist/`
- **Hermes skill**: `/root/locksmith/.hermes/skills/persistence-save-migration-specialist.md`

## Routing Keys

In `playdate-game-crew.yaml`:
- `save-migration`
- `schema-integrity`
- `nil-safety`
- `corrupt-recovery`
- `legacy-fallback`
- `bounded-writes`
- `datastore-schema`
- `endless-save-schema`
- `save-versioning`
- `migration-audit`
