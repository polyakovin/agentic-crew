---
name: agentic-crew-author
description: Create, adapt, or review specialist-agent harness folders, A2A Agent Cards, and crew packs using the Agentic Crew A2A profile. Use when designing reusable agent teams, Codex skills, Codex custom subagents, Hermes-style specialists, routing tables, or handoff protocols for software projects.
---

# Agentic Crew Author

Use this skill to create or update specialist-agent teams based on the harness
folders, Agent Cards, and A2A profile in this repository.

## Required Sources

Read these before creating or changing a role:

1. `protocol/interaction-protocol.md`
2. Existing agent harness folders in `agents/`
3. Existing role cards, Agent Cards, and `harness.yaml` files for the closest
   matching agents
4. The target pack in `packs/`, if any
5. The target project's local specs, instructions, tests, and git history

## Harness Authoring Rules

- Every specialist must live in its own `agents/<id>/` folder.
- Every specialist folder must include `README.md`, `role.md`,
  `agent-card.json`, and `harness.yaml`.
- `harness.yaml` must name the A2A profile, payload schema, role card, Agent
  Card, accepted payloads, emitted payloads, context policy, quality gates, and
  handoff behavior.
- Packs should reference harness folders through an `agents:` list, not separate
  loose role/card lists.

## Role Authoring Rules

- Keep each role narrow.
- Keep `What To Read` narrow and role-specific.
- Include Mission, Use When, Scope, Non-goals, What To Read, Workflow,
  Minimum Deliverable, Quality Gates, Blockers, and Handoff Contract.
- Define the A2A `AgentSkill` metadata the role should expose.
- Require a `specialistReport` Agentic Crew payload or an A2A artifact as
  output.
- Do not invent a custom cross-agent protocol. Use A2A for discovery,
  messaging, tasks, parts, and artifacts.
- Do not copy third-party prompts verbatim.
- Treat external frameworks as structure donors, not source of truth.

## Agent Card Authoring Rules

- Every exported specialist should have an A2A Agent Card in its harness folder.
- Use `supportedInterfaces` for runtime endpoints. Keep template endpoints as
  placeholders until a runtime is deployed.
- Include `defaultInputModes` and `defaultOutputModes` with `text/plain` and
  `application/json` unless the runtime cannot support them.
- Keep `skills` narrow. A broad skill list is a sign the role should be split.

## Pack Authoring Rules

- Every pack must reference `protocol/interaction-protocol.md`.
- Every agent entry needs `id`, `path`, `harness`, and `required`.
- Routing keys should name work types, not vague departments.
- Prefer optional specialists unless a role is always needed.

## Review Checklist

- Does each role prevent a real failure mode?
- Is there overlap with another role?
- Is `What To Read` small enough for the role?
- Does each exported role have A2A AgentSkill metadata?
- Does each specialist have a complete `agents/<id>/` harness folder?
- Does the role have evidence-based quality gates?
- Does the role know when to block?
- Can the orchestrator route work using the pack's routing table?
