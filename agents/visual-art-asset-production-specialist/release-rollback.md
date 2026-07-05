# Visual Art / Asset Production Specialist Release And Rollback

## Status

Draft package. Not production until pilot records and agent-tester review exist.

## Release Blockers

- Agent Card JSON missing or invalid.
- Harness YAML missing or invalid.
- Prompt uses unsafe lockpicking instruction phrasing.
- Master art not saved to `drafts/` before conversion.
- Pipeline bypassed — procedural fallback used instead of script conversion.
- No preview/contact sheet generated or reviewed.
- Bounding boxes show neighbor-cell leaks.
- Transparent padding below contract.
- Ornaments or functional elements clipped.
- Dithering produces large non-informative black blobs.
- Wrong dimensions for asset type.
- Title/launcher art has AI-rendered text instead of composited logo.
- Lock background shifts geometry anchors.
- Manifest not updated before runtime references new asset.
- Game renamed to Safe Cracker.
- Production assets overwritten without QA and user approval.
- QA evidence missing or falsified.
- Commit/push result missing or falsely reported.

## Rollback Scope

Rollback may restore:

- `agents/visual-art-asset-production-specialist/`
- `.hermes/agents/visual-art-asset-production-specialist/`
- `.hermes/skills/visual-art-asset-production-specialist.md`
- `.agents/skills/visual-art-asset-production-specialist/SKILL.md`
- affected entries in `packs/playdate-game-crew.yaml`
- generated assets in `drafts/`, `source/images/`, `source/launcher/` (only if explicitly part of the bad change)

Rollback owner: orchestrator.
Release gate owner: user for asset replacement; protocol steward for harness.

## Rollback Steps

1. Identify the bad art production change.
2. Capture the failure as a learning/eval case.
3. Revert only affected packaging files, routing entries, and generated assets.
4. Do not revert unrelated dirty user changes.
5. Do not revert committed game code, specs, or unrelated assets.
6. Re-run JSON/YAML/trailing-whitespace checks.
7. Verify surviving assets still pass QA on white background.
8. Update health snapshot if needed.

## Smoke Release Checks

```bash
python3 -m json.tool agents/visual-art-asset-production-specialist/agent-card.json
python3 -m json.tool agents/visual-art-asset-production-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/visual-art-asset-production-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/visual-art-asset-production-specialist
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.
