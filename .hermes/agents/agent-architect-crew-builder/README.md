# Hermes Package: Agent Architect / Crew Builder

Portable Hermes-style package for the Agent Architect / Crew Builder specialist.

This package adapts the Agentic Crew harness in
`agents/agent-architect-crew-builder/` to Hermes usage. It does not introduce a
new protocol; Hermes-facing tasks should still emit Agentic Crew-compatible
summary payloads at the boundary.

Files:

- `manifest.yaml`: package metadata and entrypoints.
- `instructions.md`: Hermes-facing operating instructions.

Status: draft-ready pending validation against the concrete Hermes runtime.
