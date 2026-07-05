---
name: agentic-crew-author
description: Create, adapt, or review new specialist agents with the right harness for the target runtime, including Agentic Crew folders, A2A Agent Cards, Codex skills/subagents, Hermes-style specialists, routing tables, packs, run records, and handoff protocols. Use when the user asks to create, add, scaffold, generalize, port, or package agents.
---

# Agentic Crew Author

Use this skill to create, adapt, or review specialist agents with the harness
they need for the target runtime and risk level.

This skill's job is not to write a short role note. Its job is to decide the
correct harness shape, create all required files, wire the agent into
packs/routing when requested, and leave validation evidence.

The output is agent infrastructure:

- `agents/<id>/` Agentic Crew harness folders;
- A2A Agent Cards and `harness.yaml`;
- Codex skills or custom-agent wrappers;
- Hermes-style specialist entrypoints/packages;
- routing tables, crew packs, handoff contracts, run-record templates, rubrics,
  eval plans, release/rollback notes, and import snapshots when needed.

The output is not product/game/application code unless the user explicitly asks
for an example target project fixture.

## Invocation Semantics

This `SKILL.md` is an authoring workflow, not a live specialist process. Reading
or following this file counts as using the `agentic-crew-author` skill; it does
not count as spawning or consulting a separate agent.

When the user explicitly asks to "use an agent", "run an agent", "delegate to an
agent", "spawn a subagent", "воспользуйся агентом", "запусти агента", or similar:

- First determine whether the requested name is a callable runtime agent, a
  Codex skill, an Agentic Crew harness folder, or only a role/package template.
- If a callable subagent or multi-agent runtime is available, discover it before
  doing the main work. In Codex contexts, use `tool_search` for multi-agent
  tools and prefer the runtime's spawn/delegate tool, such as
  `multi_agent_v1.spawn_agent`, when the user has explicitly authorized
  delegation.
- If the requested specialist exists only as a skill, harness folder, or role
  package and the runtime does not expose an exact matching agent type, that is
  not enough to fall back to local-only execution. Spawn the smallest suitable
  generic worker/default agent and pass the specialist skill path, harness path,
  role card, and task brief as operating context.
- Pass the relevant skill path, harness path, role card, or task brief to the
  spawned agent when the runtime supports structured inputs.
- Report the spawned agent id/name and its result or status in the final answer.
- If no callable runtime agent is available, say so before doing the work and
  state the fallback explicitly: for example, "I can apply the
  `agentic-crew-author` skill locally, but I cannot spawn it as a subagent in
  this environment."
- Do not describe reading a skill file, copying a harness, or following a role
  card as "using a subagent" unless a separate runtime agent was actually
  spawned or messaged.

If the user asks to create or change agent infrastructure and also asks for a
separate agent to participate, both requirements matter: use this skill for the
authoring rules, and use a callable subagent/runtime for the delegated portion
when available.

## Required Sources

Read these before creating or changing an agent:

1. `protocol/interaction-protocol.md`
2. Existing agent harness folders in `agents/`
3. Existing role cards, Agent Cards, and `harness.yaml` files for the closest
   matching agents
4. The target pack in `packs/`, if any
5. The target project's local specs, instructions, tests, and git history

## Intake

For every request, determine:

- requested invocation mode: local skill use, live subagent delegation, A2A
  runtime specialist, Codex custom subagent, harness authoring only, or hybrid;
- target runtime: Agentic Crew/A2A, Codex skill, Codex custom subagent, Hermes,
  project-local wrapper, or hybrid;
- whether this is a new agent, a port of an existing agent, a project import, or
  a harness review;
- agent id, audience, owning project, risk level, and expected routing surface;
- whether the user wants a reusable template, a project-specific specialist, or
  both;
- whether existing project files must be moved out of another repo and preserved
  as source material.

Default assumptions:

- For Agentic Crew library work, create or update `agents/<id>/`.
- For project-local agent migration, preserve an exact import snapshot before
  deleting source files.
- For high-risk or future-agent behavior changes, prefer a full split harness
  over a compact single-file role.
- Keep new packages `draft`/`draft-ready`, not `production`, until pilot records,
  eval evidence, and independent review exist.
- Never let ambiguous wording hide the execution mode. If the user asked for an
  agent but only a skill or harness is available, name that distinction before
  proceeding.

## Harness Selection

Choose the smallest harness that is still complete for the intended runtime:

