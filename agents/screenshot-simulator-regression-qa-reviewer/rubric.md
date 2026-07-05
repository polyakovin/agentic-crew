# Screenshot / Simulator Regression QA Reviewer Rubric

Use this rubric before handing off a screenshot QA review or visual regression report.

## Excellent

- All 20 canonical states captured or missing states explicitly documented.
- Contact sheet review covers text overlap, safe areas, control hints, and gameplay object occlusion for every state.
- Visual regression comparison cites specific screenshots (current and baseline) with clear description of changes.
- Adventure flow covers full story → gameplay → win cadence with per-state assessment.
- Simulator crash triaged: classification with evidence, last successful screenshot, recommended owner.
- Every finding cites exact screenshot name, severity, affected spec/source, and recommended owner.
- Overlap/safe-area findings reference layout contracts from `docs/renderer.md` and `docs/ui.md`.
- No vision-model analysis unless user explicitly requested.
- Game consistently called Locksmith (hero Silas Crane).
- Findings scoped to visual QA — layout issues handed to Pixel UI, copy to Narrative.
- QA verdict is evidence-based (clean / findings-present / blocked).
- Validation passed: JSON, YAML, trailing whitespace.
- Scoped changes committed and pushed.

## Acceptable

- Most canonical states captured; some gaps documented but not all 20 checked.
- Contact sheet reviewed but some safe-area or control-hint checks skipped.
- Visual regression comparison done but some changes described without severity.
- Adventure flow covered main phases but some story screen transitions not checked.
- Crash classified but evidence not fully documented (which PNGs exist).
- Findings cite screenshots but some miss spec references or ownership.
- Some overlap findings handoff to wrong specialist.
- Minor Safe Cracker references slipped through.

## Needs Revision

- Only a few screenshots reviewed instead of all canonical states.
- Contact sheet not assembled or not reviewed.
- Overlap/safe-area findings not backed by layout contracts.
- Visual regression comparison missing or compared against wrong baseline.
- Simulator crash not classified — just reported "it crashed".
- Findings lack screenshot names, severity, or ownership.
- Game occasionally referred to as Safe Cracker.
- Vision analysis of images performed without user request.
- QA verdict given without reviewing all required states.
- Handoff items too vague ("someone should fix the layout").
- Validation not run.

## Critical Failure

- Falsified or fabricated screenshot evidence.
- Claimed visual readiness without reviewing screenshots.
- Modified screenshots, contact sheets, or image files.
- Changed game code, specs, or assets to "fix" a visual finding.
- Renamed game to Safe Cracker.
- Ignored critical overlap that obscures primary game object.
- Analysed images with vision models after user said not to.
- Skipped validation and claimed complete.
- Committed or pushed unrelated dirty worktree changes.

## Challenge Checklist

- Are all 20 canonical states captured or their absence documented?
- Has the contact sheet been reviewed for overlap and safe-area violations?
- Is there a visual regression comparison against a valid baseline?
- Has the full Adventure flow been audited state-by-state?
- Is every simulator crash classified with evidence?
- Does every finding cite a specific screenshot and spec reference?
- Are layout issues handed to Pixel UI Specialist (not fixed in-place)?
- Are copy/tone issues handed to Narrative Specialist?
- Is the game consistently called Locksmith?
- Was vision-model analysis explicitly requested before use?
- What would make this QA review incomplete or unreliable?
- What evidence proves the QA verdict?
