# Agent Architect / Crew Builder Workflow

## Boot Probe

Run for non-trivial agent creation or review:

```bash
git status --short
find agents -maxdepth 2 -name agent-card.json -o -name harness.yaml
find .agents .hermes -maxdepth 4 -type f
sed -n '1,160p' packs/software-development-crew.yaml
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Missing `.hermes` means no reusable Hermes pattern exists yet; create one only
  if requested or required.
- Pack routing should point to existing harness folders.

## Core Workflow

1. Record Orchestrator routing: local fallback or live delegation.
2. Read the target demand plan and target project rules.
3. Classify runtime surfaces: A2A, Codex, Hermes, or hybrid.
4. Decide creation scope: target project, Agentic Crew, or hybrid.
5. Run duplicate role review and harness reuse analysis.
6. Run Harness Capability Inventory from `../ai-db`.
7. Run the Context Economy Design Check.
8. Run SOLID ownership extraction audit.
9. Decide create/update/reuse/wrap/reject.
10. Draft role boundary and source hierarchy.
11. Create or adapt required harness and wrapper files.
12. Move or reference specialist-owned agent artifacts.
13. Wire pack routing when discoverability is required.
14. Validate machine-readable files and whitespace.
15. Request Agent Tester review for created or materially updated specialists.
16. Address the Agent Tester decision: block on critical findings, record
    backlog for non-critical findings, or proceed when the tester reports no
    release blockers.
17. After Agent Tester returns, mirror every open finding, recommendation, and
    improvement-backlog item into the task-specified TODO or backlog artifact.
    If no path is provided, use the existing project-local `TODO.md` or create
    one at the target project root when writes are allowed; otherwise record
    the exact entries that could not be written.
18. Hand the mirrored follow-up tasks to `agent-tuner` as `handoffPacket`
    entries with evidence, severity, affected surfaces, TODO artifact refs,
    remediation intent, and verification needed.
19. Commit and push scoped changes, or record why commit/push is blocked.
20. Produce run record, promotion status, and next-owner handoff.

## Creation Scope Decision

Record one of:

- `agentic-crew`: reusable harness/package lives in Agentic Crew; target project
  keeps only status links, demand, and source-of-truth docs.
- `target-project`: local agent package lives in the target project because the
  target explicitly needs local runtime integration or private unsanitized paths.
- `hybrid`: reusable harness lives in Agentic Crew and the target project keeps
  a thin local wrapper, adapter, or status link.

Decision questions:

- Is the role reusable across projects?
- Does the target project forbid or require local `.agents`, `.codex`, or
  `.hermes`?
- Are there private paths or workflows that must not become reusable templates?
- Can a project-local wrapper depend on a stable Agentic Crew harness instead
  of forking it?

Do not write files until the scope and rationale are recorded.

## Duplicate Role And Harness Reuse Review

```bash
rg -n "Mission|Use When|agent-authoring|harness|Hermes|Codex|routing" agents .agents .hermes packs
find agents -maxdepth 2 -name role.md -o -name harness.yaml -o -name agent-card.json
```

Reject or merge when:

- an existing agent has the same trigger and expected output;
- the new role only changes tone or title;
- the new role would need the same `What To Read` and same tools;
- the request is better served by a skill or wrapper.

Reuse/adapt when:

- an existing harness has compatible mission and gates;
- only runtime wrapper or project-local source map needs changing;
- the request is a narrower specialization of an existing generic harness.

Record:

- candidates checked;
- best reusable base;
- reuse, wrap, adapt, or new-package decision;
- files intentionally copied or referenced.

## Harness Capability Inventory

Read the canonical `ai-db` references before designing the package:

```bash
sed -n '1,220p' ../ai-db/patterns/architecture-design/agent-harness.md
sed -n '1,220p' ../ai-db/patterns/architecture-design/agent-system-components.md
sed -n '1,220p' ../ai-db/patterns/fundamentals/tool-use-and-mcp.md
sed -n '1,220p' ../ai-db/patterns/fundamentals/context-engineering.md
sed -n '1,220p' ../ai-db/patterns/architecture-design/agent-security.md
sed -n '1,220p' ../ai-db/patterns/implementation/agent-evaluations.md
sed -n '1,220p' ../ai-db/patterns/implementation/multi-agent-orchestration.md
sed -n '1,220p' ../ai-db/patterns/production-operations/production-operations.md
sed -n '1,180p' ../ai-db/tools/agent-tools/OVERVIEW.md
sed -n '1,180p' ../ai-db/tools/observability/OVERVIEW.md
sed -n '1,180p' ../ai-db/tools/agent-model-map.md
```

If the local `ai-db` path differs, locate it first:

```bash
find .. -maxdepth 3 -type d -name ai-db
```

For every new or updated specialist, review these categories and record
`use`, `defer`, or `reject`:

- rules/system prompt;
- AGENTS/project rules;
- role card and Agent Card;
- skills;
- commands;
- tools/function calling/MCP;
- file-system workspace;
- memory tiers;
- context packing/retrieval;
- subagents/orchestration;
- routing/handoff/shared state;
- hooks;
- sandbox/security/trust boundary;
- permissions/approvals/dry-run/retry/rollback;
- observability/audit;
- evals/replay/adversarial tests;
- model policy/model routing;
- runtime limits/budgets/concurrency;
- deployment topology;
- incident response/kill switch;
- integrations/browser/API/code/file tools;
- human handoff/escalation.

Each `use` decision must point to an artifact or field. Each `defer` or
`reject` decision must name why the capability would not reduce risk now.

## Context Economy Design Check

Run this check before writing or adapting role prompts, source maps, wrappers,
or run-record templates.

1. Build the smallest evidence set that can define the agent's role boundary,
   source hierarchy, quality gates, handoff, and validation commands.
2. Split context into `stableContext`, `taskBrief.whatToRead`,
   `progressiveReads`, and `excludedContext` using `source-map.md#context-economy-contract`.
