# Agent Architect / Crew Builder

High-risk specialist harness for designing, creating, reviewing, and packaging
other specialist agents.

Status: draft-ready portable harness.

This agent turns a target project's approved demand list into complete agent
infrastructure:

- Agentic Crew/A2A harness folder;
- A2A Agent Card;
- Codex skill or custom-agent wrapper when requested;
- Hermes package/wrapper when requested or required by the target project;
- harness capability inventory based on `../ai-db`, with each capability marked
  `use`, `defer`, or `reject`;
- routing entries, rubrics, eval plans, release notes, and run-record templates.
- Agent Tester post-creation review before status promotion.

The Crew Builder is intentionally separate from `.agents/skills/agentic-crew-author`.
That skill is the authoring workflow. This folder is the deployable specialist
that owns role-boundary decisions, duplicate prevention, package completeness,
and handoff evidence.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy and ownership boundaries.
- `workflow.md`: creation/review workflow, `ai-db` capability inventory, and
  validation commands, plus Agent Tester post-creation review.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and protocol-review checklist.
- `eval-plan.md`: seed eval families and promotion thresholds.
- `run-record.template.json`: structured record for non-trivial runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

Related wrappers:

- `../../.agents/skills/agent-architect-crew-builder/SKILL.md`
- `../../.hermes/skills/agent-architect-crew-builder.md`
- `../../.hermes/agents/agent-architect-crew-builder/`
