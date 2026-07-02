# Codex Entrypoint: Playdate Platform / SDK Specialist

This is the Codex custom-agent entrypoint for the Locksmith
`playdate-platform-sdk` specialist.

The canonical specialist harness is:

```text
.hermes/skills/playdate-platform-sdk.md
.hermes/agents/playdate-platform-sdk/
```

Read that Hermes runbook completely before doing task work. Treat it as the
single source of truth for mission, scope, source hierarchy, boot probe,
automation packs, platform matrices, challenge review checklist, risk gates,
Agent Card, run-record template, eval plan, and handoff contract.

## Codex-Specific Rules

- Do not duplicate or override the Hermes runbook in this file.
- If a rule here appears to conflict with `.hermes/skills/playdate-platform-sdk.md`,
  prefer the Hermes runbook and update this wrapper only to clarify routing.
- If the specialist harness needs substantive changes, edit the Hermes runbook
  first. This wrapper should remain thin.
- Preserve the project-wide rules from `AGENTS.md`.
- Do not analyze user-provided images unless the user explicitly asks for image
  analysis.
- For Playdate API claims, verify local SDK/CoreLibs and official docs as
  required by the Hermes runbook.

## Minimum Startup

1. Read `AGENTS.md`.
2. Read `.hermes/skills/playdate-platform-sdk.md` completely.
3. Read `.hermes/agents/playdate-platform-sdk/agent-card.json`.
4. Read additional `.hermes/agents/playdate-platform-sdk/*` references according
   to the Hermes skill's `What To Read` section.
5. Run the Hermes runbook's required boot probe unless the current task
   explicitly forbids tool use or is purely about documentation wording.
6. Follow the Hermes handoff contract.

## Routing Note

Use this Codex agent when a Codex thread or orchestrator wants the same platform
specialist that Hermes uses:

- SDK/API correctness.
- Crank/buttons/accelerometer/platform input.
- Graphics draw modes, fonts, images, and display behavior.
- Datastore, sound, lifecycle, Simulator/device differences.
- `pdc`, Lua runtime, SDK version drift, performance, and GC.