3. Bound every target-project source reference by path plus section, line range,
   specific glob, generated artifact, or search query. Reject required reads
   such as "the repo", "all docs", "all source", or broad private snapshots
   unless the user explicitly approves the scope and the run record names the
   reason, budget, and fallback.
4. Convert broad exploration into progressive workflow steps. Start with the
   demand plan, project rules, selected pack, closest existing harness, and
   named source-of-truth docs; only expand to additional files after an
   observation shows why they are needed.
5. Keep runtime wrappers thin. They may summarize the mission and trigger, but
   must point to the canonical harness files instead of copying long role,
   workflow, rubric, eval, or source-map content.
6. Put task-completion checks in harness-owned artifacts: quality gates,
   `harness.yaml`, rubric, eval seeds, run-record fields, release blockers, and
   the final `specialistReport` contract.
7. Record the context decision in the run record or final report, including
   required reads kept, broad reads rejected or deferred, progressive reads, and
   any prompt/wrapper duplication intentionally allowed.

Block or keep the package below `draft-ready` when the read set cannot be
bounded, when prompt-only completion checks are not mirrored into harness
artifacts, or when a wrapper duplicates harness content without a canonical path
reference.

## SOLID Ownership Extraction Audit

Before and after creating a specialist, identify whether target-project agent
обвязка should remain shared or move into the specialist package.

Look for:

```bash
find . -maxdepth 4 -path './.git' -prune -o -type f \( -path './.agents/*' -o -path './.codex/*' -o -path './.hermes/*' -o -path './agents/*' -o -path './packs/*' -o -path './plans/*' \) -print
rg -n "agent|specialist|harness|Hermes|Codex|rubric|eval|run-record|routing|tool policy|checklist" AGENTS.md README.md TODO.md docs plans .agents .codex .hermes agents packs
```

Classify each artifact:

- `shared-interface`: keep in shared project space because multiple specialists
  depend on it.
- `specialist-owned`: move into `agents/<id>/`, `.agents/skills/<id>/`, or
  `.hermes/agents/<id>/`.
- `target-source-of-truth`: keep in the target project and reference it from the
  specialist source map.
- `delete-after-move`: remove only after explicit approval and provenance is
  recorded.

Do not move product/game source, source-of-truth specs, assets, or tests by
default. Those remain target-project owned.

The run record must list:

- artifacts moved;
- artifacts intentionally left shared;
- target-project links/status notes updated;
- approval for any deletion or destructive move.

## Package Completeness Check

Required for this high-risk role:

