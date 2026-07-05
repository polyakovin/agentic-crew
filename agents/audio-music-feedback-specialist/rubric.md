# Audio / Music Feedback Specialist Rubric

Use this rubric before handing off an audio change proposal or audit.

## Excellent

- Audio problem is specific, measurable, and player-relevant.
- Every parameter change has old→new values with rationale and player feel.
- soundfx.lua boundary audited and confirmed: no game rules, UI refs,
  mode imports, persistence reads.
- music_data.lua boundary audited: no gfx.*, no playdate.*, no persistence,
  no lifecycle state mutations.
- Crash safety verified: nil checks present, missing audio degrades
  gracefully.
- Volume mixing verified: SFX > music in all modes, with measurements.
- PDX size impact documented if audio assets changed: before/after sizes,
  format changes, total PDX size delta.
- Spec updated before code when API/contract/behaviour changed.
- `make check` evidence included (with known_failure_carveout noted).
- AGENTS.md Pitfalls updated for any discovered audio error.
- All evidence attached: build output, asset sizes, volume parameters.

## Acceptable

- Audio problem identified but could be more specific.
- Parameter changes proposed but one value lacks clear rationale.
- Boundary audit done but one check skipped (e.g., mode imports checked
  but persistence reads not explicitly verified).
- Crash safety checked but edge case (fileplayer nil + sampleplayer nil
  simultaneously) not tested.
- Volume mixing checked in one mode but not all.
- PDX size impact mentioned but before/after numbers not both provided.
- Spec updated but one minor detail (asset format note) missed.
- `make check` run but output not attached.

## Needs Revision

- Audio problem is vague ("SFX could be better").
- No old→new parameter values — just "increase volume" without numbers.
- Boundary audit not done — soundfx.lua might now import a mode module.
- Nil checks missing from a play function.
- Music is louder than SFX ("it sounds fine" without measurement).
- PDX size not checked after adding new audio assets.
- Spec not updated but code changed.
- `make check` not run.
- Game referred to as Safe Cracker.
- Broad refactor proposed instead of targeted change.

## Critical Failure

- Renames game to Safe Cracker.
- Adds game logic or mode imports to soundfx.lua.
- Adds gfx.* or playdate.* calls to music_data.lua.
- Makes SFX quieter than music.
- Removes nil checks — game crashes on missing audio.
- Modifies safe.lua, renderer.lua, ui.lua, or input.lua for audio reasons.
- Falsifies build evidence (claims `make check` passes when it doesn't).
- Ignores explicit user instruction.
- Commits audio changes without review.

## Challenge Checklist

- What is the specific audio problem? (Measurable, player-relevant)
- What are the old parameter values?
- What are the new parameter values?
- What is the rationale for each change?
- What does the player hear/feel differently?
- Is soundfx.lua still audio-only? (no game rules, UI, persistence, modes)
- Is music_data.lua still declarative? (no gfx.*, playdate.*, persistence, state)
- Do nil checks protect every sampleplayer/fileplayer usage?
- Is SFX volume > music volume in every mode?
- Has PDX size been analysed if assets changed?
- Is the spec updated before code?
- Does `make check` pass (or known_failure_carveout documented)?
- Is the game called Locksmith (hero: Silas Crane)?
- Are AGENTS.md Pitfalls updated for audio errors?
- Would the player notice the difference?
