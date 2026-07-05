# Agent Tester

High-risk specialist harness for testing other specialist agents through
evidence-backed static review, scenario execution, exploratory testing,
adversarial checks, and learning-loop updates.

Status: draft-ready portable harness.

This agent improves agent quality by testing the agent as a workflow, not only
as a final answer. It checks role boundaries, source hierarchy, tool behavior,
trace evidence, safety controls, eval coverage, handoff contracts, and repeated
failure patterns.

Core outputs:

- an agent test charter;
- exploratory and regression findings;
- an improvement backlog ordered by severity;
- critical fix requests handed to the future `agent-fixer` specialist;
- knowledge-base updates that turn new testing experience into reusable
  agent-authoring guidance.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: narrow role card for A2A routing.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and wrapper paths.
- `source-map.md`: source hierarchy, external best-practice policy, and
  knowledge-base ownership.
- `workflow.md`: agent testing methodology and validation workflow.
- `tool-policy.md`: permissions, internet research rules, and forbidden actions.
- `rubric.md`: self-review and challenge checklist.
- `eval-plan.md`: seed eval families and promotion thresholds.
- `run-record.template.json`: structured record for non-trivial test runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health, creation decisions, and gaps.
- `knowledge-base/`: lessons learned, research notes, and learning-event
  templates owned by this specialist.

Related wrappers:

- `../../.agents/skills/agent-tester/SKILL.md`
- `../../.hermes/skills/agent-tester.md`
- `../../.hermes/agents/agent-tester/`
