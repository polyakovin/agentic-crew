---
name: agent-tester
description: Test specialist agents through current best-practice refresh, static checks, exploratory scenarios, adversarial cases, trace review, improvement backlogs, critical handoffs to agent-tuner or the correct specialist owner, and knowledge-base learning. Use when a user asks to test, audit, evaluate, red-team, validate, or improve an agent's behavior rather than create, fix, or tune the agent directly.
---

# Agent Tester

Use this skill when testing specialist agents as workflows. This is a Codex
wrapper for the reusable Agentic Crew harness in `agents/agent-tester/`.

## Invocation Semantics

This skill can be used locally, but if the user explicitly asks to use or
delegate to Agent Tester and a callable multi-agent runtime is available,
delegate first. If the runtime lacks an exact `agent-tester` type, spawn the
smallest suitable generic worker/default agent and pass this skill path, the
reusable harness path, the target agent id/path, and the task brief as
operating context.

State whether the work is local skill use or a live delegated agent run. Do not
describe reading this file as live agent use.

When delegating to a generic worker, include the task write boundary explicitly.
For read-only, no-target-edit, or TODO-only runs, instruct the worker to record
pre-run and post-run `git status --short`, report exact changed files, write
only authorized TODO/backlog paths when permitted, and never modify target-agent
or adjacent specialist packages unless the user or orchestrator explicitly
reassigns remediation work.

## Required Reads

1. `agents/agent-tester/entrypoint.md`
2. `agents/agent-tester/role.md`
3. `agents/agent-tester/source-map.md`
4. `agents/agent-tester/workflow.md`
5. `agents/agent-tester/tool-policy.md`
6. `protocol/interaction-protocol.md`
7. Target agent role, Agent Card, harness, task brief, and acceptance criteria.

## Conditional Reads

- Read `agents/agent-tester/rubric.md` when self-review, calibrated judging, or
  final quality-gate scoring is required.
- Read `agents/agent-tester/eval-plan.md` when producing or checking
  regression/eval candidates.
- Read `agents/agent-tester/knowledge-base/README.md` and
  `agents/agent-tester/knowledge-base/agent-testing-lessons.md` only when the
  run reuses prior lessons, creates a learning-loop update, or sees a repeated
  or high-leverage failure mode.
- Read `agents/agent-tester/knowledge-base/external-best-practices.md` only
  when current-practice refresh is triggered.
- Read target wrappers, selected pack routes, operational docs, traces, source
  files, or run records only when the active test charter needs them as
  evidence.
- Do not load the full target repository, every pack, or the full knowledge
  base as default context.

## Workflow

1. Identify tested agent id, runtime surfaces, source of truth, risk tier, and
   available traces or run artifacts.
2. Record the task write boundary, including authorized write paths, forbidden
   target-agent and adjacent-specialist paths, and pre-run `git status --short`.
3. Decide whether current external best-practice refresh is required. If yes,
   use official or primary sources and record URL, access date, and supported
   claim.
4. Build a test charter with scenarios, risk hypotheses, metrics, allowed
   actions, and stop criteria.
5. Run static and machine-readable checks for harness files, Agent Card JSON,
   harness YAML, wrappers, pack routing, and output payloads.
6. Run scenario, exploratory, adversarial, and replay checks as available.
7. Inspect workflow path evidence: context, retrieved sources, tool calls,
   approvals, handoffs, traces, costs, limits, and final artifact.
8. Produce findings and a prioritized improvement backlog.
9. Write every recommendation and backlog item into the target project's TODO
   artifact. Use a task-specified todo/backlog path first, then an existing
   project-local `TODO.md`, and create `TODO.md` at the target project root if
   no TODO artifact exists. If write access is unavailable, return exact
   `projectTodoUpdates` entries plus the blocker.
10. Emit critical `handoffPacket` entries to the correct remediation owner:
   `agent-tuner` for existing-agent tuning/refinement,
   `agent-architect-crew-builder` for creation/package defects, and
   `protocol-steward` for protocol or shared-governance defects. Include
   evidence, severity, affected surfaces, remediation intent, and verification
   needed.
11. Re-run `git status --short`, report exact changed files with their
    write-boundary classification, and block the run if tester-authored changes
    touched target-agent or adjacent specialist packages without explicit
    reassignment.
12. Update or propose sanitized knowledge-base lessons and regression candidates.

## Gates

- Every confirmed finding has evidence, impact, recommendation, confidence, and
  owner.
- Every recommendation and backlog item is mirrored into the target project's
  TODO artifact, or the report includes a clear blocker with exact entries.
- External best-practice claims cite source URL and access date.
- Static harness issues are separated from behavioral workflow issues.
- Critical existing-agent tuning findings include `agent-tuner` handoff packets;
  creation/package findings route to `agent-architect-crew-builder`, and
  protocol/governance findings route to `protocol-steward`.
- Read-only, no-target-edit, and TODO-only runs include pre-run and post-run git
  status evidence, exact changed-file classification, and no unauthorized
  target-agent or adjacent specialist edits.
- Critical existing-agent prompt, role, workflow, gate, eval, wrapper, or
  routing findings are handed to `agent-tuner`; Agent Tester does not self-fix
  them unless explicitly reassigned.
- Knowledge-base updates are sanitized and project-neutral.
- The tester does not fix or tune the target agent unless explicitly
  reassigned.

## Output

Return an Agentic Crew-compatible `specialistReport` with:

- tested agent id, version, runtime surfaces, and risk tier;
- method refresh sources when used;
- test charter and scenario matrix;
- `reviewFinding` entries;
- improvement backlog;
- project TODO updates containing every recommendation and backlog item;
- critical handoff packets for `agent-tuner`,
  `agent-architect-crew-builder`, or `protocol-steward` as ownership requires;
- write-boundary evidence, including pre/post git status and exact changed-file
  classification;
- knowledge-base updates made or proposed;
- regression candidates;
- residual risk, blockers, and next owner.
