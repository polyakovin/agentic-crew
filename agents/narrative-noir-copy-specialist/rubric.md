# Narrative / Noir Copy Specialist Rubric

Use this rubric before handing off a copy audit or text change.

## Excellent

- Copy problem is clearly stated: what is wrong and why.
- All text changes documented: old → new, rationale per item.
- Tone justification references `docs/lore.md` — dieselpunk 1920s noir,
  Silas Crane voice.
- All text verified ASCII-only (checked with `[^\x00-\x7F]` regex).
- Length fit for Playdate 400×240: Fonts.drawTextAligned with opaque background
  confirmed.
- Spec (`docs/lore.md` or `docs/ui.md`) updated before data file when
  terminology/convention changes.
- Canon-consistency audit run: cross-file references checked, no drift.
- `make test-all`, `make lint`, `make build`, `make check` all pass.
- Report includes: canon check results, tone justification, length fit,
  affected files, residual risks, handoff if needed.
- AGENTS.md updated if new copy/text pitfall discovered.
- Small, targeted change — not rewritten prose.

## Acceptable

- Copy problem stated but vague.
- Text changes documented but tone rationale thin.
- ASCII check done but length fit not verified on Simulator.
- Spec updated after data or rationale not fully explained.
- `make test-all` passes but no manual screen-fit check.
- Risks mentioned but handoff not considered.
- One minor canon reference missed (non-critical).

## Needs Revision

- No copy problem stated — "changed X to Y" with no why.
- Tone justification missing or says "sounds better" without noir reference.
- Spec not updated when terminology/convention changed.
- Unicode/emoji found in text (Playdate font failure risk).
- Text obviously too long for 400×240 screen with opaque background.
- `make check` not run or fails unreported.
- Text change breaks existing tests without test update.
- Marketing tone in in-game string.
- Silas Crane voice broken (cheerful, chatty, modern).
- Broad rewrite instead of targeted tone fix.

## Critical Failure

- Renames game to Safe Cracker.
- Adds Unicode/emoji/smart quotes to text strings.
- Adds `gfx.*` / `playdate.*` to declarative data files.
- Adds game logic to data files disguised as "copy flavour."
- Changes economy numbers/pricing/balance under guise of "copy."
- Changes renderer.lua or font calls under guise of "copy fit."
- Adds marketing/public-facing copy to in-game strings.
- Commits changes without validation.
- Overwrites unrelated user changes.
- Claims "tone improvement" but Silas Crane sounds like a Marvel hero.

## Challenge Checklist

- What is the copy problem in one sentence?
- Which text string(s) changed, and why?
- Does the new text sound like dieselpunk 1920s noir?
- Is Silas Crane's voice terse, professional, weary?
- Is every character ASCII?
- Does the text fit 400×240 with opaque background?
- Is the loreLine one tight sentence — atmospheric, not expository?
- Is the canon consistent across all files?
- Is the spec updated before data when terminology changed?
- Does `make check` pass?
- Are existing tests still passing?
- Is this the smallest change that fixes the tone?
- Would the player feel the noir atmosphere?
- Are residual risks documented (UI-fit edge cases, ambiguous tone)?
- Is the handoff clear if another specialist is needed?
