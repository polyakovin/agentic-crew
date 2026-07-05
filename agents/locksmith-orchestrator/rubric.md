# Locksmith Orchestrator Rubric

## Excellent

- Task type is correctly classified (code/spec/asset/coordination).
- Baseline `make test-all` is run for any code task.
- Relevant spec is read before routing implementation.
- Behaviour/UX/API/mechanic/rule changes route spec update BEFORE code.
- Correct specialist is selected for each component.
- Cross-cutting tasks are sequenced correctly (spec → code → verify).
- Verification suite runs after specialist returns.
- New failures are documented in AGENTS.md with cause and correct syntax.
- Architectural boundaries are respected.
- Guardrails are never violated.
- Conflicting specialist outputs are resolved using manifest contracts.

## Acceptable

- Task is routed correctly but verification step missed a subset of tests.
- Minor failure documented in AGENTS.md but could be more detailed.
- Cross-cutting coordination works but sequencing could be tighter.
- Baseline `make test-all` run but result not explicitly recorded.

## Needs Revision

- Spec not read before routing implementation.
- Behaviour change implemented without spec update.
- Wrong specialist routed for a component.
- Architectural boundary violated.
- Verification suite not run after specialist work.
- Failure not documented in AGENTS.md.
- Guardrails violated (renamed Silas, called game Safe Cracker, analyzed images
  without request, hardcoded SDK path, generated procedural art).
- `make test-all` baseline not run before routing code changes.

## Critical Failure

- Game code modified without specialist delegation (unless user explicitly
  permitted direct action).
- Game code modified that violates architectural boundaries.
- Guardrails violated with no correction.
- Spec changed without consulting the relevant specialist.
- User request ignored or misrepresented in routing.
- Verification suite not run and build/test failures introduced.
- Specialist output accepted without checking architectural compliance.
