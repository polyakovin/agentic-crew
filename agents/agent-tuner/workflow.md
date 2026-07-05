# Agent Tuner Workflow

## Boot Probe

Run only when the task is non-trivial or publication may follow. Skip commands
that would broaden context beyond the named target surfaces.

```bash
git status --short
find agents -maxdepth 2 -name agent-card.json -o -name harness.yaml
find .agents .hermes -maxdepth 4 -type f
sed -n '1,180p' packs/software-development-crew.yaml
```

Interpretation:

- Dirty worktrees require scoped edits and explicit commit blockers.
- Pack routing should point to existing harness folders.
- Missing wrapper surfaces are tuning findings only if the target runtime claims
  those surfaces.

## Minimal Context Intake Ladder

Use the harness as the execution spine. Do not start by scanning the repository
or reading every wrapper. Escalate context only when a phase needs it:

1. `R0_boot`: read the task brief plus `harness.yaml`, `source-map.md`,
   `workflow.md`, and `tool-policy.md`; classify target, tuning mode, write
   scope, stop conditions, and validation budget.
2. `R1_contract`: read `protocol/interaction-protocol.md` only when the caller
   did not supply the report shape; read `release-rollback.md` when reporting
   rollback, blockers, or promotion status.
3. `R2_target`: read only named target role cards, Agent Cards, harnesses,
   wrappers, selected pack routes, prior findings, run records, and
   source-of-truth constraints.
4. `R3_overlap`: read neighboring role cards or harness summaries only when
   overlap, duplicate ownership, or handoff conflict is in scope.
5. `R4_enrichment`: read `eval-plan.md`, `run-record.template.json`, or named
   TODO/backlog artifacts only when eval seeds, durable records, wrapper drift,
   or TODO hygiene are changed.

Record the highest phase reached and any skipped phase in the run record or
final report. If a deliverable cannot be completed without a skipped source,
escalate to the next phase rather than guessing.

## Core Workflow

### 1. Intake And Mode Check

1. Identify the target agent id, current status, runtime surfaces, and source
   of truth.
2. Confirm this is tuning of an existing or draft described agent.
3. Classify mode:
   - `patch-plan-only`;
   - `bounded-edits`;
   - `handoff-required`.
4. Record write scope, forbidden files, expected output, validation budget, and
   promotion status limits.
5. Select the minimal intake ladder phase needed for the requested deliverable
   and name any task-supplied files that replace broader default reads.
6. Read `eval-plan.md` before reporting eval seeds or acceptance gates, read
   `release-rollback.md` before reporting rollback or promotion status, and
   read `run-record.template.json` before producing a durable run record.

Stop and hand off when:

- no target agent or draft description exists;
- the work is net-new agent creation or wrapper packaging;
- independent testing or red-team review is the actual request;
- protocol or pack governance must change.

### 2. Overlap And Ownership Review

Compare the target agent with neighboring specialists:

- mission;
- trigger/use-when;
- required reads;
- allowed tools and side effects;
- expected output payload;
- quality gates;
- handoff destination.

Default decisions:

- If three or more fields duplicate an existing specialist, propose reuse,
  merge, or route rather than tune a duplicate role.
- If a neighboring role owns the work, produce a handoff packet.
- If the overlap is narrow, propose or apply the smallest boundary patch.

Record SOLID ownership:

- which prompts, tools, evals, wrappers, routes, or checklists are
  specialist-owned;
- which artifacts intentionally remain shared interfaces;
- which source-of-truth docs must stay in the target project.

### 3. Tuning Patch Plan Or Bounded Edit

For each proposed change, capture:

- failure mode reduced;
- source evidence;
- target file or field;
- exact patch or recommended edit;
- risk if omitted;
- rollback step;
- validation command.

Preferred patch surfaces:

- mission and use-when text;
- non-goals and forbidden actions;
- source map and trust boundaries;
- workflow gates and blocker rules;
- Agent Card skill descriptions and examples;
- harness accepted/emitted payloads and release blockers;
- Codex/Hermes wrapper alignment;
- eval seeds and run-record fields;
- pack routing keys when discoverability is too broad or missing.

Avoid broad restyling, unrelated refactors, and changes that only alter tone.

### 4. TODO Hygiene

When tuning work completes a tracked TODO or backlog item:

- remove the completed task entry from the TODO/backlog artifact instead of
  leaving a checked-off item;
- preserve unresolved tasks, unrelated tasks, and historical context that is
  still needed to understand open work;
- record removed task titles, evidence, and validation in the run record or
  final report;
- do not delete a TODO/backlog file unless all entries are complete and the
  task explicitly allows file deletion.

### 5. Eval Seeds And Acceptance Gates

Every material tuning change should add or update at least one eval seed or
acceptance gate, unless explicitly deferred with rationale.

Seed families:

- duplicate-role collision;
- false production readiness;
- prompt injection in target agent docs or run logs;
- missing source-of-truth citation;
- overbroad tool permission;
- broken handoff to Builder, Tester, Steward, or Implementation;
- wrapper drift between Agentic Crew, Codex, and Hermes surfaces;
- scoped publication that keeps commit/push separate from promotion review;
- completed TODO entries left behind as stale checked-off work;
- broad context intake that reads unrelated agents, wrappers, packs, or repo
  history before the harness phases require them.

### 6. Agent Tester Review Gate

After material tuning edits, request Agent Tester review with:

- target agent id and tuning mode;
- changed files and patch summary;
- overlap analysis;
- eval seeds and acceptance gates;
- validation results;
- known dirty-worktree exclusions;
- promotion status requested.

If no callable Agent Tester runtime is available, record a blocker and keep the
package below promotion-ready. Do not substitute self-review for independent
Agent Tester review.

### 7. Publication Gate

When scoped edits were made and publication is authorized:

1. Re-run `git status --short --branch`.
2. Stage only the files changed by the tuning run.
3. Verify staged paths are within the authorized write scope.
4. Confirm the current branch and remote are the expected publication target.
5. Commit with a message that names the tuned agent and failure mode reduced.
6. Push after commit succeeds.

Do not stage, commit, or push when unrelated dirty changes cannot be excluded,
validation fails, or the branch/remote is unexpected. Missing Agent Tester review
blocks promotion status, not publication of validated scoped edits.

### 8. Final Report

Return an Agentic Crew `specialistReport` containing:

- `tunedAgentId`;
- `runtimeSurfaces`;
- `tuningMode`;
- source-of-truth evidence;
- overlap and ownership analysis;
- changes made or patch plan;
- eval seeds and routing recommendations;
- TODO/backlog entries removed, preserved, or newly added;
- validation results;
- Agent Tester result or blocker;
- rollback plan;
- promotion status;
- next owner.

## Validation Pack

Run from the Agentic Crew repository root after tuning this package or another
agent package:

```bash
python3 -m json.tool agents/agent-tuner/agent-card.json
python3 -m json.tool agents/agent-tuner/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-tuner/harness.yaml"); YAML.load_file(".hermes/agents/agent-tuner/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); puts "yaml ok"'
git diff --check
```

For target-agent tuning, add the target agent's changed JSON/YAML/TOML files to
the validation list.
