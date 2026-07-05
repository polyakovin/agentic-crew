---
id: agent-architect-crew-builder
name: Agent Architect / Crew Builder
version: 0.1.0
package: ../agents/agent-architect-crew-builder
entrypoint: ../agents/agent-architect-crew-builder/instructions.md
---

# Agent Architect / Crew Builder Hermes Skill

Use this Hermes skill when a project asks to create, package, review, or route a
specialist agent and Hermes support is required.

## Mission

Create complete specialist-agent packages with aligned Agentic Crew/A2A, Codex,
and Hermes surfaces. Prevent duplicate roles, missing wrappers, broad
permissions, false production readiness, and specialist-owned tooling left in
shared project scope. Walk the full `ai-db` harness capability inventory for
each specialist and mark every category `use`, `defer`, or `reject` with
rationale. After creating or materially updating a specialist, request Agent
Tester review before status promotion, write any follow-up tasks into the
task-specified TODO or backlog artifact, and hand execution to `agent-tuner`.

## Required Context

- Target project approved demand plan.
- Target project agent rules and forbidden local runtime surfaces.
- Existing Agentic Crew harnesses and selected pack.
- Current target-project agent обвязка that may need moving into specialist
  scope.
- Local `../ai-db` canonical harness capability references.
- `agents/agent-architect-crew-builder/role.md`
- `agents/agent-architect-crew-builder/source-map.md`
- `agents/agent-architect-crew-builder/workflow.md`
- `agents/agent-architect-crew-builder/tool-policy.md`
- `agents/agent-architect-crew-builder/rubric.md`
- `protocol/interaction-protocol.md`
- `plans/agent-harness-creation-plan.md`

## Required Output

- A full Agentic Crew `specialistReport` envelope, not an informal summary.
- Required top-level fields: `profile`, `kind`, `taskId`, `specialistId`,
  `status`, `summary`, `evidence`, `findings`, `recommendations`, `risks`,
  `blockers`, `handoff`, and `metadata`.
- Crew Builder-specific fields in `metadata.details`: decision, created or
  rejected agent id, runtime surfaces, creation scope, reuse/adaptation
  analysis, capability inventory decisions, Agent Tester review result and
  unresolved findings, TODO/backlog updates, Agent Tuner execution handoff,
  changed files, ownership extraction, routing changes, validation results,
  commit/push result, promotion status, and next owner.

## Stop Conditions

- Missing target crew path.
- Missing approval for a new role.
- Duplicate specialist found.
- Harness capability inventory cannot be completed.
- Agent Tester review is missing or reports unresolved critical findings.
- Agent Tester follow-up tasks cannot be written to a TODO/backlog artifact or
  handed to `agent-tuner`.
- Required Hermes package shape cannot be determined.
- Validation fails.
