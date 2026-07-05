---
name: agent-architect-crew-builder
description: Create or review complete specialist-agent packages across Agentic Crew/A2A, Codex, and Hermes surfaces, including Agent Cards, harness YAML, wrappers, routing, rubrics, eval plans, run records, and validation evidence. Use when a project asks to create, package, route, or review a specialist agent.
---

# Agent Architect / Crew Builder

Use this skill when authoring or reviewing specialist-agent infrastructure. This
is a Codex wrapper for the full harness in
`agents/agent-architect-crew-builder/`.

## When To Use

- A user asks to create a new specialist agent.
- A target project has an approved demand plan or creation order.
- A specialist must ship with Codex and Hermes support.
- A pack needs routing for agent creation, harness authoring, or wrapper work.
- An existing agent proposal may duplicate another specialist.
- A specialist harness must decide which `ai-db` capabilities to use, defer, or
  reject.

## Required Reads

1. `agents/agent-architect-crew-builder/entrypoint.md`
2. `agents/agent-architect-crew-builder/role.md`
3. `agents/agent-architect-crew-builder/source-map.md`
4. `agents/agent-architect-crew-builder/workflow.md`
5. `agents/agent-architect-crew-builder/tool-policy.md`
6. `agents/agent-architect-crew-builder/rubric.md`
7. `protocol/interaction-protocol.md`
8. `plans/agent-harness-creation-plan.md`
9. Target project demand plan and rules supplied by the task

## Workflow

1. State whether work is local skill use or a live delegated agent.
   When the user explicitly asks for Crew Builder or another named builder
   agent and a callable multi-agent runtime exists, delegate to that runtime
   before implementation. If no exact `agent-architect-crew-builder` runtime
   role exists, spawn the smallest suitable generic worker/default agent with
   this skill path, the harness folder, and the task brief. Only use local skill
   fallback when no callable runtime can accept the delegated brief or when
   higher-priority tool policy forbids spawning.
2. Classify runtime surfaces: A2A, Codex, Hermes, or hybrid.
3. Decide creation scope: target project, Agentic Crew, or hybrid.
4. Check existing agents/wrappers for overlap and reusable harness candidates.
5. Run the full Harness Capability Inventory from `../ai-db`, marking every
   category `use`, `defer`, or `reject` with rationale.
6. Run SOLID ownership extraction audit for shared agent data/tooling.
7. Create or adapt the smallest complete package.
8. Move specialist-owned agent обвязка into the specialist package when it no
   longer serves the general project.
9. Wire pack routing only when the specialist should be discoverable.
10. Validate JSON/YAML/TOML/Markdown whitespace.
11. Request Agent Tester review for created or materially updated specialists.
12. Block on Agent Tester critical findings, or record non-critical backlog and
    proceed only when review is complete.
13. After Agent Tester returns, mirror follow-up tasks into the task-specified
    TODO or backlog artifact and hand their execution to `agent-tuner` with
    evidence, severity, remediation intent, and verification needed.
14. Commit and push scoped changes, or record the exact blocker.
15. Return a full Agentic Crew `specialistReport` envelope with files changed,
    scope decision, reuse analysis, capability inventory, ownership extraction,
    validation evidence, Agent Tester result, commit/push result, blockers, and
    promotion status in `metadata.details`, not a loose summary.

## Gates

- Do not create a new agent without approved target demand or a recorded
  decision.
- Do not write files before recording scope decision.
- Do not create a new harness from scratch when an existing compatible harness
  can be reused, wrapped, or adapted.
- Do not skip any `ai-db` harness capability category, and do not mark every
  category `use` without role-specific rationale.
- Do not mark created or promotion-ready before Agent Tester review is recorded.
- Do not create target-local `.agents`, `.codex`, or `.hermes` when the target
  plan forbids local runtime surfaces.
- Do not leave specialist-only prompts, tools, evals, rubrics, wrappers, or
  checklists duplicated in shared project space without rationale.
- Do not move product source, specs, assets, tests, or domain data unless the
  user explicitly asks for that product-architecture change.
- Do not mark production-ready without pilot records, eval evidence, and
  independent review.
- Do not leave Agent Tester follow-up tasks outside the task-specified TODO or
  backlog artifact, and do not skip the `agent-tuner` execution
  handoff when follow-up work remains.
- Do not commit or push unrelated dirty worktree changes.
- Do not report completion if push is blocked.
- Hermes support is complete only when both skill and package files exist and
  the manifest validates.

## Output

Return a full Agentic Crew `specialistReport` envelope, not an informal summary.
The top-level payload must include:

- `profile`: `agentic-crew/a2a-profile/v0.1`;
- `kind`: `specialistReport`;
- `taskId`;
- `specialistId`: `agent-architect-crew-builder`;
- `status`;
- `summary`;
- `evidence`;
- `findings`;
- `recommendations`;
- `risks`;
- `blockers`;
- `handoff`;
- `metadata`.

Place Crew Builder-specific fields in `metadata.details`, including:

- created or rejected agent id;
- runtime surfaces;
- scope decision;
- reuse analysis;
- capability inventory;
- files changed;
- ownership extraction;
- routing changes;
- validation results;
- Agent Tester review result;
- TODO/backlog updates for Agent Tester follow-up tasks;
- Agent Tuner execution handoff;
- commit/push result;
- promotion status;
- next owner.
