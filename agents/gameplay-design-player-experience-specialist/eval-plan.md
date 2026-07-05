# Eval Plan — Gameplay Design / Player Experience Specialist

## Evaluation Dimensions

### 1. Proposal Quality

**Test:** Run specialist on 3 sample tasks (see role.md examples).
**Check:** Each output has all required sections (player problem, proposed experience, affected docs, acceptance criteria, test scenarios, risks).
**Pass:** 3/3 proposals have ≥3 acceptance criteria and ≥2 test scenarios.

### 2. Non-goal Compliance

**Test:** Run specialist on a task that touches crank micro-mechanics.
**Check:** Specialist identifies out-of-scope and proposes handoff to crank-feel-specialist.
**Pass:** Handoff recommended, no attempt to solve in scope.

**Test:** Run specialist on a task that touches pixel rendering.
**Check:** Specialist identifies out-of-scope and proposes handoff to pixel-ui-renderer-specialist.
**Pass:** Handoff recommended.

### 3. Spec-First Compliance

**Test:** Run specialist on a UX change request (e.g. "move MISS indicator").
**Check:** Spec update is proposed before code.
**Pass:** Proposal names the affected docs/*.md and includes concrete spec wording change.

### 4. Playdate Constraint Awareness

**Test:** Run specialist on a proposal that could violate Playdate limits (e.g. full-color feedback, complex animation).
**Check:** Specialist flags the constraint and adjusts proposal.
**Pass:** Proposal works within 1-bit 400×240, crank/D-pad/A/B, no audio-essential feedback.

### 5. Handoff Quality

**Test:** Run specialist on a cross-cutting task.
**Check:** Handoff context includes spec update reference and clear next specialist.
**Pass:** Handoff includes affected specs, acceptance criteria, and next specialist id.

### 6. RNG Boundary Enforcement

**Test:** Run specialist on a task proposing RNG-based feedback ("randomize miss direction for variety").
**Check:** Specialist identifies this as RNG-dependent mechanic (non-goal violation) and proposes precision-based alternative.
**Pass:** No RNG-based mechanic accepted in proposal; precision-based alternative offered.

## Scoring

Each dimension: 0 (fail), 1 (partial), 2 (pass).
- 8+: Ready for pilot
- 4-7: Needs training/refinement
- 0-3: Redesign needed

## Success Over Time

- 5 consecutive proposals with score ≥4/5: promote to pilot
- 10 consecutive proposals with score ≥5/5: promote to production
- Any proposal that reaches implementation and passes user acceptance: count as real-world evidence
