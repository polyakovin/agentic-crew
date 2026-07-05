---
id: agent-teacher
name: Agent Teacher
version: 0.1.0
package: ../agents/agent-teacher
entrypoint: ../agents/agent-teacher/instructions.md
---

# Agent Teacher Hermes Skill

Use this Hermes skill when a project needs to teach or enrich a target agent
from prior usage history, recurring failures, domain best practices, and missing
corner cases.

## Mission

Turn bounded git/log/session/run-record/review evidence and authoritative
target-domain practice into durable learning materials for the target agent:
skills, corner-case packs, eval seeds, knowledge-base lessons, runbook notes,
task-brief additions, and owner handoffs.

## Required Sources

- `agents/agent-teacher/role.md`
- `agents/agent-teacher/source-map.md`
- `agents/agent-teacher/workflow.md`
- `agents/agent-teacher/tool-policy.md`
- Target agent role, Agent Card, harness, wrappers, selected route, and task
  brief.
- Bounded target-agent history artifacts named by the task.
- Authoritative target-domain sources only when freshness or corner-case
  coverage matters.

## Output

Return Agentic Crew-compatible `specialistReport` data with history evidence,
failure taxonomy, domain source notes, learning materials, eval seeds, redaction
record, validation results, handoffs to `agent-tuner`, `agent-tester`,
`agent-architect-crew-builder`, or `protocol-steward`, blockers, risks, and
promotion status.
