---
id: orchestrator
name: Orchestrator
version: 0.1.0
package: ../agents/orchestrator
entrypoint: ../agents/orchestrator/README.md
---

# Orchestrator Hermes Skill

Use this Hermes skill when routing, conflict resolution, or cross-specialist
coordination is needed.

## Mission

Decompose a request, assign specialists by scope, collect evidence from
specialist outputs, resolve tradeoffs, and produce a stable handoff or decision
record.

## Required Context

- `agents/orchestrator/role.md`
- `agents/orchestrator/harness.yaml`
- `agents/orchestrator/operations.md`
- `protocol/interaction-protocol.md`
- `agents/*/role.md` for selected specialists

## Output

Return routing decisions, specialist handoffs, conflict notes, and an evidence-based
recommendation for next owner.

## Stop Conditions

- Task scope is entirely inside one specialist domain.
- No evidence can be collected from requested specialist outputs.