| Target | Required surface |
|---|---|
| Agentic Crew/A2A | `agents/<id>/README.md`, `role.md`, `agent-card.json`, `harness.yaml`, operational docs, `run-record.template.json` |
| High-risk Agentic Crew/A2A | Split docs: `entrypoint.md`, `source-map.md`, `workflow.md`, `tool-policy.md`, `rubric.md`, `eval-plan.md`, `release-rollback.md`, `health-snapshot.md` |
| Codex reusable workflow | `.agents/skills/<id>/SKILL.md` with mission, triggers, sources, workflow, gates, output rules |
| Codex custom subagent | `.codex/agents/<id>.toml` plus `.codex/agents/<id>.instructions.md` |
| Hermes-style specialist | `.hermes/skills/<id>.md` plus `.hermes/agents/<id>/` package when risk or user expectation requires full harness |
| Project import/snapshot | `projects/<project>/current-agent-files/` or another named import folder with README and exact copied roots |
| Crew routing | `packs/<pack>.yaml`, routing keys, required/optional flags, and handoff notes |

When the user says "create a new agent", "add an agent", "завести агента",
"оформить специалистов", or points to a TODO/list of agents, assume they want a
complete harness, not a lightweight reusable note, unless they explicitly ask for
a tiny draft.

## Harness Authoring Rules

- Every Agentic Crew specialist must live in its own `agents/<id>/` folder.
- Every specialist folder must include `README.md`, `role.md`,
  `agent-card.json`, `harness.yaml`, operational docs, and
  `run-record.template.json`.
- `harness.yaml` must name the A2A profile, payload schema, role card, Agent
  Card, accepted payloads, emitted payloads, context policy, quality gates,
  operational references, run-record template, and handoff behavior.
- Standard specialists may use one `operations.md` file containing source map,
  workflow, tool policy, rubric, eval seeds, release notes, and health notes.
- High-risk specialists should split operations into files such as
  `entrypoint.md`, `source-map.md`, `workflow.md`, `tool-policy.md`,
  `rubric.md`, `eval-plan.md`, `release-rollback.md`, and
  `health-snapshot.md`.
- Packs should reference harness folders through an `agents:` list, not separate
  loose role/card lists.
- Project-specific imports should be labeled as imports/snapshots, not silently
  mixed into reusable `agents/<id>/` folders.

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
- Include non-goals, blockers, risk gates, and what must be handed to another
  specialist.

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

## Creation Workflow

1. Resolve invocation semantics: local skill, live subagent, A2A specialist
   package, or hybrid. If live delegation was requested, use the available
   runtime tool or disclose that only local skill/harness use is possible.
2. Identify target runtime and harness type.
3. Read closest existing harnesses and protocol docs.
4. Draft the role boundary: mission, use when, scope, non-goals, blockers,
   quality gates, handoff contract.
5. Create the required harness files for that runtime.
6. Add Agent Card skills and `harness.yaml` accepted/emitted payloads.
7. Add or update pack/routing entries when the agent should be discoverable by a
   crew.
8. Add run-record template, rubric, eval seed, and release/rollback notes when
   risk or persistence requires it.
9. Validate machine-readable files: JSON, YAML/TOML when applicable.
10. Check trailing whitespace and links/paths.
11. Report created paths, validation, unresolved gaps, promotion status, and
    whether the work used a local skill, spawned agent, or runtime fallback.

## Review Checklist

- Does the request need a full harness rather than a short instruction file?
- Did the response distinguish skill use from live subagent/runtime use?
- If the user explicitly asked to use an agent, was a callable agent spawned or
  was the lack of a runtime disclosed before fallback work?
- Is the target runtime explicit or reasonably inferred?
- Does each role prevent a real failure mode?
- Is there overlap with another role?
- Is `What To Read` small enough for the role?
- Does each exported role have A2A AgentSkill metadata?
- Does each specialist have a complete `agents/<id>/` harness folder or the
  equivalent required surface for Codex/Hermes?
- Does the harness include operational docs and a run-record template?
- Does the role have evidence-based quality gates?
- Does the role know when to block?
- Can the orchestrator route work using the pack's routing table?
- Are project-specific assumptions isolated from reusable templates?
- Are imported source files preserved before deletion from a source repo?

## Review Output

When reviewing an existing agent, lead with defects:

- missing required harness files;
- claimed agent/subagent use when only a skill or role file was read;
- user asked for live delegation but no runtime attempt or fallback disclosure
  occurred;
- unclear target runtime;
- role too broad or overlapping;
- untestable quality gates;
- missing handoff contract;
- missing machine-readable validation;
- production status claimed without pilot/eval/review evidence.
