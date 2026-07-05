# Workflow — Gameplay Design / Player Experience Specialist

## Full Workflow

### Phase 1: Context (read-only)

1. Read AGENTS.md (rules, pitfalls, architecture)
2. Read docs/manifest.json (component contracts)
3. Read relevant specs: game.md, safe.md, input.md, ui.md, test-scenarios.md, lore.md
4. Read affected source files (context only)

### Phase 2: Problem Formulation

5. Identify player problem — one sentence describing what feels wrong
6. Verify problem is in scope (not crank-micro-mechanics, not pixel-rendering)

### Phase 3: Design Proposal

7. Write proposed experience (what the player should feel/see/hear)
8. List affected docs/specs
9. List affected systems (source files)
10. Define acceptance criteria (3-5 concrete, verifiable conditions)
11. Define test scenarios (unit, integration, manual)
12. Document risks and tradeoffs

### Phase 4: Spec Update

13. Update relevant docs/*.md first (spec-first principle)
14. If adding test scenarios, update docs/test-scenarios.md

### Phase 5: Handoff or Implementation

15. If change needs code: handoff to implementation-engineer or relevant specialist with spec update context
16. If change needs crank-touch: handoff to crank-feel-specialist
17. If change needs pixel-layout: handoff to playdate-pixel-ui-renderer-specialist
18. If change is purely UX-design: provide report to user/orchestrator

### Phase 6: Report

19. Write concise report with all proposal components
20. Document AGENTS.md pitfalls if new lessons discovered

## Quick Workflow (small proposals)

For small, clear UX requests (e.g. "сдвинуть MISS feedback левее"):
1. Read relevant spec
2. Write design proposal (player problem → proposed experience → acceptance criteria → scenarios)
3. Update spec
4. Report

## Workflow Diagram

```
Context → Problem → Proposal → Spec Update → Handoff → Report
   ↓          ↓          ↓           ↓           ↓         ↓
Read all   1-line    Experience   Update     Delegate   Document
specs      problem  + AC +       docs/*.md  to right   findings
                    scenarios               specialist
