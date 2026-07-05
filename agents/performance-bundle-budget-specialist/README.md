# Performance Bundle Budget Specialist

Draft portable harness for performance analysis, memory allocation audit,
bundle-size budget tracking, and cache strategy review for the Locksmith
Playdate game.

Status: draft.

This specialist owns:

- Allocation hot path analysis: draw/update paths audited for per-frame
  table creation, `gfx.image.new`, `cos`/`sin` calls, `pcall`, unbounded loops.
- Market pool cache review: dirty-flag-driven rebuilds verified, per-frame
  rebuild patterns flagged.
- PDX size budget: compiled bundle size tracked and broken down by category
  (images, audio, code).
- Asset size audit: PNG image sizes reviewed for optimization, audio
  tracks checked against compression budget.
- Audio compression budget: background loops IMA ADPCM mono 11025 Hz
  (~120 KB/track), total playlist ~500 KB; short SFX PCM 16-bit mono
  22050 Hz.
- Cache strategy recommendations: memoized geometry, pre-computed tables,
  dirty-flag-driven rebuilds.
- Performance comfort: static analysis for 30 fps minimum sustained,
  no GC stutter in draw.
- Proposal review: flagging new allocation/draw calls/assets that
  would regress performance.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, ownership boundaries, hot path inventory,
  duplicate role check.
- `workflow.md`: performance analysis workflow, allocation template, PDX budget
  checklist, asset size audit, audio compression audit, cache strategy review,
  boundary audit commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and quality checklist.
- `eval-plan.md`: 10 seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for performance audit runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Related Wrappers

- Codex: `.agents/skills/performance-bundle-budget-specialist/SKILL.md` (in Locksmith project)
- Hermes skill: `.hermes/skills/performance-bundle-budget-specialist.md` (in Locksmith project)
- Hermes package: `.hermes/agents/performance-bundle-budget-specialist/` (in Locksmith project)

## Routing

Pack routing keys:
- `performance: performance-bundle-budget-specialist`
- `bundle-budget: performance-bundle-budget-specialist`
- `allocation-risk: performance-bundle-budget-specialist`
- `asset-size: performance-bundle-budget-specialist`
- `audio-compression: performance-bundle-budget-specialist`
- `pdx-growth: performance-bundle-budget-specialist`
- `draw-perf: performance-bundle-budget-specialist`
- `cache-strategy: performance-bundle-budget-specialist`

(in `packs/playdate-game-crew.yaml`)
