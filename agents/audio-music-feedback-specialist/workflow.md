# Audio / Music Feedback Specialist Workflow

## Boot Probe

Run before any audio change:

```bash
git status --short
cd /root/locksmith && make check 2>&1 | tail -10 || true
ls -la source/sounds/ | head -20
wc -c source/sounds/*.wav 2>/dev/null | tail -5
```

Interpretation:

- Dirty worktrees: note but don't block — audio changes don't modify
  non-audio code.
- Failing `make check`: flag but don't block unless the failing module
  is soundfx itself.
- Audio asset sizes: baseline for PDX size impact analysis.

## Core Workflow

1. **Read project context**: AGENTS.md → `docs/manifest.json` →
   `docs/soundfx.md` → `docs/playdate-best-practices.md`.
2. **Read audio sources**: `source/soundfx.lua`, `source/music_data.lua`.
3. **Audit audio assets**: `source/sounds/` — file formats, sizes,
   presence.
4. **Formulate audio problem**: specific, measurable, player-relevant.
   "Tick volume is -12 dB below music in Speedrun mode" — not "SFX
   could be louder."
5. **Propose change**: old → new parameters with rationale and player
   feel description.
   - Volume: current dB/level → target dB/level, why.
   - Timing: current ms/frames → target ms/frames, why.
   - Format: current codec/rate → target codec/rate, why.
   - Lifecycle: current behaviour → target behaviour, why.
6. **Update spec first** if API / contract / behaviour changes.
   `docs/soundfx.md` is the source of truth.
7. **Implement minimal change** respecting audio-only boundary.
8. **Verify**: `make test-all` → `make lint` → `make build` → `make check`.
9. **Report** with all evidence.

## Audio Change Proposal Template

```markdown
## Audio Problem
[Specific, measurable, player-relevant description]

## Change Proposal
| Parameter | Old Value | New Value | Rationale |
|-----------|-----------|-----------|-----------|
| [param]   | [old]     | [new]     | [why]     |

**Player feel:** [one sentence — what changes for the player]

## Boundary Check
- [ ] soundfx.lua still audio-only: no game rules, UI refs, mode imports, persistence reads
- [ ] music_data.lua still declarative: no gfx.*, no playdate.*, no persistence, no lifecycle state mutations
- [ ] Crash safety verified: nil checks on sampleplayer/fileplayer
```

## Volume Mixing Verification

```bash
# Check that SFX volume parameters are higher than music volume parameters
# In soundfx.lua, find SFX volume settings vs music volume settings
rg -n "volume\|setVolume\|Volume" source/soundfx.lua

# Verify music volume is intentionally quiet
rg -n "musicVolume\|music_volume\|MUSIC_VOL" source/soundfx.lua

# Verify SFX volumes are readable levels
rg -n "sfxVolume\|sfx_volume\|SFX_VOL\|playCatch\|playMiss\|playTick" source/soundfx.lua
```

Manual verification: run the game in Simulator, listen in each mode
(Title → Speedrun → Adventure → Endless). SFX should always be clearly
audible over background music.

## PDX Size Analysis

```bash
# Before change: record audio asset sizes
ls -la source/sounds/*.wav

# After change: rebuild and check PDX size
make clean && make build
ls -la Locksmith.pdx

# Check individual asset sizes in the PDX
unzip -l Locksmith.pdx | grep -E '\.wav$|\.pda$'

# Estimate per-track compression (ADPCM ~4:1 vs PCM)
# PCM 16-bit mono 22050 Hz = 44,100 bytes/sec
# IMA ADPCM mono 11025 Hz = ~5,500 bytes/sec
```

## Crash Safety Audit

```bash
# Verify nil checks exist in soundfx.lua
rg -n "if.*== nil\|if not\|pcall\|sampleplayer\|fileplayer" source/soundfx.lua

# Check init() for graceful degradation
rg -n -A 3 "function.*init\|SoundFX.init" source/soundfx.lua

# Verify play functions don't crash on nil players
rg -n -A 3 "function.*play\|SoundFX.play" source/soundfx.lua
```

Manual verification: temporarily rename a .wav file, rebuild, and run.
The game should play silently for that sound, not crash.

## Shuffled Bag Audit (Music Playlist)

```bash
# Verify playlist logic in soundfx.lua
rg -n "shuffle\|bag\|playlist\|nextTrack\|finishCallback\|update" source/soundfx.lua

# Verify track metadata in music_data.lua
rg -n "TRACKS\|PLAYLIST\|DEFAULT_TRACK" source/music_data.lua
```

Manual verification: run the game for 5+ minutes with music on. Verify:
- No track repeats immediately after itself.
- Tracks transition without gaps or overlaps.
- After all tracks play, the bag reshuffles.
- Stopping and restarting music picks a new random track.

## Validation Pack

Run from Locksmith root:

```bash
make check && echo "Check: PASS" || echo "Check: FAIL"
```

For audio, `make check` is informational unless the failing module
is `source/soundfx.lua` itself.

## Pitfalls

1. **SFX volume ≤ music volume**: the most common audio regression.
   Always verify SFX are louder than music in every mode.

2. **Missing nil check on sampleplayer**: `playdate.sound.sampleplayer.new()`
   can return nil. Every SFX play function must guard against nil player.

3. **Music toggle affecting SFX**: `SoundFX.stopMusic()` must stop only
   the fileplayer, not the sampleplayer instances used for SFX.

4. **PDX bloat from uncompressed music**: a 30-second PCM loop at 22050 Hz
   is ~1.3 MB. IMA ADPCM at 11025 Hz brings it to ~165 KB. Always check
   format before adding new music tracks.

5. **Track order: immediate repeats**: shuffled bag must avoid playing
   the same track twice in a row. If only one track exists, that's fine;
   but with 2+ tracks, no immediate repeat.

6. **Finish callback not firing**: `playdate.sound.fileplayer` finish
   callbacks may not fire in all SDK versions. The `update(dtMs)` fallback
   must be robust.

7. **Volume changes not propagated**: changing volume in `init()` but
   not calling `setVolume()` on already-playing players leaves stale
   volume settings.

8. **Spec-code drift**: changing audio behaviour in `soundfx.lua` without
   updating `docs/soundfx.md`. The spec is the source of truth.

9. **Game name confusion**: never refer to Locksmith as Safe Cracker.

10. **Boundary violation**: adding `require("safe")` or
    `require("renderer")` to `soundfx.lua`. Audio module is pure audio.

## Commit And Push Gate

Audio changes modify code. Commit when:

- `source/soundfx.lua` audio behaviour changes.
- `source/music_data.lua` playlist data changes.
- `docs/soundfx.md` spec updates.
- Audio assets added/changed/recompressed.
- AGENTS.md Pitfalls updated with audio-specific error.

If committing agent infrastructure changes:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped audio-specialist changes.
3. Commit with atomic message: `"feat(audio): <change description>"`
4. Push the current branch.
5. Record commit SHA and push result.
