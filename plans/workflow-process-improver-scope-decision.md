# Workflow Process Improver Scope Decision

Task id: `workflow-process-improver-agent-2026-07-05`
Decision date: 2026-07-05
Decision owner: `agent-architect-crew-builder`

## Decision

Create a reusable Agentic Crew specialist package with id
`workflow-process-improver`.

- Scope: `agentic-crew`.
- Runtime surfaces: A2A harness, Codex skill wrapper, Hermes skill and package,
  and software-development crew routing.
- Status target for this creation pass: `draft-ready` after validation and
  independent Agent Tester review. Promotion to pilot remains blocked without
  scenario and pilot run evidence.

## Rationale

The requested role is reusable across projects: it audits how project workflow
tasks actually happened, using logs, sessions, run records, handoffs, TODOs,
review findings, and other available process evidence. It does not depend on
private target-project paths, product code, or a single domain.

The role is distinct from adjacent specialists:

- `agent-tester` tests specialist agents as workflows and writes findings.
- `agent-teacher` turns target-agent history into durable learning material for
  that target agent.
- `agent-tuner` edits existing agent definitions from findings or tuning
  goals.
- `workflow-process-improver` audits the project workflow process itself,
  proposes process improvements, and improves authorized information-capture
  surfaces when gaps in logs, sessions, run records, or task state make future
  workflow analysis weaker.

## Write Boundary

Authorized scoped paths for this task:

- `agents/workflow-process-improver/**`
- `.agents/skills/workflow-process-improver/**`
- `.hermes/skills/workflow-process-improver.md`
- `.hermes/agents/workflow-process-improver/**`
- `packs/software-development-crew.yaml`
- `TODO.md`
- `plans/workflow-process-improver-scope-decision.md`

Forbidden paths:

- product or application source code;
- unrelated existing specialist packages;
- existing dirty files under `agents/playdate-platform-sdk/**`;
- protocol schemas unless separately assigned to `protocol-steward`;
- destructive moves or deletions.

## Multica CLI Discovery

Repository-local search found no Multica config, backlog file, or command
contract in this repository. Host-level discovery found an installed
`multica` CLI at `/opt/homebrew/bin/multica`; `multica issue create --help`
defines the proposal-write command shape, and `multica project list --output
json` identified project `590c34c6-1c3c-49b3-b662-34ebd8cf34b4` (`Locksmith`).

Current follow-up proposals were written to Multica issues `POL-3` and `POL-4`
and mirrored in `TODO.md`. The new specialist must still require explicit
side-effect approval and command/destination evidence before future Multica
create or update operations.
