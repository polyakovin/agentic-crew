# Agent Tuner Workflow

## Boot Probe

Run for non-trivial tuning tasks:

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
5. Read `eval-plan.md` before reporting eval seeds or acceptance gates, read
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
- completed TODO entries left behind as stale checked-off work.

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

### 7. Final Report

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
