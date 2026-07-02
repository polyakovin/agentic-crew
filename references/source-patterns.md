# Source Patterns

This repository adapts public multi-agent patterns without copying third-party
prompts or implementation code.

## A2A

Pattern used:

- Agent Cards for discovering agent identity, capabilities, interfaces, and
  skills;
- A2A tasks, messages, parts, and artifacts as the cross-agent contract;
- JSON-RPC `SendMessage`/`SendStreamingMessage` and task lifecycle methods as
  the runtime interaction model.

Adapted here as:

- `protocol/interaction-protocol.md`;
- `protocol/message-schema.json` for Agentic Crew data payloads carried inside
  A2A parts or artifacts;
- `agents/*/agent-card.json` templates for exported specialists.

## OpenAI Plugins / Skills

Pattern used:

- skills and plugins as reusable, versioned agent capability packages;
- small `SKILL.md` entrypoints;
- explicit trigger descriptions and workflow instructions.

Adapted here as:

- `.agents/skills/agentic-crew-author/SKILL.md`;
- agent harness folders that can become Codex skills or custom subagents.

## MetaGPT

Pattern used:

- software-company style role separation;
- SOP-driven collaboration;
- product, architecture, project coordination, and engineering roles.

Adapted here as:

- `orchestrator`;
- `product-manager`;
- `system-architect`;
- `implementation-engineer`;
- protocol-first handoffs instead of a runtime-specific workflow.

## ChatDev

Pattern used:

- staged software development through specialist dialogue;
- coding and testing phases as separate responsibilities;
- communication discipline to reduce hallucinated handoffs.

Adapted here as:

- `qa-reviewer`;
- explicit `taskBrief`, `specialistReport`, and `handoffPacket` payloads inside
  A2A messages or artifacts.

## CrewAI

Pattern used:

- agents with roles, goals, and tasks;
- crews/packs as reusable role collections;
- routing work to specialists by task type.

Adapted here as:

- `packs/*.yaml`;
- routing tables from work type to specialist id;
- optional specialists that can be composed per project.

## Project-Specific Extension

The `playdate-platform-sdk` role is adapted from the Locksmith project needs:

- Playdate SDK/API quirks;
- crank/buttons/accelerometer behavior;
- graphics draw modes and 1-bit constraints;
- datastore and simulator/device differences;
- performance limits for a small handheld platform.
