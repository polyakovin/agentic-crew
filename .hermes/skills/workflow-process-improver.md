---
id: workflow-process-improver
name: Workflow Process Improver
version: 0.1.0
package: ../agents/workflow-process-improver
entrypoint: ../agents/workflow-process-improver/instructions.md
---

# Workflow Process Improver Hermes Skill

Use this Hermes skill when a project needs to audit how workflow tasks actually
happened and turn evidence-backed process improvements into tracked proposals.

## Mission

Analyze bounded logs, sessions, run records, TODOs, review findings, handoffs,
task events, and project workflow evidence. Reconstruct the workflow timeline,
find bottlenecks and information-capture gaps, improve authorized capture
surfaces, and write proposals to Multica CLI or an approved fallback artifact.
For Multica writes, preserve approval source, destination id, sanitized command
shape, response `id`/`identifier`/`status`/`project_id`, duplicate-check result,
retry count, and rollback path in the run record.

## Required Sources

- `agents/workflow-process-improver/role.md`
- `agents/workflow-process-improver/source-map.md`
- `agents/workflow-process-improver/workflow.md`
- `agents/workflow-process-improver/tool-policy.md`
- Target project rules, task brief, Multica destination evidence, accepted
  write scope, and bounded workflow evidence.

## Output

Return Agentic Crew-compatible `specialistReport` data with target project,
workflow scope, proposal destination, evidence inventory, timeline, process
findings, information-capture gaps, improvements applied, proposals written,
Multica result or fallback TODO/backlog updates, validation, handoffs, blockers,
risks, proposal-write approval evidence, rollback path, and promotion status.
