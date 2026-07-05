---
name: agent-tester
description: Test specialist agents through current best-practice refresh, static checks, exploratory scenarios, adversarial cases, trace review, improvement backlogs, critical handoffs to a future agent-fixer, and knowledge-base learning. Use when a user asks to test, audit, evaluate, red-team, validate, or improve an agent's behavior rather than create or fix the agent directly.
---

# Agent Tester

Use this skill when testing specialist agents as workflows. This is a Codex
wrapper for the reusable Agentic Crew harness in `agents/agent-tester/`.

## Invocation Semantics

This skill can be used locally, but if the user explicitly asks to use or
delegate to Agent Tester and a callable multi-agent runtime is available,
delegate first. If the runtime lacks an exact `agent-tester` type, spawn the
smallest suitable generic worker/default agent and pass this skill path, the
target agent path, and the task brief as operating context.

State whether the work is local skill use or a live delegated agent run. Do not
describe reading this file as live agent use.

## Required Reads

1. `agents/agent-tester/entrypoint.md`
2. `agents/agent-tester/role.md`
3. `agents/agent-tester/source-map.md`
4. `agents/agent-tester/workflow.md`
5. `agents/agent-tester/tool-policy.md`
6. `agents/agent-tester/rubric.md`
7. `agents/agent-tester/knowledge-base/README.md`
8. `agents/agent-tester/knowledge-base/agent-testing-lessons.md`
9. `protocol/interaction-protocol.md`
10. Target agent role, Agent Card, harness, wrappers, pack route, and run
    artifacts named by the task.

## Workflow

1. Identify tested agent id, runtime surfaces, source of truth, risk tier, and
   available traces or run artifacts.
2. Decide whether current external best-practice refresh is required. If yes,
   use official or primary sources and record URL, access date, and supported
   claim.
3. Build a test charter with scenarios, risk hypotheses, metrics, allowed
   actions, and stop criteria.
4. Run static and machine-readable checks for harness files, Agent Card JSON,
   harness YAML, wrappers, pack routing, and output payloads.
5. Run scenario, exploratory, adversarial, and replay checks as available.
6. Inspect workflow path evidence: context, retrieved sources, tool calls,
   approvals, handoffs, traces, costs, limits, and final artifact.
7. Produce findings and a prioritized improvement backlog.
8. Emit critical `handoffPacket` entries to `agent-fixer`; mark
   `blocked-planned-specialist` while that future specialist is unavailable.
9. Update or propose sanitized knowledge-base lessons and regression candidates.

## Gates

- Every confirmed finding has evidence, impact, recommendation, confidence, and
  owner.
- External best-practice claims cite source URL and access date.
- Static harness issues are separated from behavioral workflow issues.
- Critical findings include `agent-fixer` handoff packets.
- Knowledge-base updates are sanitized and project-neutral.
- The tester does not fix the target agent unless explicitly reassigned.

## Output

Return an Agentic Crew-compatible `specialistReport` with:

- tested agent id, version, runtime surfaces, and risk tier;
- method refresh sources when used;
- test charter and scenario matrix;
- `reviewFinding` entries;
- improvement backlog;
- critical handoff packets for `agent-fixer`;
- knowledge-base updates made or proposed;
- regression candidates;
- residual risk, blockers, and next owner.
