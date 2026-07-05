# Adventure State Machine Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly analyzes, proposes, and implements
Adventure state machine changes while respecting architectural boundaries,
spec-driven development, state invariants, and mode isolation.

## Metrics

- task success;
- state machine issue clarity;
- current/proposed flow completeness;
- transition guard correctness;
- boundary respect (adventure_mode/phase/renderer/data separation);
- spec update completeness;
- `make check` pass rate;
- state invariant preservation;
- reachability audit completeness;
- phase lifecycle correctness;
- exit behaviour correctness;
- persistence boundary correctness;
- coupling avoidance (Adventure vs Speedrun/Endless);
- risk documentation.

## Seed Cases

### ASM-001: Unreachable State Audit

Input: "Audit all ADVENTURE_STATE values — which ones are unreachable?"

Expected:

- Reads AGENTS.md → `docs/manifest.json` → `docs/game.md` →
  `source/states.lua` → `source/adventure_mode.lua`.
- Maps every ADVENTURE_STATE value to its transition path.
- Identifies: info_curtain (1) — legacy remapped, never entered directly;
  info_painting (4) — not currently used; phase2_painting (5) — not used
  (curtain_cleared handles roll-up); info_safedial (6) — not used;
  info_clock (8) — not used.
- Recommends: either remove unused states or document that they're reserved
  for future phases.
- Do NOT remove states without spec update and user approval.
- `make check` passes (no code changes if only auditing).

### ASM-002: Phase State Leak Detection

Input: "Check if phase state from curtain lock is leaking into curtain_cleared."

Expected:

- Reads `source/adventure_mode.lua` enterState() and update() for
  phase1_curtain → curtain_cleared transition.
- Verifies: enterState(curtain_cleared) sets self.phase = nil.
- Verifies: update() in curtain_cleared uses self.rollProgress
  and self.crankAngle (owned by adventure_mode, not phase).
- Checks that self.lastLift (set in phase1_curtain) doesn't affect
  curtain_cleared logic.
- Reports: phase object is nil'd; adventure_mode-owned fields are
  overwritten or irrelevant. No leak.
- If leak found: proposes fix and updates spec.

### ASM-003: Story Transition Orphan Detection

Input: "Verify all story transitions resolve — no orphan nextKey/nextState."

Expected:

- Reads STORY_TRANSITIONS table, all showStory() calls, and advanceStory().
- Traces arrival → intro → hero → curtain_lock → phase1_curtain.
- Traces dynamic: curtain_cleared → showStory("safe_dial", phase3_safedial).
- Traces dynamic: phase3_safedial done → showStory("clock", phase4_clock).
- Traces dynamic: phase4_clock done → showStory("win", win).
- Reports: all transitions resolved or identifies orphans.
- If orphan found: proposes fix (missing STORY_TRANSITIONS entry or wrong nextKey).

### ASM-004: B Button Exit Prevention

Input: "Verify B button does not exit Adventure mode at any point."

Expected:

- Audits update() in adventure_mode.lua for every state.
- Confirms: no code path where B button triggers state change to exit.
- Confirms: only win state with aPressed returns false.
- Confirms: story state only responds to aPressed (not bPressed).
- Reports: B is fully blocked in all Adventure states.
- If B exit path found: proposes fix to block it.

### ASM-005: Win Screen Exit Correctness

Input: "Verify win screen returns false on A and transitions to title correctly."

Expected:

- Reads adventure_mode.lua win state handling.
- Confirms: win state update() checks actions.aPressed and returns false.
- Confirms: win state draw() calls self.renderer:drawWin().
- Traces main.lua: AdventureMode:update(dt) return value → if false, set state = title.
- Reports: exit flow is correct or identifies gap.
- Manual verification: enter Adventure, complete all phases, press A on win →
  should see title screen.

### ASM-006: Persistence Boundary Audit

Input: "Audit Datastore.markAdventureComplete() — is it called at the right time?"

Expected:

- Reads adventure_mode.lua for markAdventureComplete() call.
- Confirms: only called in phase4_clock done && aPressed block.
- Verifies: NOT called on curtain_cleared, NOT called on phase3_safedial,
  NOT called on System Menu exit.
- Reads datastore.lua: markAdventureComplete() sets adventureCompleted flag.
- Reports: persistence boundary is correct or identifies premature/missing call.

### ASM-007: New Phase Addition Design

Input: "Design adding a new phase 'phase2_painting' between curtain_cleared
and phase3_safedial."

Expected:

- Reads current flow: curtain_cleared → showStory("safe_dial", phase3_safedial).
- Proposes: curtain_cleared → enterState(phase2_painting) → phase done →
  showStory("safe_dial", phase3_safedial).
- Updates ADVENTURE_STATE enum if phase2_painting=5 needs activation.
- Updates docs/game.md with new phase spec.
- Updates docs/manifest.json component dependencies.
- Ensures phase lifecycle: ad_painting module with new/update/draw.
- Adds transition guard: phase2_painting can only be entered from
  curtain_cleared (rollProgress >= 1).
- `make check` passes.
- Manual verification steps provided.

### ASM-008: State Machine Simplification

Input: "The ADVENTURE_STATE enum has info_curtain, info_painting, info_safedial,
info_clock, phase2_painting that are never used. Should we remove them?"

Expected:

- Identifies which states are truly unreachable vs reserved.
- Proposes either: (a) remove unused states and update all references, or
  (b) comment them as reserved for future phases.
- If removing: updates states.lua, adventure_mode.lua (enterState remap),
  docs/game.md, docs/manifest.json, docs/test-scenarios.md.
- Warns: removing enum values shifts numeric values — check that no
  serialized data depends on numeric enum values.
- `make check` passes.
- Asks for user approval before making breaking enum change.

### ASM-009: System Menu Cleanup Verification

Input: "What happens to Adventure progress when the player opens System Menu
and selects Title?"

Expected:

- Reads main.lua system menu handling.
- Traces: System Menu "Title" → main.lua calls current mode cleanup.
- Confirms: Adventure mode cleanup does NOT call Datastore.markAdventureComplete().
- Confirms: Adventure mode cleanup resets self.phase, self.state, story state.
- Reports: Adventure progress is NOT saved on System Menu exit (correct behaviour).
- Contrasts with Endless mode which auto-saves.

### ASM-010: Spec/Code Drift Detection

Input: "Check docs/game.md Adventure section against actual adventure_mode.lua
code — are they in sync?"

Expected:

- Reads docs/game.md Adventure state machine section line by line.
- Reads adventure_mode.lua line by line.
- Compares: state list, transition order, story flow, exit behaviour,
  enterState routing.
- Reports every discrepancy with file:line references.
- Proposes spec or code updates to resolve each discrepancy.
- Updates the one that's wrong (spec-first principle: if spec matches
  intended behaviour, update code; if code matches intended behaviour,
  update spec).
- `make check` passes.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real state machine change is implemented, tested, and deployed.
- No boundary violations in any run.
- No coupling with Speedrun/Endless introduced.

Move to `production` only after:

- Pilot evidence from 3+ state machine changes.
- Spec updates are consistent and complete.
- Pitfalls are documented in AGENTS.md.
- No unreachable state regressions.
- No orphan transition regressions.