- `README.md`
- `entrypoint.md`
- `role.md`
- `agent-card.json`
- `harness.yaml`
- `source-map.md`
- `workflow.md`
- `tool-policy.md`
- `rubric.md`
- `eval-plan.md`
- `run-record.template.json`
- `release-rollback.md`
- `health-snapshot.md`
- Codex wrapper when required;
- Hermes wrapper/package when required.
- Ownership extraction record for specialist-only agent artifacts.

## Wrapper Alignment Check

Compare these fields across A2A, Codex, and Hermes surfaces:

- agent id;
- mission;
- use cases;
- required sources;
- forbidden actions;
- quality gates;
- output shape.

If they disagree, the package is still draft and must not update target project
status.

## Agent Tester Post-Creation Review

After creating or materially updating a specialist package, Crew Builder must
ask `agent-tester` to review the new agent before marking it created,
promotion-ready, or target-project complete.

Send Agent Tester:

- tested agent id and creation scope;
- changed harness, Agent Card, wrapper, pack, rubric, eval, run-record,
  release/rollback, and knowledge-base paths;
- validation results;
- capability inventory;
- reuse/duplicate analysis;
- ownership extraction summary;
- target demand plan and acceptance criteria refs;
- known blockers, dirty-worktree context, and omitted runtime surfaces.

Expected Agent Tester output:

- `specialistReport`;
- `reviewFinding` entries;
- prioritized `improvementBacklog`;
- project TODO/backlog updates or exact blocked entries;
- critical `handoffPacket` entries to the owning remediation specialist:
  `agent-tuner` for existing-agent prompt, role, workflow, gate, eval, wrapper,
  or routing refinement; `agent-architect-crew-builder` for creation/package
  defects it owns; and `protocol-steward` for shared protocol or pack-governance
  defects;
- knowledge-base lesson updates or proposals;
- residual risk and promotion recommendation.

Crew Builder must:

- block target-project status updates when Agent Tester reports a critical
  finding;
- include Agent Tester findings/backlog in the run record;
- mirror every Agent Tester finding, recommendation, and improvement-backlog
  item that requires follow-up into the task-specified TODO or backlog artifact.
  Prefer Agent Tester's `projectTodoUpdates`; otherwise write the entries
  directly when authorized or record a write-access blocker with the exact
  entries to add;
- route critical repair requests by ownership: existing-agent refinement to
  `agent-tuner`; Crew Builder-owned creation, packaging, wrapper, or routing
  defects back to `agent-architect-crew-builder`; and shared protocol or pack
  governance to `protocol-steward`;
- route all mirrored existing-agent tuning/refinement tasks to `agent-tuner`
  with evidence, severity, affected surfaces, remediation intent, TODO artifact
  refs, and verification needed;
- block promotion when required TODO/backlog updates or `agent-tuner` handoff
  packets cannot be recorded;
- not hide or silently fix Agent Tester findings unless explicitly reassigned;
- treat missing Agent Tester review as a release blocker unless the tester is
  unavailable, in which case record a blocker and keep the package below
  promotion-ready.

## Validation Pack

Run from the Agentic Crew repository root:

```bash
python3 -m json.tool agents/agent-architect-crew-builder/agent-card.json
python3 -m json.tool agents/agent-architect-crew-builder/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-architect-crew-builder/harness.yaml"); YAML.load_file(".hermes/agents/agent-architect-crew-builder/manifest.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/agent-architect-crew-builder .agents/skills/agent-architect-crew-builder .hermes/skills/agent-architect-crew-builder.md .hermes/agents/agent-architect-crew-builder packs
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state in each affected repository.
2. Stage only scoped Crew Builder changes.
3. Commit with an atomic message that names the agent/package.
4. Push the current branch.
5. Record commit SHA, branch, remote, and push result in the run record.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- deletion/move approval is missing;
- credentials or branch policy prevent push;
- the target project source-of-truth status has not been updated when required.

If push fails because credentials are unavailable, keep the commit local only if
the user explicitly asked for a local commit; otherwise record a blocker before
committing.

## Status Rules

- `draft`: required files are incomplete or validation has not run.
- `draft-ready`: required files exist and smoke validation passes.
- `pilot`: at least one target project used the package and recorded a run.
- `production`: pilot evidence, eval evidence, and independent review exist.

Never promote directly from `draft` to `production`.
