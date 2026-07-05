# Adventure State Machine Specialist Rubric

Use this rubric before handing off a state machine analysis or transition change.

## Excellent

- State machine issue clearly stated in one line.
- Current and proposed flows documented as directed graph (states + transitions).
- All transition guards documented with rationale.
- Spec updated before code change (docs/game.md, docs/manifest.json).
- Code change respects all architectural boundaries (adventure_mode.lua for
  state machine, ad_curtain/ad_safedial/ad_clock for phase logic,
  ad_renderer for drawing, ad_story_data for declarative data).
- `make test-all`, `make lint`, `make build`, `make check` all pass.
- New test scenarios added to docs/test-scenarios.md.
- State invariants listed and verified.
- Manual verification steps provided for Simulator/Playdate.
- Risks and alternatives documented.
- AGENTS.md updated if new pitfall discovered.
- Reachability audit completed for all ADVENTURE_STATE values.
- Phase lifecycle verified (enter/update/draw/exit/reset).
- Exit behaviour verified (B blocked, win+A returns false, System Menu cleanup).
- Persistence boundary verified (markAdventureComplete only at Clock done).
- No coupling with Speedrun/Endless logic.
- Small incremental change, not rewritten state machine.

## Acceptable

- State machine issue stated but vague.
- Current/proposed flows documented but lacking transition guard details.
- Spec updated after code or rationale not fully explained.
- `make test-all` passes but no manual verification steps.
- Risks mentioned but alternatives not explored.
- One minor state machine check missed (non-critical).
- Phase lifecycle mostly correct but one edge case not addressed.

## Needs Revision

- No state machine issue stated — "changed enterState()" with no why.
- Spec not updated when ADVENTURE_STATE enum or flow changed.
- Architectural boundary violated (e.g., state machine logic in ad_renderer).
- `make check` not run or fails unreported.
- State machine change breaks existing tests without test update.
- No manual verification guidance.
- Broad refactor instead of targeted transition fix.
- New ADVENTURE_STATE value added without updating all references.
- Orphan story transition introduced (dangling nextKey/nextState).
- B button exit added to Adventure (breaking intentional design).
- Phase state leak: self.phase not recreated/cleaned on transition.

## Critical Failure

- Renames game to Safe Cracker.
- Breaks Adventure/Speedrun/Endless mode lifecycle.
- Adds Adventure state machine logic to Speedrun or Endless code.
- Makes B button exit Adventure mode (breaking core design constraint).
- Calls Datastore.markAdventureComplete() before Clock done.
- Introduces unreachable state or impossible transition.
- Introduces duplicate flag tracking same condition.
- Adds new global mutable state without necessity.
- Changes ADVENTURE_STATE enum values without updating references.
- Commits changes without validation.
- Overwrites unrelated user changes.
- Claims "state machine fix" but introduces coupling or unreachable paths.

## Challenge Checklist

- What is the state machine issue in one line?
- What are the current states and transitions?
- What are the proposed states, transitions, and guards?
- Is the spec updated before code (docs/game.md)?
- Do all architectural boundaries hold?
- Does `make check` pass?
- Are new test scenarios needed (docs/test-scenarios.md)?
- Is every ADVENTURE_STATE value reachable (or documented as unreachable)?
- Are all story transitions resolved?
- Is phase lifecycle clean (enter/exit/reset)?
- Does B button NOT exit Adventure?
- Does win screen return false on A?
- Is Datastore.markAdventureComplete() at the right point?
- Is there any coupling with Speedrun/Endless?
- Are state invariants preserved?
- Is this the smallest change that achieves the fix?
- Are risks documented?
- Are manual verification steps clear?
