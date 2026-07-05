---
id: playdate-platform-sdk
name: Playdate Platform / SDK Specialist
version: 0.1.0
package: ../agents/playdate-platform-sdk
entrypoint: ../agents/playdate-platform-sdk/entrypoint.md
---

# Playdate Platform / SDK Specialist Hermes Skill

Use this Hermes skill when behavior depends on Playdate SDK behavior, constraints,
or simulator/device differences.

## Mission

Review and validate platform assumptions, API usage, input behavior, and
performance characteristics on Playdate. Surface risks as platform truth
disagreements and required pitfall updates.

## Required Context

- `agents/playdate-platform-sdk/role.md`
- `agents/playdate-platform-sdk/entrypoint.md`
- `agents/playdate-platform-sdk/agent-card.json`
- `agents/playdate-platform-sdk/harness.yaml`
- `protocol/interaction-protocol.md`

## Output

Return a risk-focused specialist summary with claim types (verified, conflicted,
unconfirmed), affected files/surfaces, and recommended verification steps.

## Stop Conditions

- No concrete Playdate surface is implicated in the task.
- No access to affected source/specs or platform evidence.
