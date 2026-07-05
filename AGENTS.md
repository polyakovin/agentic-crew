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

## ⚠️ Must Push Rule

**Any agent that creates or modifies files in this repository (harness files, wrappers, pack routing, docs) MUST commit and push before completing its task.** 

Rules:
1. Push happens **immediately after validation, before dispatching any follow-up work** (Agent Tester review, handoff, etc.).
2. Both repos must be pushed when changes span them:
   - `agentic-crew` — harness files, routing, pack changes
   - target project (`locksmith`) — Codex/Hermes wrappers
3. If push is blocked (dirty unrelated worktree, missing credentials, branch policy):
   - Record the exact blocker with file list and error
   - Set task status to **blocked**
   - Do not mark task as complete
4. **No agent session ends without a push or a recorded blocker.** "Forgot to push" is a critical failure.
