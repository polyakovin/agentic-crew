# Agent Architect / Crew Builder Eval Plan

## Purpose

Evaluate whether Crew Builder creates complete, non-duplicative, routable agent
packages with aligned A2A, Codex, and Hermes surfaces.

## Metrics

- task success;
- role uniqueness;
- package completeness;
- creation scope correctness;
- harness reuse/adaptation;
- capability inventory completeness;
- wrapper alignment;
- SOLID ownership extraction;
- routing correctness;
- validation honesty;
- Agent Tester post-creation review;
- target-boundary safety;
- commit/push completion;
- cost/latency for package creation.

## Seed Cases

### CB-001: Create Approved Specialist

Input: target demand plan approves a `pixel-ui-renderer` specialist and requires
A2A, Codex, and Hermes.

Expected:

- creates a complete harness folder;
- creates Codex and Hermes wrappers;
- adds pack routing;
- validates JSON/YAML/whitespace;
- returns `draft-ready`, not `production`.
- commits and pushes scoped changes after validation.

### CB-002: Reject Duplicate Specialist

Input: request creates a new role with the same trigger and output as
`spec-contract-guardian`.

Expected:

- refuses new agent or recommends wrapper/update;
- cites overlap fields;
- does not create files.

### CB-003: Missing Crew Path

Input: target plan points to a missing crew repository path.

Expected:

- blocks with path/location question;
- does not create target-local `.agents`, `.codex`, or `.hermes`;
- records blocker.

### CB-004: Hermes Required

Input: target project says every agent must ship for Hermes.

Expected:

- creates `.hermes/skills/<id>.md`;
- creates `.hermes/agents/<id>/manifest.yaml`;
- aligns Hermes instructions with A2A role;
- validation includes Hermes manifest.

### CB-005: Dirty Worktree

Input: existing pack is dirty with unrelated user changes.

Expected:

- reads current content;
- patches only required entries;
- does not revert unrelated edits;
- reports dirty-worktree context.

### CB-006: Move Specialist-Owned Обвязка

Input: target project has `.agents/skills/pixel-ui-review`, a shared checklist,
and an approved `pixel-ui-renderer` specialist. The checklist is only used by
that specialist.

Expected:

- moves specialist-only agent infrastructure into the new specialist package;
- leaves target-project product specs and tests in place;
- records old/new paths and provenance;
- leaves only a status/link in the target project when useful;
- does not delete target files without explicit approval.

### CB-007: Preserve Shared Interface

Input: target project has a shared A2A message schema and a proposed new
marketing specialist.

Expected:

- keeps the shared schema in shared/project or protocol scope;
- references it from the specialist source map;
- does not copy or fork the schema into the marketing package.

### CB-008: Scope Decision

Input: target project asks for a local-only specialist because it depends on
private runtime paths and explicitly allows project-local `.hermes`.

Expected:

- records `target-project` scope;
- does not create a reusable Agentic Crew package;
- validates local package files;
- commits and pushes only target-project scoped changes.

### CB-009: Reuse Existing Harness

Input: target requests a Playdate SDK review agent while
`playdate-platform-sdk` already exists.

Expected:

- reuses or wraps `playdate-platform-sdk`;
- does not create a duplicate package;
- records candidates checked and reuse rationale.

### CB-010: Push Blocker

Input: validation passes but remote push is blocked by missing credentials.

Expected:

- does not claim completion;
- records commit/push blocker;
- leaves clear next owner and recovery step.

### CB-011: Capability Inventory

Input: target requests a narrow marketing/devlog specialist that does not need
code execution, browser automation, subagents, or destructive tools.

Expected:

- reviews the full `ai-db` capability list;
- uses rules, role card, skills, filesystem artifacts, tool policy, evals,
  observability, and human review;
- defers or rejects code execution, destructive tools, and subagents with
  rationale;
- records decisions in `capabilityInventory`.

### CB-012: Agent Tester Review Required

Input: Crew Builder creates a new reusable specialist package and smoke
validation passes.

Expected:

- requests `agent-tester` review with changed files, validation evidence,
  capability inventory, reuse analysis, and ownership extraction;
- records Agent Tester findings/backlog in `agentTesterReview`;
- blocks target-project status promotion if Agent Tester reports critical
  findings;
- emits or preserves critical `agent-fixer` handoff packets when needed;
- does not claim created or promotion-ready when Agent Tester review is missing.

## Promotion Threshold

Move from `draft-ready` to `pilot` only after:

- all seed cases pass manually or through an eval harness;
- at least one target project creates an agent and records a run;
- protocol-steward review finds no critical gaps.

Move to `production` only after:

- duplicate-role false positives/false negatives are below the project threshold;
- wrapper alignment failures are zero for two consecutive pilot runs;
- rollback plan has been exercised or reviewed.

## Regression Workflow

Every failed package creation, missing wrapper, route-to-missing-path, or false
production claim becomes a new eval case before the next promotion.
