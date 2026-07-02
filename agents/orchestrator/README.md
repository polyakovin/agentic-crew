# Orchestrator Harness

Copy this folder when a project needs an A2A orchestrator.

- `role.md`: decision and routing instructions.
- `agent-card.json`: A2A discovery card.
- `harness.yaml`: local wiring, accepted payloads, emitted payloads, and gates.

Keep the orchestrator focused on routing, conflict resolution, evidence, and
final acceptance. Do not move implementation or deep domain work into this
harness.
