# Agentic Crew

Reusable specialist-agent harness folders and a shared Agent2Agent profile for
project-specific agent teams.

This repository is a harness library, not an agent runtime. It provides:

- an Agentic Crew profile for the A2A protocol;
- copyable `agents/<id>/` harness folders with role instructions, A2A Agent
  Cards, and local wiring;
- reusable software-development specialist harnesses inspired by public
  multi-agent software-company patterns;
- a Playdate Platform / SDK Specialist role for Playdate game projects;
- a Codex skill that helps author new specialist harnesses using the same
  protocol.

No third-party prompts or source code are copied verbatim. The harnesses are
project-neutral templates designed to be adapted to a repository's own specs,
history, risks, tools, and verification gates.

## Layout

```text
protocol/
  interaction-protocol.md
  message-schema.json
agents/
  README.md
  orchestrator/
    README.md
    role.md
    agent-card.json
    harness.yaml
    operations.md
    run-record.template.json
  product-manager/
    ...
  system-architect/
    ...
  implementation-engineer/
    ...
  qa-reviewer/
    ...
  protocol-steward/
    ...
  playdate-platform-sdk/
    entrypoint.md
    role.md
    agent-card.json
    harness.yaml
    source-map.md
    workflow.md
    tool-policy.md
    rubric.md
    eval-plan.md
    run-record.template.json
    release-rollback.md
    health-snapshot.md
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
2. Copy only the `agents/<id>/` harness folders needed for the project.
3. Narrow each copied agent's `What To Read` and `harness.yaml` context policy
   to project-local sources.
4. Deploy or adapt each agent's `agent-card.json` for the runtime endpoint.
5. Keep `operations.md` or split operational docs in sync with the project:
   source hierarchy, workflow, tool policy, rubric, evals, run records, and
   release/rollback.
6. Require every specialist to exchange Agentic Crew payloads using
   `protocol/interaction-protocol.md`.
7. Keep the orchestrator on the strongest model available when tradeoffs,
   conflict resolution, risk calls, or final acceptance matter.

The A2A protocol is the wire-level contract: Agent Cards advertise specialists,
`SendMessage` starts or continues work, `Task`/`Message` carry state, and
`Part`/`Artifact` objects carry text or structured JSON. Agentic Crew adds only
the project-specific payload shapes such as `taskBrief`, `specialistReport`,
`reviewFinding`, `decisionRecord`, and `handoffPacket`.

## Design Rules

- Prefer narrow roles over a large generic "senior engineer" prompt.
- Treat skills and harness folders like dependencies: review provenance and
  keep them versioned.
- Do not let agent instructions override project source of truth.
- If a role cannot cite evidence, it should report uncertainty or block.
- Use automation and tests as gates; use agents for judgment and investigation.
- High-risk specialists should have split operational docs, not only a compact
  `operations.md`.
