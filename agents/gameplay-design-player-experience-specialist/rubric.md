# Rubric — Gameplay Design / Player Experience Specialist

## Quality Dimensions

### 1. Player Problem Clarity (weight: high)
- [ ] Player problem is one sentence describing what the player feels/experiences
- [ ] Problem is not technical (bad code) but experiential (bad feeling)
- [ ] Problem is in scope (not crank-micro-mechanics, not pixel-rendering)

### 2. Proposed Experience (weight: high)
- [ ] Describes what the player should see/hear/feel moment-to-moment
- [ ] References specific Playdate input/output: crank position, screen area, sound timing
- [ ] Respects 1-bit 400×240 constraints
- [ ] Respects crank 0-360°, D-pad, A/B, Menu button limitations

### 3. Affected Docs/Specs (weight: medium)
- [ ] Every affected docs/*.md is named
- [ ] docs/manifest.json update is considered where relevant
- [ ] docs/test-scenarios.md update is considered

### 4. Acceptance Criteria (weight: high)
- [ ] Each criterion is concrete and verifiable (yes/no, not "feels better")
- [ ] Criteria can be checked in Playdate Simulator
- [ ] Criteria are numbered 1-3 minimum

### 5. Test Scenarios (weight: high)
- [ ] At least 2 test scenarios per proposal
- [ ] Scenarios include edge cases (first attempt, rapid attempts, all-pins-locked)
- [ ] Scenarios can be automated or manually run in Simulator

### 6. Risks/Tradeoffs (weight: medium)
- [ ] At least 1 risk and 1 tradeoff documented
- [ ] Risks name the affected system or mode

### 7. Spec-First Compliance (weight: high)
- [ ] Spec update is proposed before any code change
- [ ] Spec update is concrete (wording changes or additions)
- [ ] Non-goals are not violated (no RNG, no name-safe-cracker, etc.)

### 8. Handoff (weight: medium)
- [ ] Handoff target is clear (self / crank-feel / pixel-ui / implementer)
- [ ] Handoff context includes spec update reference

## Scoring

Each dimension: 0 (absent), 1 (partial), 2 (full). Total: 0-16.
- 14-16: Ready for pilot
- 10-13: Needs refinement
- 0-9: Needs redesign before handoff
