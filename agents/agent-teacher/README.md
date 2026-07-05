# Agent Teacher

Status: draft-ready portable harness. Pilot and production remain blocked until
sample learning packets, eval evidence, and pilot records exist.

Agent Teacher is a reusable Agentic Crew specialist that enriches an existing
or draft target agent from real usage history and authoritative domain practice.
It mines bounded git history, logs, sessions, traces, run records, review
findings, TODOs, and prior handoffs, then prepares durable learning materials so
the target agent improves on future runs.

## Owns

- Usage-history analysis for a named target agent.
- Recurring-failure and missing-corner-case discovery.
- Target-domain best-practice refresh from official or primary sources when
  current domain facts matter.
- Durable learning packets: skill drafts, corner-case packs, eval seeds,
  knowledge-base lessons, runbook notes, task-brief additions, and handoff
  packets.
- Handoffs to `agent-tuner` when the target agent's prompt, role, workflow,
  wrappers, eval seeds, or routing need actual tuning edits.

## Does Not Own

- Independent behavioral testing or red-team review. Route that to
  `agent-tester`.
- Applying prompt or workflow tuning to an existing agent unless explicitly
  reassigned. Route that to `agent-tuner`.
- Creating or packaging new specialists. Route that to
  `agent-architect-crew-builder`.
- Protocol governance. Route that to `protocol-steward`.
- Product or application code changes.

## Files

- `entrypoint.md`: mission, calibration, and A2A contract.
- `role.md`: role card and AgentSkill metadata.
- `agent-card.json`: A2A Agent Card.
- `harness.yaml`: runtime wiring, context policy, gates, and handoff behavior.
- `source-map.md`: evidence hierarchy, history-source policy, and material
  ownership.
- `workflow.md`: operating procedure from intake to learning packet.
- `tool-policy.md`: allowed tools, trust boundaries, and write policy.
- `rubric.md`: quality rubric.
- `eval-plan.md`: regression and scenario seeds.
- `run-record.template.json`: durable run record shape.
- `release-rollback.md`: promotion, rollback, and incident rules.
- `health-snapshot.md`: scope decision, overlap review, capability inventory,
  ownership extraction, and current promotion state.
