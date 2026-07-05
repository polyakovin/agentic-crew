# Screenshot / Simulator Regression QA Reviewer Eval Plan

## Purpose

Evaluate whether the Screenshot / Simulator Regression QA Reviewer correctly
captures or audits screenshots, reviews contact sheets for overlap/safe-area
regressions, compares visual before/after states, audits Adventure flow, and
triages simulator crashes for Locksmith.

## Metrics

- Screenshot sweep completeness (canonical states coverage).
- Contact sheet review thoroughness (overlap, safe-area, control hints, occlusion).
- Visual regression detection accuracy.
- Adventure flow audit completeness.
- Crash classification correctness.
- Finding specificity (screenshot name, severity, spec reference, owner).
- Handoff correctness (layout → Pixel UI, copy → Narrative, mechanics → Gameplay Design).
- Game naming consistency (Locksmith, not Safe Cracker).
- Vision-model usage restraint.
- Validation honesty.
- Commit/push completion.

## Seed Cases

### SR-001: Full Screenshot Sweep

Input: Locksmith has a fresh build. Run a full screenshot sweep across all
20 canonical states and produce a QA report.

Expected:

- All 20 canonical states attempted via screenshot harness.
- Missing states documented with reason (harness not available, simulator not accessible, specific state causes crash).
- Contact sheet reviewed for every captured state.
- Overlap, safe-area, control-hint, and occlusion checks performed.
- Findings cite specific screenshots and layout contracts.
- QA verdict: clean / findings-present / blocked.

### SR-002: Visual Regression After UI Change

Input: A commit changed `ui.lua` — the title screen menu layout shifted.
Compare current screenshots against baseline.

Expected:

- Baseline identified (previous commit screenshots or tagged baseline).
- Current vs baseline comparison for title screen states.
- Layout shift described: which elements moved, by how much.
- Overlap check: did the shift cause new occlusion?
- Finding cites specific screenshots, spec references, and recommends Pixel UI Specialist.
- If no baseline exists, gap documented and new baseline created.

### SR-003: Adventure Flow Audit

Input: Adventure mode was refactored. Audit the full story → gameplay → win
screenshots.

Expected:

- Every Adventure state checked: arrival, intro, hero, curtain_lock, phase1_curtain, curtain_cleared, safe_dial, clock, win.
- Story screens checked for: background, title strip placement, caption chips, prompt badge.
- Gameplay screens checked for: no story copy over art, pin hints on first two pins, clean backgrounds.
- Win screen checked for: A: Title prompt visible, no story copy over art.
- No `info_curtain` floating-arrow scene present.
- Each finding cites TC number from `docs/test-scenarios.md`.

### SR-004: Overlap Detection

Input: Endless mode added a new stat row. Check for overlap/safe-area violations.

Expected:

- All Endless screenshots reviewed: flea market, inventory, picking, crafting, achievements, stats.
- Preview card padding and BUY button positioning checked.
- Tab icons and scrollbar checked for row text overlap.
- Selected-row A-button marker checked for vertical alignment.
- Feedback text checked for row/dial/target occlusion.
- Any overlap documented with screenshot evidence and layout contract reference.

### SR-005: Simulator Crash Triage

Input: The simulator crashed during a screenshot sweep after a code change.
Diagnose and classify.

Expected:

- Checked which PNG files exist in build/screenshots/.
- Identified the last successfully captured screenshot — documents the crash point.
- Classified the crash: harness bug, game bug, or environment issue.
- Evidence cited: which PNGs exist, error output, harness source line.
- Recommended owner for the fix.
- Does NOT declare the harness failed based on crash alone.

### SR-006: Missing Screenshot Harness

Input: Locksmith does not have a screenshot harness (`build/screenshot_src/main.lua` missing).
User requests visual QA.

Expected:

- Documents the harness gap.
- Lists available screenshots from existing `build/screenshots/`.
- Reviews existing screenshots for overlap/safe-area (static review).
- Cannot run a sweep — documents the blocked capability.
- Does NOT attempt to create or modify the harness.

### SR-007: Baseline Management

Input: No baseline screenshots exist. User wants regression checking enabled
for future changes.

Expected:

- Documents the baseline gap.
- Suggests capturing current screenshots as initial baseline.
- Explains what a baseline should contain: all 20 canonical states.
- Does NOT generate or modify screenshots to create a baseline.
- Hands off any screenshot capture needs (if harness missing).

### SR-008: Handoff to Pixel UI Specialist

Input: Screenshot shows pin tunnel overlap with HUD text in Speedrun mode.

Expected:

- Cites the specific screenshot.
- References the layout contract from `docs/renderer.md`.
- Hands off with specific finding: which element overlaps which, at which coordinates.
- Does NOT attempt to fix the layout in renderer.lua.
- Does NOT hand off to the wrong specialist.

### SR-009: Handoff to Narrative Specialist

Input: Adventure story caption has a typo visible in screenshot.

Expected:

- Cites the specific screenshot showing the typo.
- Documents which story key/line has the issue.
- Hands off to Narrative / Noir Copy Specialist.
- Does NOT fix the text in `ad_story_data.lua`.

### SR-010: Design Review Contours (TC-076e)

Input: UI changes were made. Check the design-review contact sheet.

Expected:

- Checks `build/screenshots_review/00_design_review_contact_sheet.png`.
- Verifies that only changed screens have top/content/feedback/bottom/margin outlines.
- Confirms outlines do not overlap or obscure the review content.
- Documents any contour harness issues.

## Promotion Threshold

Move from `draft` to `draft-ready` after:

- all harness files exist and validate;
- Hermes and Codex wrappers created in both repos;
- routing added to playdate-game-crew.yaml.

Move to `pilot` after:

- at least one real screenshot sweep executed for Locksmith;
- at least one visual regression comparison with user feedback;
- at least one simulator crash triaged and classified.

Move to `production` after:

- two successful Locksmith screenshot sweeps with contact-sheet review;
- zero critical overlap/regression findings missed;
- agent-tester review passed.
