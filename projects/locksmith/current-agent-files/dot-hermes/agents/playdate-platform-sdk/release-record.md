# Playdate Platform / SDK Release Record

## 2026-07-02 - v0.2.0 Draft Harness

Decision: draft-ready, not production/governed.

Changes:

- Added Hermes v1.1-style package around `playdate-platform-sdk`.
- Added Agent Card, source map, workflow, tool policy, rubric, eval plan,
  run-record template, release/rollback notes, health snapshot.
- Kept Codex wrapper as a thin entrypoint that points to Hermes canonical
  harness.

Evidence:

- Local `pdc --version` previously verified as `3.0.6`.
- Lua runtime absent in PATH during earlier probe.
- Official Playdate docs/changelog checked on 2026-07-02.

Known gaps:

- No pilot run records.
- No executable validators.
- No eval datasets.
- No production release gate.
