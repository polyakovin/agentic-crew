---
id: system-architect
name: System Architect
version: 0.1.0
package: ../agents/system-architect
entrypoint: ../agents/system-architect/README.md
---

# System Architect Hermes Skill

Use this Hermes skill when architecture, contracts, or module boundaries are in
play.

## Mission

Map component ownership, identify coupling risks, and produce minimal design or
boundary recommendations with rollback and verification implications.

## Required Context

- `agents/system-architect/role.md`
- `agents/system-architect/harness.yaml`
- `agents/system-architect/operations.md`
- `protocol/interaction-protocol.md`

## Output

Provide architecture recommendations with affected contracts/systems, risk list, and
next specialist handoff.

## Stop Conditions

- Scope only requires implementation or QA with no architectural impact.
- No stable source-of-truth contract can be identified for the change.
