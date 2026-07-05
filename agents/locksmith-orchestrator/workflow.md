# Locksmith Orchestrator Workflow

## Boot Probe

```
git status --short (in locksmith/)
make test-all 2>&1 | tail -5
make build 2>&1 | tail -5
```

Interpretation:
- Dirty worktrees mean changes in flight — route carefully.
- Failing tests before any change means pre-existing instability — note it
  before routing the task.
- Failing build means a broken state — do not route code tasks until it's
  diagnosed.

## Core Workflow

1. **Read** AGENTS.md, docs/manifest.json, docs/workflow.md (always).
2. **Determine task type**: code change, spec change, asset change, test
   failure, build failure, or cross-cutting coordination.
3. **Run baseline** `make test-all` for any code-affecting task.
4. **Read the relevant spec** for the component(s) the task touches.
5. **Decide**: Does the task change behaviour, UX, API, mechanics, or rules?
   - Yes → route spec update to Spec/Contract Guardian or user-authorized agent
     **before** any implementation.
   - No → proceed to implementation routing.
6. **Route to the correct specialist** using the routing rules in role.md.
7. **Collect specialist output**: if multiple specialists, collect and reconcile.
8. **Verify**: run `make lint`, `make test-all`, `make build` on the result.
9. **Handle failure**: diagnose → fix → re-run → document in AGENTS.md.
10. **Report**: task status, what was changed, test results, new pitfalls.

## Routing Heuristic

| Question | Decision |
|----------|----------|
| Changes safe.lua physics, pin mechanics, crankAngle? | → crank-feel-specialist or Lua Gameplay Engineer |
| Changes renderer.lua drawing or ad_renderer.lua? | → Playdate Pixel UI / Renderer Specialist |
| Changes input.lua capture logic? | → Crank Feel / Micro-Mechanics Specialist |
| Changes ui.lua screens, HUD, menus? | → Playdate Pixel UI / Renderer Specialist |
| Changes speedrun/adventure/endless mode logic? | → Lua Gameplay Engineer or mode specialist |
| Changes endless_data, economy_catalogs, balance? | → Endless Economy Specialist |
| Changes datastore or endless_save persistence? | → Lua Gameplay Engineer |
| Changes soundfx.lua or music_data.lua? | → Audio/Music Feedback Specialist |
| Changes adventure story flow or sub-modules? | → Adventure State Machine Specialist |
| Changes images/ or adds new visual assets? | → Visual Art / Asset Production Specialist |
| Changes devlog, README, marketing copy? | → Devlog/Community Marketing Specialist |
| Spec/manifest/code contract drift? | → Spec/Contract Guardian |
| Build/test/toolchain issue? | → Toolchain/Build Gatekeeper |
| Needs a new agent or harness? | → Agent Architect / Crew Builder |
| Cross-cutting, conflicts, blocked task? | Orchestrator handles coordination |

## Coordinator Protocol (cross-cutting tasks)

1. Identify all affected components from the user request.
2. Read all relevant specs.
3. Decide the order: spec updates first, then implementation.
4. Route each component change to its specialist with the full context
   (what changed, why, which other components are affected).
5. If specialists disagree on approach, read both specs and the manifest
   contracts. The orchestrator resolves based on:
   - Manifest API contracts take precedence.
   - Architectural boundary rules (safe.lua purity) are non-negotiable.
   - If still unresolved, escalate to the user with trade-offs.
6. After all specialists report back, run the full verification suite.

## Failure-Handling Workflow

1. **Build failure (`make build` fails)**:
   - Check `pdc` availability (never hardcode path).
   - Check for Lua syntax errors in changed source files.
   - Route to Toolchain/Build Gatekeeper.
   - Document the cause in AGENTS.md pitfalls.

2. **Test failure (`make test-all` fails)**:
   - Identify the failing test(s) and assertion(s).
   - Check if the failure is pre-existing or introduced.
   - Route to the component owner specialist.
   - Document new failure patterns in AGENTS.md.

3. **Lint failure (`make lint` / spec-review.lua)**:
   - Identify which architectural rule was violated.
   - Route back to the implementing specialist.
   - If the rule is wrong or stale, route to Spec/Contract Guardian.

4. **Spec drift (spec ≠ code)**:
   - Route to Spec/Contract Guardian.
   - Block any further changes to that component until drift is resolved.

5. **Boundary violation**:
   - Immediate block. Do not let code with a boundary violation through.
   - Report to the violating specialist and route to Spec/Contract Guardian.

6. **New runtime error or pitfall discovered**:
   - Document in AGENTS.md immediately (mandatory rule).
   - Include: what happened, what API/method was wrong, correct syntax.

## Quality Gates

- User request is understood and task type is classified before any routing.
- `make test-all` baseline is established for code tasks.
- Relevant spec is read before routing implementation.
- If behaviour/UX/API/mechanics/rules change: spec is updated before code.
- After specialist work: `make lint` + `make test-all` + `make build` pass.
- Any new failure is documented in AGENTS.md pitfalls.
- Architectural boundaries are verified.
- Guardrails (no Safe Cracker rename, no Silas rename, no image analysis
  without explicit request, no hardcoded SDK path) are respected.
