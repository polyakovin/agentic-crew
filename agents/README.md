# Agent Harnesses

Each folder under `agents/` is a deployable specialist harness. The folder is
the unit to copy into a project or wrap with an A2A runtime.

Required files:

- `role.md`: human-readable specialist instructions.
- `agent-card.json`: A2A discovery card for the specialist.
- `harness.yaml`: local wiring for protocol, payloads, source policy, gates,
  and handoff behavior.
- `operations.md` or split operational docs: source map, workflow, tool policy,
  rubric, eval/release notes, and health expectations.
- `run-record.template.json`: structured record for non-trivial specialist
  runs.

Shared contracts:

- A2A profile: `../protocol/interaction-protocol.md`
- Agentic Crew payload schema: `../protocol/message-schema.json`

The harness keeps each specialist narrow. Do not broaden `always_read` into the
whole repository; task-specific context should come through an A2A `taskBrief`
payload.

High-risk or platform-heavy specialists may split `operations.md` into multiple
files such as `source-map.md`, `workflow.md`, `tool-policy.md`, `rubric.md`,
`eval-plan.md`, `release-rollback.md`, and `health-snapshot.md`.
