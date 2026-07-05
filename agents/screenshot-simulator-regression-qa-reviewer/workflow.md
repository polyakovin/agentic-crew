# Screenshot / Simulator Regression QA Reviewer Workflow

## Boot Probe

Run before any screenshot QA or visual audit task:

```bash
git status --short
ls build/screenshots/ build/screenshots_review/ build/adventure_screenshots/ 2>&1
ls build/screenshot_src/main.lua build/adventure_screenshot_src/main.lua 2>&1
cd /root/locksmith && make build 2>&1 | tail -10 || true
```

Interpretation:

- Dirty worktrees: note but do not block visual audit (we don't modify code).
- Missing screenshot directories: flag as evidence gap.
- Missing screenshot harness: flag as capability gap — cannot capture new screenshots.
- `make build` failure: document; if Locksmith.pdx cannot be built, new screenshots cannot be captured via harness.

## Core Workflow

1. Read `AGENTS.md` → `docs/test-scenarios.md` → `docs/visual-reference-pack.md`.
2. Inventory existing screenshots: `ls build/screenshots/ build/adventure_screenshots/`.
3. Check screenshot harness availability: `ls build/screenshot_src/main.lua`.
4. Run `make build` to confirm PDX buildability.
5. If screenshots need capturing and harness exists, invoke the harness.
6. Identify which canonical states are captured and which are missing.
7. For each captured screenshot, check: overlap, safe-area, control clipping, HUD occlusion.
8. Compare against baseline captures if available — identify visual regressions.
9. Audit Adventure flow: story screens → gameplay phases → win.
10. If simulator crashed, classify: harness bug, game bug, or environment issue.
11. Document findings with screenshot evidence and spec references.
12. Compile QA report with verdict and handoff items.

## Screenshot Sweep

Execute when a full screenshot sweep is needed:

1. Confirm `make build` succeeds and `Locksmith.pdx` exists.
2. Run `build/screenshot_src/main.lua` through the simulator to capture all runtime states.
3. Run `build/adventure_screenshot_src/main.lua` to capture Adventure flow.
4. Verify all expected PNG files exist in `build/screenshots/` and `build/adventure_screenshots/`.
5. Check for the all-states contact sheet: `build/screenshots/00_all_states_contact_sheet.png`.
6. Check for the design-review contact sheet: `build/screenshots_review/00_design_review_contact_sheet.png`.
7. Check for the Adventure contact sheet: `build/adventure_screenshots/00_adventure_contact_sheet.png`.
8. Report which states are captured, which are missing, and any harness errors.

## Contact Sheet Review

For each contact sheet, systematically check:

### Text Overlap

- [ ] Story captions (line chips) do not overlay gameplay artwork.
- [ ] Feedback text (MISS!, +1s, NO COINS, MAXED) in `feedbackLane` does not obscure selected rows, dial targets, or bottom controls.
- [ ] HUD status text (top bar) stays within top bar, not overlapping scene-critical labels.
- [ ] Timer/score text is readable against its background.

### Safe Areas

- [ ] Primary game object (lock, safe dial, clock face) has clear space around it — not clipped by HUD, controls, or feedback.
- [ ] Bottom controls / hints visible above screen bottom edge.
- [ ] Title screen mode rows vertically centered in dark area (y=102..239).
- [ ] Story title strip at top-right, 7px below screen top, flush with right edge.
- [ ] Story prompt badge at bottom-right, above screen bottom.

### Control Hints

- [ ] A/B button overlays do not obscure critical gameplay elements.
- [ ] Pin hint (crank + A button) visible on first two pins only (TC-118b).
- [ ] Speedrun/Endless modes suppress visual input hints as specified.
- [ ] Adventure gameplay scenes do not show header/footer control hints.

### Gameplay Object Occlusion

- [ ] Lock rendering (pins, springs, pick, shear line) not obscured by HUD or feedback.
- [ ] Safe dial and stethoscope visible, dial not covered by text.
- [ ] Clock face and hands readable, minute tick marks not clipped.
- [ ] Endless preview card has proper padding, image not clipped, BUY button visible.
- [ ] Endless tabs and scrollbar do not overlap row text.

## Visual Regression Check

When comparing current screenshots against baseline:

1. Identify the most recent baseline: previous commit's screenshots or a tagged baseline.
2. For each canonical state, compare current vs baseline:
   - Layout shifts: did elements move?
   - New occlusion: did new UI elements overlap existing ones?
   - Missing elements: did anything disappear?
   - Contrast changes: is text still readable?
   - Dithering artifacts: did conversion introduce noise?
3. Document each regression with:
   - Screenshot name (both current and baseline).
   - Description of the change.
   - Severity: critical / warning / info.
   - Affected spec/source owner.
4. If no baseline exists, flag the gap and create one from current captures.

## Overlap / Safe-Area Check

Run for every screenshot sweep or after UI/rendering changes:

1. Load each screenshot (file existence, dimensions — do NOT vision-analyse unless user requests).
2. Cross-reference against layout contracts in `docs/renderer.md` and `docs/ui.md`:
   - Shear line position: (17,119) → (360,119).
   - Pin tunnel area defined by shaft slots.
   - Safe dial center: (269,117), radius ~74.
   - Clock face center: (200,118), radius 50.
   - Title dark area: y=102..239.
3. Check for the design-review contact sheet (`build/screenshots_review/00_design_review_contact_sheet.png`) — this shows top/content/feedback/bottom/margin outlines for changed screens (TC-076e).
4. Flag any overlap, clipping, or safe-area violation.

## Adventure Flow Review

Audit the full Adventure story → gameplay → win cadence:

1. Story arrival: art-only background for 1000ms, then text chips appear.
2. Story intro (Verity Hale): background + captions, title strip at top-right.
3. Story hero (Silas / painting): background + captions, advances to curtain_lock.
4. Story curtain_lock (The Canvas Lock): close-up lock art, advances to gameplay.
5. Phase1_curtain gameplay: clean dynamic background, pins visible, no story copy.
6. Curtain cleared: roll-up scene, no floating-arrow info_curtain.
7. Story safe_dial → safe dial gameplay: stethoscope scene, dial readable.
8. Story clock → clock gameplay: clock face, hands, no TARGET H:MM hint (TC-118g).
9. Story win → win screen: A: Title prompt at bottom-center, no story copy over art.

Check each against TC-118h through TC-118j5 in `docs/test-scenarios.md`.

## Crash Triage

When the simulator crashes during screenshot capture:

1. Check which PNG files exist in the output directories:
   ```bash
   ls -la build/screenshots/*.png build/adventure_screenshots/*.png 2>&1
   ```
2. Identify the last successfully captured screenshot — this is where the crash occurred.
3. Check screenshot harness output for error markers.
4. Classify the crash:

   | Classification | Evidence | Owner |
   |----------------|----------|-------|
   | Harness bug | Harness script error, all PNGs missing, Lua syntax error in harness | `toolchain-build-sandbox-gatekeeper` or harness author |
   | Game bug | Specific state triggers crash, earlier states captured successfully | `gameplay-design-player-experience-specialist` or relevant mode specialist |
   | Environment issue | Simulator not found, `make build` fails, missing SDK, disk full | `toolchain-build-sandbox-gatekeeper` |
   | Unknown | Cannot determine from available evidence | Document as blocker, request simulator access |

5. Do NOT declare the harness failed based on crash alone — check whether expected PNGs exist and open first.
6. Document the crash classification, evidence, and recommended owner.

## Validation Pack

Run from the Agentic Crew repository root:

```bash
python3 -m json.tool agents/screenshot-simulator-regression-qa-reviewer/agent-card.json
python3 -m json.tool agents/screenshot-simulator-regression-qa-reviewer/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/screenshot-simulator-regression-qa-reviewer/harness.yaml'))"
rg -n "[[:blank:]]$" agents/screenshot-simulator-regression-qa-reviewer
```

Also verify from Locksmith root when Hermes wrappers exist:

```bash
cd /root/locksmith && python3 -c "import yaml; yaml.safe_load(open('.hermes/agents/screenshot-simulator-regression-qa-reviewer/manifest.yaml'))"
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state in each affected repository.
2. Stage only scoped changes.
3. Commit with an atomic message that names the agent.
4. Push the current branch.
5. Record commit SHA, branch, remote, and push result.

Block instead of committing when validation failed or unrelated dirty files would be staged.
