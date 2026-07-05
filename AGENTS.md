# Agentic Crew Project Rules

## Orchestrator-First Handling

Every user request in this repository must enter through the Orchestrator before
implementation, review, documentation, or specialist authoring work begins.

Exception: when the user explicitly assigns a task to a specific specialist or
named builder agent, do not spawn, message, or otherwise call a live Orchestrator
agent for help. Treat the user's explicit agent choice as the routing decision,
record only the minimal local routing note needed for evidence, and route
directly to the named agent. Escalate back to Orchestrator only when the named
agent is unavailable, the request conflicts with project rules, or the user asks
for orchestration.

For explicit specialist or named builder requests, live delegation is still
required whenever any callable multi-agent runtime is available. If the runtime
does not expose an exact specialist type, spawn the smallest suitable generic
worker/default agent and pass the named specialist's skill path, harness path,
role card, and task brief as its operating context. Do not treat the absence of
an exact runtime role name as a local-fallback condition. Use local specialist
fallback only when no callable runtime can accept the delegated brief, or when
current higher-priority tool policy forbids spawning.

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
- Do not use live Orchestrator delegation for explicit specialist requests
  covered by the Orchestrator-First exception above.
- For explicit specialist requests, discover live delegation tools first and
  delegate directly to the named specialist or to a generic worker/default agent
  loaded with that specialist's harness. Report the spawned agent id/name and
  result.
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
- `agent-tester` for behavioral testing, exploratory testing, adversarial
  checks, improvement backlogs, and knowledge-base learning for agents.

When a request is simple and low risk, the Orchestrator may keep the work local
after recording the routing decision and explaining why no specialist handoff is
needed.
