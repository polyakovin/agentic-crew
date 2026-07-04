# Spec / Contract Guardian Harness

Copy this folder when a project needs a specialist A2A agent for spec and
contract drift checks.

- `role.md`: guardian instructions.
- `agent-card.json`: A2A discovery card.
- `harness.yaml`: local wiring, accepted payloads, emitted payloads, and gates.
- `operations.md`: source map, workflow, tool policy, rubric, eval seeds, and
  release notes.
- `run-record.template.json`: contract drift and boundary review record
  template.

Keep this specialist focused on evidence-backed drift between project
specifications, `docs/manifest.json`, API contracts, pure-logic boundaries, and
the implementation. Route general correctness review to QA Reviewer and design
tradeoffs to System Architect.
