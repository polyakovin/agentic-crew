# Audio / Music Feedback Specialist Evaluation Plan

## Seed Cases

### AM-001: SFX Volume Tuning
**Input:** User reports tick sound is barely audible over noir loop in Speedrun mode.
**Expected:** Proposal with old→new volume parameters, player feel description, volume mixing verification in all modes.
**Gate:** SFX volume > music volume, parametric change, boundary audit passes.

### AM-002: Music Playlist Lifecycle Audit
**Input:** User suspects shuffled bag repeats the same track too often.
**Expected:** Audit of playlist logic, shuffled bag implementation verification, observation over 5+ minutes.
**Gate:** No immediate repeat with 2+ tracks, proper reshuffle on exhaustion.

### AM-003: Music Toggle Verification
**Input:** User reports music toggle in Settings doesn't stop music immediately.
**Expected:** Trace the toggle path (Settings → Datastore → SoundFX), verify immediate effect, confirm SFX unaffected.
**Gate:** Music stops within one frame of toggle, SFX continue playing.

### AM-004: PDX Size Analysis — New Track
**Input:** User wants to add a new 45-second background loop as PCM.
**Expected:** PDX size impact analysis: PCM vs ADPCM size estimates, recommendation for ADPCM at 11025 Hz, before/after PDX size.
**Gate:** Size analysis with actual byte counts, format recommendation with rationale.

### AM-005: Crash Safety — Missing Asset
**Input:** Simulate missing catch.wav (temporarily rename, rebuild, run).
**Expected:** Verify nil checks in soundfx.lua, confirm game doesn't crash, document silent degradation.
**Gate:** No crash, diagnostic printed, other sounds unaffected.

### AM-006: Volume Mixing Across Modes
**Input:** Full audio audit: SFX vs music volume in Title, Speedrun, Adventure, Endless.
**Expected:** Per-mode volume measurements, any imbalances flagged with proposed fixes.
**Gate:** SFX > music in every mode, or documented exceptions with rationale.

### AM-007: Boundary Violation Detection
**Input:** Someone added `require("safe")` to soundfx.lua.
**Expected:** Boundary audit catches it, agent reports violation, proposes fix.
**Gate:** Violation detected and reported, no game rules in audio module.

### AM-008: Spec Update Before Code
**Input:** API change: SoundFX needs a new `setSFXVolume()` function.
**Expected:** Update `docs/soundfx.md` first, then implement, then verify.
**Gate:** Spec diff before code diff, API documented before implementation.

### AM-009: ADPCM Conversion
**Input:** bazaar_loop.wav is 450 KB as PCM — user wants it converted.
**Expected:** Conversion proposal with old→new format, estimated size reduction, PDX size before/after.
**Gate:** Format change documented, size reduction calculated, quality trade-offs noted.

### AM-010: Finish Callback Fallback
**Input:** User reports music track transition is broken — update() fallback not working.
**Expected:** Audit finish callback vs update() fallback lifecycle, propose fix if broken.
**Gate:** Track transitions work with or without finish callback support.
