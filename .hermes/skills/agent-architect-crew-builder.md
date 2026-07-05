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
Tester review before status promotion.

## Required Context

- Target project approved demand plan.
- Target project agent rules and forbidden local runtime surfaces.
- Existing Agentic Crew harnesses and selected pack.
- Current target-project agent обвязка that may need moving into specialist
  scope.
- Local `../ai-db` canonical harness capability references.
- `agents/agent-architect-crew-builder/role.md`
- `agents/agent-architect-crew-builder/workflow.md`
- `agents/agent-architect-crew-builder/tool-policy.md`

## Required Output

- A created, updated, rejected, or blocked decision.
- Creation scope decision.
- Existing harness reuse/adaptation analysis.
- Harness capability inventory decisions.
- Agent Tester review result and unresolved findings.
- File list for changed agent and wrapper artifacts.
- Ownership extraction summary.
- Validation results.
- Commit/push result.
- Promotion status.
- Handoff owner for unresolved work.

## Stop Conditions

- Missing target crew path.
- Missing approval for a new role.
- Duplicate specialist found.
- Harness capability inventory cannot be completed.
- Agent Tester review is missing or reports unresolved critical findings.
- Required Hermes package shape cannot be determined.
- Validation fails.
