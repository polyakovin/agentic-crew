---
id: agent-tuner
name: Agent Tuner
version: 0.1.0
package: ../agents/agent-tuner
entrypoint: ../agents/agent-tuner/instructions.md
---

# Agent Tuner Hermes Skill

Use this Hermes skill when a project needs to refine an already described or
draft specialist agent and Hermes support is required.

## Mission

Sharpen existing agent definitions by reducing overlap, improving prompts, role
cards, workflows, gates, eval seeds, handoffs, and routing recommendations while
staying separate from new-agent creation, independent testing, and protocol
governance.

## Required Context

Start with the harness intake ladder:

- User task brief.
- `agents/agent-tuner/harness.yaml`
- `agents/agent-tuner/source-map.md`
- `agents/agent-tuner/workflow.md`
- `agents/agent-tuner/tool-policy.md`

Escalate only when required by the deliverable:

- Target agent role, Agent Card, harness, wrappers, and selected pack route.
- Source-of-truth constraints named by the task.
- Neighboring specialist role cards when overlap is suspected.
- Prior Agent Tester findings or run records when supplied.
- TODO or backlog artifacts named by the task when tuning work resolves tracked
  tasks.
- `agents/agent-tuner/eval-plan.md` before reporting eval seeds or acceptance
  gates.
- `agents/agent-tuner/release-rollback.md` before reporting rollback,
  blockers, or promotion status.
- `agents/agent-tuner/run-record.template.json` when producing or updating a
  durable run record.

## Required Output

- Tuned agent id and tuning mode.
- Highest context intake phase reached.
- Source-of-truth and overlap analysis.
- Bounded patch plan or scoped edits.
- Eval seeds and routing recommendations.
- TODO/backlog entries removed, preserved, or newly added.
- Validation evidence.
- Agent Tester review result or blocker.
- Rollback plan, blockers, promotion status, and next owner.

## Stop Conditions

- No target agent or draft description exists.
- Request is new-agent creation, independent testing, protocol governance, or
  product implementation.
- Write scope is missing for requested edits.
- Validation fails.
- Agent Tester review is missing for promotion.
