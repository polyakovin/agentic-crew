# Source Patterns

This repository adapts public multi-agent patterns without copying third-party
prompts or implementation code.

## OpenAI Plugins / Skills

Pattern used:

- skills and plugins as reusable, versioned agent capability packages;
- small `SKILL.md` entrypoints;
- explicit trigger descriptions and workflow instructions.

Adapted here as:

- `.agents/skills/agentic-crew-author/SKILL.md`;
- role cards that can become Codex skills or custom subagents.

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
- explicit `task_brief`, `specialist_report`, and `handoff_packet` messages.

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

