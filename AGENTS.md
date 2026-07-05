# Agentic Crew Project Rules

## Request Handling

Do not automatically route repository requests through the Orchestrator. Handle
the user's request directly unless the user explicitly asks for orchestration,
delegation, or a named specialist.

When delegation is requested, use the smallest useful specialist set and preserve
evidence, blockers, and handoff ownership using
`protocol/interaction-protocol.md`.

## Specialist Boundaries

Specialists are optional collaborators. Use them only when requested or when a
task genuinely needs their focused ownership:

- `product-manager` for requirements and acceptance criteria;
- `system-architect` for architecture, ownership, boundaries, and tradeoffs;
- `spec-contract-guardian` for manifest/spec/API/pure-logic contract drift;
- `implementation-engineer` for scoped code changes;
- `qa-reviewer` for regression, correctness, usability, and test coverage;
- `protocol-steward` for Agentic Crew protocol, harness, and pack governance.
- `agent-tester` for behavioral testing, exploratory testing, adversarial
  checks, improvement backlogs, and knowledge-base learning for agents.

When a request is simple and low risk, keep the work local.
