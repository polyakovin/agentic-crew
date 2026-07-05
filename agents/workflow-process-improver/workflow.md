# Workflow Process Improver Workflow

## Boot Probe

Run for non-trivial workflow process improvement tasks:

```bash
git status --short
rg -n "multica|Multica|workflow|session|log|run-record|handoff|TODO|backlog" AGENTS.md README.md TODO.md plans agents packs .agents .hermes
find agents -maxdepth 2 -name agent-card.json -o -name harness.yaml
find .agents .hermes -maxdepth 4 -type f
```

Interpretation:

- Dirty worktrees require a write-boundary ledger before edits.
- A Multica mention is not a CLI destination unless there is a concrete command,
  config, backlog artifact, or task-provided contract.
- Missing Multica destination requires fallback TODO/backlog approval or a
  blocker with exact proposal entries.

## Core Workflow

1. Record execution mode: local skill use, live delegated agent run, or A2A
   runtime run.
2. Record target project, workflow scope, allowed evidence sources, write
   boundary, forbidden paths, and proposal destination.
3. Discover Multica destination.
4. Build workflow evidence inventory.
5. Reconstruct workflow timeline.
6. Analyze process bottlenecks and information-capture gaps.
7. Author improvement proposals with evidence, owner, severity, and
   verification.
8. Write proposals to Multica CLI or approved fallback.
9. Apply authorized information-capture improvements when scoped and safe.
10. Validate changed artifacts.
11. Produce `specialistReport`, run record, and handoff packets.

## 1. Intake And Write Boundary

Capture:

- target project path or identifier;
- task id and workflow time window;
- project rules and user constraints;
- authorized read paths and source types;
- authorized write paths;
- forbidden paths;
- whether external side effects are allowed;
- proposal destination priority;
- expected output format.

For no-target-edit or TODO-only runs, record pre-run and post-run
`git status --short`, exact changed files, and why each changed file is within
the allowed write boundary.

## 2. Multica Destination Discovery

Look for a concrete destination in this order:

1. Task brief fields such as `multicaCommand`, `multicaProject`,
   `multicaIssue`, `proposalDestination`, or explicit CLI instructions.
2. Installed CLI evidence when allowed, such as `which multica`,
   `multica --help`, `multica issue create --help`, `multica project list`,
   or `multica issue list --output json`.
3. Target project files that clearly define Multica CLI usage, such as
   `multica.yaml`, `.multica/*`, package scripts, docs, or backlog artifacts.
4. Existing TODO/backlog artifacts explicitly named as fallback.

Use narrow searches such as:

```bash
rg -n "multica|Multica|proposalDestination|project board|backlog" .
```

Do not infer command syntax from generic prose. If a command is present or
discovered from CLI help/output, record:

- command family and version when available;
- destination project, board, issue, or queue;
- dry-run/preview availability;
- approval state for external side effects;
- output or error summary.

If no concrete Multica destination exists, continue only when a fallback
TODO/backlog path is authorized. Otherwise, return blocked with the exact
proposal entries that need a destination.

Read-only CLI success is discovery evidence, not approval to create or update
Multica records. Before any proposal write, the run record must name the
`approvalState` and `approvalSource`. If the CLI has no dry-run flag, dry-run
means preview only: record the destination, title, status, assignee state,
description preview or hash, and sanitized command shape without executing the
write.

## 3. Workflow Evidence Inventory

Collect only bounded evidence required for the workflow scope:

- project rules;
- task briefs and acceptance criteria;
- run records and session summaries;
- logs and traces;
- review findings;
- handoff packets and decision records;
- TODO/backlog entries;
- project-board or issue exports;
- bounded git history for workflow-owned files;
- user or maintainer corrections.

For each evidence item, record path or command, time window, source type, trust
label, privacy classification, redaction status, and supported claim.

Stop and ask for narrowing when the task requires all logs, all sessions, or an
entire repository history without a reason and budget.

## 4. Workflow Timeline Reconstruction

Build a timeline that separates:

- intake and acceptance criteria;
- planning and task decomposition;
- delegation and owner changes;
- source reads and context updates;
- tool calls and approvals;
- blockers and retries;
- handoffs and review loops;
- TODO/backlog updates;
- completion, non-completion, or blocked state.

