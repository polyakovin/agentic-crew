# Adventure State Machine Specialist Workflow

## Boot Probe

Run for non-trivial state machine analysis or transition changes:

```bash
git status --short
cd /root/locksmith && make test-all 2>&1 | tail -5
cd /root/locksmith && make lint 2>&1 | tail -5
cd /root/locksmith && make build 2>&1 | tail -5
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Failing `make test-all` before changes means fix first, then change state machine.
- Missing `pdc` means blocked build gate — cannot verify rendering impacts.

## Core Workflow

1. **Read project context**: AGENTS.md → `docs/manifest.json` → `docs/game.md`
   → `source/states.lua` → `source/adventure_mode.lua` → affected
   `source/adventure/*.lua`.
2. **Identify the state machine issue**: which state(s), which transition(s),
   what invariant is violated.
3. **Model current flow**: draw or list states and transitions as a directed
   graph. Mark unreachable states, impossible transitions, missing guards.
4. **Model proposed flow**: draw or list corrected states and transitions.
   Add guards, document invariants.
5. **Update spec first**: if the change affects ADVENTURE_STATE enum, transition
   contract, or documented flow, update `docs/game.md` and/or `docs/manifest.json`.
6. **Implement**: make the smallest code change respecting architectural
   boundaries. Update `source/states.lua` (enum), `source/adventure_mode.lua`
   (state machine), and `source/adventure/*.lua` (phase modules) as needed.
7. **Update test scenarios**: add or update entries in `docs/test-scenarios.md`.
8. **Verify**: `make test-all` → `make lint` → `make build` → `make check`.
9. **Report**: write concise report with state machine issue, current flow,
   proposed flow, specs updated, code touched, tests, invariants, manual
   verification steps, risks, alternatives.

## State Transition Change Template

When proposing or making a state machine change, use this template:

```
State Machine Issue: [one line]

Current Flow:
  [state] → [transition] → [state] → ...

Proposed Flow:
  [state] → [transition + guard] → [state] → ...

Transition Guards:
  [guard_name]: [condition checked before transition]

Specs updated:
  [spec_file]: [description of change]

Code files touched:
  [source_file]: [description of change with line refs]

Tests added/updated:
  [test_scenario]: [description]

State Invariants Preserved:
  - [invariant 1]
  - [invariant 2]

Manual verification:
  1. [step to verify on Simulator/Playdate]

Risks:
  - [risk description]

Alternatives:
  - [alternative approach if applicable]
```

## State Machine Analysis Checklist

### Reachability

- [ ] Every ADVENTURE_STATE value (except legacy info_curtain=1) is reachable
  from the initial state (story/arrival).
- [ ] No state can only be reached through a path that includes itself
  (no dead cycles).
- [ ] info_curtain (1) is explicitly remapped — never entered directly.

### Transition Completeness

- [ ] Every state has a defined exit path (normal or via cleanup).
- [ ] No transition leaves the state machine in an undefined state.
- [ ] All STORY_TRANSITIONS entries have valid nextKey or nextStateName.
- [ ] All dynamic showStory() calls pass valid nextState values.

### Guards

- [ ] Phase entry guarded: self.phase must be nil or phase-specific check before
  entering a gameplay phase.
- [ ] Story advance guarded: art-only timer (1000ms) must elapse before
  A button works on story screens.
- [ ] Win exit guarded: aPressed on win screen returns false (not earlier).
- [ ] Persistence guarded: markAdventureComplete only on Clock done && aPressed.

### Phase Lifecycle

- [ ] enterState() creates phase object (AdCurtain/AdSafeDial/AdClock) or sets
  phase = nil appropriately.
- [ ] Phase cleanup: self.phase = nil on exit, effects cleared, story state reset.
- [ ] No phase state leaks between phases (crankPos, rollProgress, lastLift, etc.
  are reset or overwritten).

### Exit Behaviour

- [ ] B button does not cause state transition (no B exit from Adventure).
- [ ] System Menu "Title" → main.lua calls mode cleanup without saving progress.
- [ ] System Menu "Settings" → returns to Adventure without state reset.
- [ ] Win screen with A → update() returns false → main.lua sets state = title.

### Spec/Code Consistency

- [ ] ADVENTURE_STATE enum values in states.lua match values used in
  adventure_mode.lua.
- [ ] docs/game.md Adventure section describes the actual implemented flow.
- [ ] docs/manifest.json adventure_mode component matches actual code.
- [ ] docs/test-scenarios.md covers all documented transitions.

## Architecture Boundary Audit

Before any code change, verify:

```bash
# Check adventure_mode.lua doesn't directly use gfx.* (delegates to ad_renderer)
rg -n "gfx\.\||playdate\.graphics" source/adventure_mode.lua | rg -v "ad_renderer\|AdRenderer" && echo "NOTE: check if gfx usage is via renderer" || echo "OK"

# Check adventure/*.lua phase modules don't reference each other's internals
rg -n "ad_curtain\|ad_safedial\|ad_clock" source/adventure/ad_curtain.lua source/adventure/ad_safedial.lua source/adventure/ad_clock.lua && echo "NOTE: check cross-phase refs" || echo "OK"

# Check states.lua — no logic beyond enum definitions
rg -n "function\|if\|for\|while" source/states.lua && echo "NOTE: states.lua should be pure data" || echo "OK"
```

## Validation Pack

Run from Locksmith root:

```bash
make test-all && echo "Tests: PASS" || echo "Tests: FAIL"
make lint && echo "Lint: PASS" || echo "Lint: FAIL"
make build && echo "Build: PASS" || echo "Build: FAIL"
# Full cycle:
make check && echo "Check: PASS" || echo "Check: FAIL"
```

If tests fail after a state machine change:
1. Check that ADVENTURE_STATE enum values haven't shifted.
2. Ensure new state transitions don't break existing test expectations.
3. Update test assertions if the state machine change is intentional.
4. Never change tests to "make them pass" — tests validate contracts.

## Story Flow Audit Checklist

When auditing story flow:

1. Verify STORY_TRANSITIONS table completeness: intro → hero → curtain_lock.
2. Verify all dynamic showStory() calls pass valid ADVENTURE_STATE values.
3. Verify advanceStory() clearing: self.storyKey, self.storyNextKey,
   self.storyNextState, self.storyStartMs all set to nil.
4. Check art-only (1000ms) → text slide timing: STORY_ART_ONLY_MS constant,
   isStoryTextVisible(), getStoryElapsedMs().
5. Verify A button only advances on text slide (not during art-only phase).
6. Verify story screen clears effects (self:clearEffects() in showStory).
7. Check that story screens don't draw effects overlay
   (self.state ~= ADVENTURE_STATE.story check in draw()).

## Exit Behaviour Verification

Manual test on Simulator/Playdate:

1. **B button during gameplay**: press B during phase1_curtain, curtain_cleared,
   phase3_safedial, phase4_clock. Should do nothing (no exit).
2. **B button during story**: press B during any story screen. Should do nothing.
3. **B button on win**: press B on win screen. Should do nothing.
   Press A on win screen. Should exit to title.
4. **System Menu → Title**: open system menu, select Title.
   Should exit to title. Adventure progress should NOT be saved
   (Datastore.markAdventureComplete should NOT have been called).
5. **System Menu → Settings**: open system menu during Adventure, select Settings,
   change a setting, press Back. Should return to Adventure at same state
   without reset.

## Pitfalls

1. **info_curtain remap**: ADVENTURE_STATE.info_curtain=1 is a legacy value.
   enterState() remaps it to phase1_curtain. Do NOT add code that enters
   info_curtain intentionally — it's a dead code path kept for backward compat.

2. **Dual state tracking**: avoid tracking the same condition in both
   self.state and a separate boolean flag. If self.state == phase1_curtain,
   don't also check self.inCurtainPhase — use the state directly.

3. **Phase state leak**: when exiting a gameplay phase (e.g., phase1_curtain →
   curtain_cleared), ensure self.phase is recreated (new AdCurtain()) or set
   to nil. The old phase object's internal state must not persist.

4. **Story transition orphans**: every showStory() call must eventually
   resolve through advanceStory(). An orphan nextKey or nextState that's
   never consumed will cause silent bugs.

5. **Win screen exit timing**: the win screen should NOT return false
   immediately. It requires an explicit A press. Returning false prematurely
   would skip the win story/screen.

6. **Persistence boundary**: Datastore.markAdventureComplete() is called only
   on Clock done && aPressed — NOT on earlier phases. Calling it earlier would
   mark Adventure complete before the player finishes.

7. **System Menu cleanup**: when the Playdate System Menu triggers a Title
   transition, main.lua calls the mode's cleanup but Adventure does NOT save
   progress. This differs from Endless mode which auto-saves.

8. **B button expectation**: players expect B to go back, but Adventure
   intentionally blocks this. The design reason: Adventure is an onboarding/
   story flow — players should experience it as one unbroken sequence.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped state-machine-specialist changes.
3. Commit with atomic message: `"feat(adventure-state): <issue description>"`
4. Push the current branch.
5. Record commit SHA and push result.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- credentials or branch policy prevent push.
