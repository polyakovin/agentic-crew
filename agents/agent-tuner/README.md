# Agent Tuner

High-risk specialist harness for refining already described or draft specialist
agents without taking over agent creation, independent testing, or protocol
governance.

Status: draft-ready portable harness for the post-creation gate. Pilot and
production remain blocked pending future pilot run records, eval evidence, and
release-gate review.

This agent turns an existing agent definition or proposal into a sharper,
safer, more routable specialist package by:

- narrowing mission, triggers, non-goals, and handoff boundaries;
- reducing overlap with neighboring specialists;
- improving role cards, prompts, workflows, gates, eval seeds, and routing
  recommendations;
- producing bounded patch plans or scoped edits to agent infrastructure;
- removing completed TODO/backlog tasks that the tuning run resolves, while
  leaving unresolved work open;
- preserving tuning evidence, residual risk, and next-owner handoffs.

The Agent Tuner is intentionally separate from:

- `agent-architect-crew-builder`, which creates and packages new specialists;
- `agent-tester`, which tests, audits, red-teams, and evaluates agent behavior;
- `protocol-steward`, which owns Agentic Crew protocol and governance changes.

## Files

- `entrypoint.md`: mission, calibration, and entry instructions.
- `role.md`: role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, trust boundaries, and ownership rules.
- `workflow.md`: tuning workflow, validation commands, and escalation rules.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review checklist for tuning outputs.
- `eval-plan.md`: seed eval families and promotion thresholds.
- `run-record.template.json`: structured record for tuning runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: scope decision, reuse analysis, capability inventory,
  ownership audit, and current blockers.

Related wrappers:

- `../../.agents/skills/agent-tuner/SKILL.md`
- `../../.hermes/skills/agent-tuner.md`
- `../../.hermes/agents/agent-tuner/`