Mark each event with evidence id and confidence. Distinguish facts from
inferences.

## 5. Process And Information-Capture Analysis

Classify workflow findings:

- missing or late source-of-truth read;
- unclear owner or next specialist;
- stale status or missing progress update;
- repeated blocker without escalation;
- duplicated work or unbounded context search;
- missing validation or review gate;
- proposal not written to the expected task system;
- missing run-record, handoff, log, trace, or TODO field;
- privacy or redaction weakness;
- unsafe external side effect or approval gap.

For each information-capture gap, name:

- missing field, event, or artifact;
- affected workflow step;
- evidence that the gap caused uncertainty or rework;
- owner;
- recommended artifact change;
- verification needed.

## 6. Proposal Authoring And Destination Write

Each proposal should include:

- stable id;
- summary;
- severity;
- owner;
- workflow step affected;
- evidence ids;
- recommended change;
- expected benefit;
- risk if omitted;
- destination status;
- verification needed.

Write order:

1. Multica CLI destination when concrete and approved.
2. Task-specified TODO/backlog fallback.
3. Existing project-local TODO/backlog fallback when project rules permit.
4. Blocked entries in final report when no write path is allowed.

Do not leave proposals only in the chat response when a write destination is
available.

For every Multica proposal write, preserve enough evidence to distinguish
discovery from approved write authority:

- approval state and approval source;
- destination `project_id` or `issue_id`;
- exact create or update command shape with secrets redacted;
- Multica JSON response `id`, `identifier`, `status`, and `project_id`;
- duplicate-check result and retry count;
- rollback command or restoration path.

Retry a failed external write at most once after fixing command or input syntax.
If success is ambiguous after a timeout or transport error, search the
destination by exact title before retrying so the run does not create
duplicates.

## 7. Authorized Information-Capture Improvements

Apply direct edits only when all are true:

- the task grants write scope;
- the edit changes workflow capture docs, templates, TODO/backlog schema,
  run-record templates, or handoff fields;
- the edit is project-local or within the assigned harness package;
- no product source, telemetry implementation, CI, protocol schema, or
  unrelated agent package changes are required.

Examples of allowed scoped improvements:

- add a missing field to a run-record template;
- add owner/evidence/verification fields to a TODO proposal template;
- add a session-summary checklist;
- add a workflow retrospective checklist;
- add a handoff field recommendation.

Examples that require handoff:

- implement telemetry collection in product code;
- install or configure Multica CLI;
- change a shared A2A schema;
- retune an existing agent workflow;
- add CI hooks or deployment automation.

## Validation Pack

Run from the Agentic Crew repository root for this package:

```bash
python3 -m json.tool agents/workflow-process-improver/agent-card.json
python3 -m json.tool agents/workflow-process-improver/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/workflow-process-improver/harness.yaml"); YAML.load_file(".hermes/agents/workflow-process-improver/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/workflow-process-improver .agents/skills/workflow-process-improver .hermes/skills/workflow-process-improver.md .hermes/agents/workflow-process-improver packs/software-development-crew.yaml TODO.md plans/workflow-process-improver-scope-decision.md
git diff --check
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Agent Tester Review Gate

After creating or materially updating this package, request Agent Tester review
before promotion. Send:

- changed harness paths;
- wrappers and routing paths;
- validation results;
- capability inventory;
- overlap analysis;
- ownership extraction summary;
- Multica discovery evidence;
- TODO/backlog fallback updates;
- dirty worktree context.

If Agent Tester is unavailable, record a blocker and keep status below
`draft-ready`.

## Handoff Routing

Route follow-up by ownership:

- `agent-tester`: independent review or behavioral testing of this specialist.
- `agent-teacher`: durable learning material for a named target agent.
- `agent-tuner`: prompt, role, workflow, eval, wrapper, or routing refinements
  to an existing agent.
- `protocol-steward`: A2A profile, message schema, pack merge semantics, or
  shared routing governance.
- `implementation-engineer`: product or integration implementation.
- Human/project owner: approval for external side effects, Multica setup, broad
  history reads, or process policy adoption.
