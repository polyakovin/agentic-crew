# Agentic Crew Project Rules

## Orchestrator-First Handling

Every user request in this repository must enter through the Orchestrator before
implementation, review, documentation, or specialist authoring work begins.

For each request:

1. Read and apply `agents/orchestrator/role.md` and
   `agents/orchestrator/operations.md`.
2. Classify the request, risk, source of truth, and likely pack. Use
   `packs/software-development-crew.yaml` by default unless the task clearly
   belongs to another pack.
3. Produce an Orchestrator routing decision, even if the decision is "no
   additional specialist is needed."
4. Route to the smallest useful specialist set. Do not send every task to every
   specialist.
5. Preserve evidence, blockers, and handoff ownership using
   `protocol/interaction-protocol.md`.

## Live Agent Versus Local Fallback

The Orchestrator is preferred as a live runtime agent when a callable
multi-agent runtime is available and the current system/developer instructions
allow spawning or messaging agents.

- If live delegation is available, discover the tool first, spawn or message the
  Orchestrator, and report the spawned agent id/name and result.
- If live delegation is unavailable or disallowed, apply the Orchestrator role
  locally and state that this is a local orchestration fallback.
- Do not describe reading `role.md`, `operations.md`, a skill file, or a harness
  folder as live agent use unless a separate runtime agent was actually spawned
  or messaged.

## Specialist Boundaries

The Orchestrator is the front door, not a replacement for specialists. It should
handoff to:

- `product-manager` for requirements and acceptance criteria;
- `system-architect` for architecture, ownership, boundaries, and tradeoffs;
- `spec-contract-guardian` for manifest/spec/API/pure-logic contract drift;
- `implementation-engineer` for scoped code changes;
- `qa-reviewer` for regression, correctness, usability, and test coverage;
- `protocol-steward` for Agentic Crew protocol, harness, and pack governance.

When a request is simple and low risk, the Orchestrator may keep the work local
after recording the routing decision and explaining why no specialist handoff is
needed.
