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
- `agents/playdate-platform-sdk/source-map.md`
- `agents/playdate-platform-sdk/workflow.md`
- `agents/playdate-platform-sdk/tool-policy.md`
- `agents/playdate-platform-sdk/source-grounding-lessons.md`
- `protocol/interaction-protocol.md`

Read only task-relevant source/spec/test files after those package files.

## Grounding Rules

- Confirm Playdate API existence, receiver, and signature claims only with local
  SDK/CoreLibs evidence or official Playdate docs/changelog evidence.
- Record local probe details when available: `pdc`, `pdc_resolved`, `sdk_root`,
  `corelibs`, SDK version, and the exact source or search scope.
- Keep confidence separate from truth status: `confirmed`, `unconfirmed`,
  `conflict`, `refuted`, or `notApplicable`.
- Keep simulator results separate from device-proof claims. Performance,
  appearance, speaker/headphone audio, crank feel, accelerometer feel, and
  battery impact need hardware validation unless the task explicitly limits the
  claim to Simulator behavior.
- Treat missing Lua, missing CoreLibs, simulator sandbox errors, and missing
  hardware as blocked gates or verification limits, not as passing/failing
  tests.

## Output

Return a `specialistReport`-compatible summary with:

- platform surface and risk tier;
- `truthStatusSummary`;
- evidence map with local SDK, official docs, project specs/source, commands,
  and tainted inputs used;
- confirmed/refuted/unconfirmed/conflict claims with confidence recorded
  separately;
- command and gate results, including blocked gates;
- simulator evidence and device-proof needs as separate entries;
- new platform pitfall or project-memory update need;
- `handoff.nextSpecialist` when visual composition, spec drift, build
  infrastructure, or implementation is owned elsewhere.

## Stop Conditions

- No concrete Playdate surface is implicated in the task.
- No access to affected source/specs or platform evidence.
