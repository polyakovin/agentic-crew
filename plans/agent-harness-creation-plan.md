# Agent Harness Creation Plan

This repository owns generic agent harness creation. Target projects own their
own demand lists, local source maps, product constraints, and project-specific
routing needs.

## Boundary

Target projects should keep:

- the list of agents they need;
- project-specific source files and specs each agent should read;
- local verification commands;
- product names, domain details, and risk history.

Agentic Crew should keep:

- reusable harness folders in `agents/<id>/`;
- A2A Agent Cards;
- `harness.yaml` conventions;
- Codex skill and custom-subagent patterns;
- Hermes-style package patterns;
- run-record templates, rubrics, eval plans, and release/rollback notes;
- generic crew packs and routing examples.

Do not store target-project snapshots or project-specific agent plans in this
repository. Project-specific examples must be sanitized.

## Intake Workflow

When a target project asks to create agents:

1. Read the target project's agent demand plan.
2. Classify each requested agent by runtime: Agentic Crew/A2A, Codex skill,
   Codex custom subagent, Hermes-style package, or hybrid.
3. Decide whether the target needs a reusable generic harness, a project-local
   wrapper, or both.
4. Create generic reusable files here only when they are project-neutral.
5. Tell the target project which local files should reference the new harness.
6. Keep status `draft` or `draft-ready` until pilot runs, eval evidence, and
   independent review exist.

## Harness Requirements

Every Agentic Crew specialist must include:

- `README.md`
- `role.md`
- `agent-card.json`
- `harness.yaml`
- `operations.md` or split operational docs
- `run-record.template.json`

High-risk or platform-heavy specialists should split operational docs into:

- `entrypoint.md`
- `source-map.md`
- `workflow.md`
- `tool-policy.md`
- `rubric.md`
- `eval-plan.md`
- `release-rollback.md`
- `health-snapshot.md`

Codex and Hermes wrappers may be documented here as reusable patterns, but
project-specific wrapper files should live in the target project only if the
target project wants local runtime integration.

## Generic Backlog

P0:

- Strengthen `.agents/skills/agentic-crew-author/SKILL.md` as the canonical
  authoring workflow for all new agents.
- Add reusable templates for standard and high-risk harness folders.
- Add a harness review checklist that can be run before publishing a new agent.
- Add schema validation notes for Agent Cards, run records, and `harness.yaml`.

P1:

- Add optional generic specialists for pixel UI, input feel, build gatekeeping,
  spec-contract review, economy/progression, asset pipeline, and copy review.
- Add pack examples that compose those roles without naming a target project.
- Add sanitized handoff examples for cross-specialist routing.

P2:

- Add eval seed cases for role overlap, missing source evidence, false
  production readiness, and incomplete handoff contracts.
- Add release promotion checklist for moving an agent from `draft-ready` to
  pilot or production.

## Done Criteria

A new generic agent is ready to use when:

- all required harness files exist;
- JSON/YAML/TOML files parse;
- `rg` finds no target-project names or private paths;
- `git diff --check` is clean;
- role boundaries, non-goals, blockers, and handoff contract are explicit;
- a target project can point to the harness without copying unrelated project
  history into this repository.
