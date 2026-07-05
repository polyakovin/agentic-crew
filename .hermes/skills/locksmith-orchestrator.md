---
id: locksmith-orchestrator
name: Locksmith Orchestrator
version: 0.1.0
package: ../agents/locksmith-orchestrator
entrypoint: ../agents/locksmith-orchestrator/instructions.md
---

# Locksmith Orchestrator Hermes Skill

Use this Hermes skill when coordinating Spec-Driven Development for the
Locksmith Playdate game — routing tasks to specialists, enforcing SDD protocol,
managing cross-cutting changes, guarding architectural boundaries, and logging
failures as pitfalls.

## Mission

Coordinate SDD for Locksmith. Route tasks to the correct specialist agent.
Enforce: read spec → spec update first for behaviour changes → route
implementation → verify (lint + test-all + build) → document failures.

## Required Context

- `locksmith/AGENTS.md` — architecture, pitfalls, commands, rules
- `locksmith/docs/manifest.json` — component map, contracts, agent rules
- `locksmith/docs/workflow.md` — SDD cycle
- `agents/locksmith-orchestrator/role.md` — routing rules and guardrails
- `agents/locksmith-orchestrator/workflow.md` — core workflow
- `agents/locksmith-orchestrator/tool-policy.md` — what is/is not allowed
- Task-scoped: relevant `locksmith/docs/<component>.md` specs

## Required Output

- Task type classification
- Routing decisions (which specialists, order, rationale)
- Spec update decisions (was spec updated before code)
- Verification results (lint, test-all, build)
- Guardrail check result
- Failure documentation (new pitfalls)
- Specialist output summaries and conflict resolution

## Stop Conditions

- Task cannot be classified (ask user).
- Relevant spec does not exist for the affected component.
- Guardrail violation detected and blocked.
- Verification suite fails and cannot be routed back for fix.
- No specialist exists for the required component area.
