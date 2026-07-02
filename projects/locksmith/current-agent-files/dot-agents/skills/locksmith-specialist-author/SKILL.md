---
name: locksmith-specialist-author
description: Create or update instructions for Locksmith project specialists. Use when designing Codex skills, Codex custom subagents, Hermes skills, routing rules, or lightweight agent cards for Playdate-safe-cracker specialist roles such as UI/renderer, crank feel, Adventure state machine, Endless economy, build gatekeeping, spec contracts, assets, or narrative.
---

# Locksmith Specialist Author

Use this skill to write robust, lightweight instructions for project specialists in
`/Users/polyakovin/repos/playdate-safe-cracker`.

The output is not game code. The output is specialist operating instructions:
a Codex skill, Codex custom subagent, Hermes skill, routing table, or draft agent card.

## Source Order

Read only what is relevant, but start with these sources before drafting:

1. `TODO.md` -> `Agent Specialists Plan`.
2. `AGENTS.md` -> project rules, Playdate pitfalls, verification gates.
3. `docs/manifest.json` -> component contracts and dependency boundaries.
4. `docs/workflow.md` -> Spec -> Plan -> Implement -> Review.
5. Existing `.hermes/skills/debug.md`, `.hermes/skills/tdd.md`, `.hermes/skills/review.md`.
6. For role-specific work, read the matching docs/source files.

If the specialist is based on recurring failures, inspect git history with:

```bash
git log --format=%s -n 300
git log --format= --name-only | awk 'NF {count[$0]++} END {for (f in count) print count[f], f}' | sort -nr | head -40
```

## Format Selection

Choose the smallest durable surface that matches the need:

- `.agents/skills/<id>/SKILL.md` for reusable Codex workflows.
- `.codex/agents/<id>.toml` for spawnable Codex subagents with separate instructions/model/sandbox.
- `.hermes/skills/<id>.md` for the existing Hermes-style project harness.
- `AGENTS.md` only for repo-wide rules that every agent must always follow.
- `docs/` only for game/product/source-of-truth specs, not agent behavior.

Prefer a Codex skill when unsure. Add a custom subagent only when parallel review,
independent judgment, model choice, or sandbox separation matters.

Project convention:

- If the user says "завести новых агентов из TODO", "оформить специалистов из
  TODO", or points to `TODO.md -> Agent Specialists Plan`, do not treat the task
  as a minimal reusable Codex skill request.
- Default to a hybrid specialist harness: canonical Hermes entrypoint
  `.hermes/skills/<id>.md`, full package `.hermes/agents/<id>/`, and thin Codex
  wrapper `.codex/agents/<id>.toml` plus
  `.codex/agents/<id>.instructions.md`, unless the user explicitly excludes
  Hermes or Codex.
- Keep the package `draft`/`draft-ready`, not `production`, until pilot run
  records, eval/certification evidence, and independent review exist.
- If the user provides a Hermes standard/reference document, follow its package
  shape for Agent Card, source map, workflow, tool policy, rubric, eval plan,
  run-record template, release/rollback, and health/release records.

## Specialist Instruction Template

Every specialist instruction should include:

```text
Mission
Use When
Scope
Non-goals
What To Read
Source Hierarchy
Workflow
Minimum Deliverable
Quality Gates
Blockers
Handoff Contract
```

For high-risk specialists, also include:

```text
Tool Policy
Risk Gates
Verification Evidence
Known Project Pitfalls
Learning Loop / When To Update AGENTS.md
```

Keep each specialist narrow. Do not create a "can do everything" role.

## Required Role Calibration

Before writing the final specialist, answer these internally:

- What recurring failure does this specialist prevent?
- Which files/specs does it own?
- Which files/specs must it not own?
- What evidence proves the specialist did useful work?
- When should it block instead of improvising?
- Which existing generic Hermes skill does it reuse?

## Locksmith Seed Specialists

Use these as starting points, not rigid categories:

- `locksmith-orchestrator`: routes work, resolves conflicts, owns SDD handoff.
- `playdate-pixel-ui-renderer`: owns 400x240 1-bit readability, layout, screenshots.
- `crank-feel-micro-mechanics`: owns crank feel, tolerance, miss/reverse rules.
- `playdate-platform-sdk`: owns Playdate SDK/API quirks, crank/buttons/accelerometer, graphics draw modes, datastore, simulator/device differences, and performance best practices.
- `adventure-state-machine`: owns Adventure phase flow and state/spec consistency.
- `endless-economy`: owns progression, market timers, jobs, saves, crafting, payout.
- `toolchain-build-gatekeeper`: owns Lua/pdc/build/check/simulator/hook evidence.
- `spec-contract-guardian`: owns manifest/spec/source consistency and boundaries.
- `asset-storefront`: owns launcher art, item sheets, teaser/store assets.
- `narrative-noir-copy`: owns loreLine, item copy, tone, Playdate-fitting text.

## Project-Specific Guardrails

- Preserve Spec-Driven Development: spec before behavior/API changes.
- Keep `safe.lua` pure logic: no Playdate graphics/input API.
- Keep renderer/UI drawing separate from game state mutation.
- Respect Playdate 400x240, 1-bit, ASCII-safe UI constraints.
- Treat missing Lua runtime as a blocked Lua gate, not a unit-test failure.
- If `make check` is blocked by missing Lua, still run `make build` when possible.
- New Playdate API/runtime pitfalls must update `AGENTS.md`.
- Do not duplicate generic debug/TDD/review checks already in `.hermes/skills/*`; reference them.
- Keep each specialist's `What To Read` narrow and role-specific; otherwise the specialist becomes the same universal agent with a longer prompt.

## Output Rules

When asked to create a specialist:

1. State the target format and file path.
2. Draft the specialist instruction with the template above.
3. Include role-specific `What To Read` and `Quality Gates`.
4. Add a short routing note: when the orchestrator should call this specialist.
5. Keep the first version draft/pilot-sized; do not create a full production package unless requested.

When asked to "завести новых агентов из TODO":

1. Read `TODO.md -> Agent Specialists Plan` and identify requested specialists.
2. Create the full hybrid Hermes+Codex harness for each requested specialist.
3. Include or update routing/review checklists so orchestrator handoff is clear.
4. Validate machine-readable files such as JSON/TOML.
5. Report every created path and any blocked specialist; do not mark a TODO item
   done unless its files exist and validation passed.

When asked to review a specialist:

1. Check whether mission, scope, non-goals, blockers, and gates are testable.
2. Check for overlap with other specialists.
3. Check whether it has enough project-specific sources.
4. Remove generic filler and tighten trigger wording.
