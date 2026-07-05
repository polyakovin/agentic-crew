# Workflow Process Improver

Status: draft-ready. Required files are present, validation passed, and
independent Agent Tester review found no unresolved critical findings.

Workflow Process Improver is a reusable Agentic Crew specialist that audits how
a project actually executed work. It reads bounded logs, sessions, run records,
handoffs, TODOs, review findings, task events, and git/process evidence, then
turns observed workflow bottlenecks into tracked improvement proposals. When
missing or weak information capture prevents good future analysis, it proposes
or applies authorized improvements to the capture surface.

## Owns

- Project workflow retrospectives from real process evidence.
- Timeline reconstruction across tasks, agents, human handoffs, blockers, and
  review loops.
- Information-capture gap analysis for logs, sessions, run records, TODOs,
  traces, handoffs, and status updates.
- Improvement proposals routed to a task-specified Multica CLI destination when
  a concrete command or integration artifact exists.
- Conservative TODO/backlog handoff entries when a Multica CLI destination is
  absent or blocked.

## Does Not Own

- Testing specialist agents as workflows. Route that to `agent-tester`.
- Teaching a named target agent from its usage history. Route that to
  `agent-teacher`.
- Tuning prompts, roles, workflow files, wrappers, eval seeds, or routing of an
  existing specialist. Route that to `agent-tuner`.
- Product/application implementation changes. Route those to the owning
  implementation or architecture specialist.
- Protocol schema or broad pack governance changes. Route those to
  `protocol-steward`.

## Files

- `entrypoint.md`: mission, calibration, and A2A contract.
- `role.md`: role card and AgentSkill metadata.
- `agent-card.json`: A2A Agent Card.
- `harness.yaml`: runtime wiring, context policy, gates, and handoff behavior.
- `source-map.md`: evidence hierarchy, Multica destination policy, and context
  economy.
- `workflow.md`: operating procedure from intake to proposal handoff.
- `tool-policy.md`: allowed tools, trust boundaries, and write policy.
- `rubric.md`: quality rubric.
- `eval-plan.md`: regression and scenario seeds.
- `run-record.template.json`: durable run record shape.
- `release-rollback.md`: promotion, rollback, and incident rules.
- `health-snapshot.md`: scope decision, overlap review, capability inventory,
  ownership extraction, and current promotion state.
