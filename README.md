# Agentic Crew

Reusable specialist-agent role cards and a shared interaction protocol for
project-specific agent teams.

This repository is a role library, not an agent runtime. It provides:

- a common communication protocol for specialist agents;
- reusable software-development specialist roles inspired by public multi-agent
  software-company patterns;
- a Playdate Platform / SDK Specialist role adapted from the Locksmith project;
- a Codex skill that helps author new role cards using the same protocol.

No third-party prompts or source code are copied verbatim. The roles are
project-neutral templates designed to be adapted to a repository's own specs,
history, risks, tools, and verification gates.

## Layout

```text
protocol/
  interaction-protocol.md
  message-schema.json
roles/
  orchestrator.md
  product-manager.md
  system-architect.md
  implementation-engineer.md
  qa-reviewer.md
  protocol-steward.md
  playdate-platform-sdk.md
packs/
  software-development-crew.yaml
  playdate-game-crew.yaml
references/
  source-patterns.md
.agents/skills/
  agentic-crew-author/SKILL.md
examples/
  handoff-packet.md
```

## How To Use

1. Pick a pack from `packs/`.
2. Copy only the roles needed for the project.
3. Narrow each role's `What To Read` to project-local sources.
4. Require every specialist to report using `protocol/interaction-protocol.md`.
5. Keep the orchestrator on the strongest model available when tradeoffs,
   conflict resolution, risk calls, or final acceptance matter.

## Design Rules

- Prefer narrow roles over a large generic "senior engineer" prompt.
- Treat skills and role cards like dependencies: review provenance and keep
  them versioned.
- Do not let role cards override project source of truth.
- If a role cannot cite evidence, it should report uncertainty or block.
- Use automation and tests as gates; use agents for judgment and investigation.
