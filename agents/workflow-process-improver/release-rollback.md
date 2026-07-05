# Workflow Process Improver Release And Rollback

## Current Status

Status: draft-ready. Required package files were created for A2A, Codex, and
Hermes surfaces, validation passed, and independent Agent Tester review found
no unresolved critical findings. Pilot promotion remains blocked until scenario
and pilot run evidence exists.

## Promotion Path

- `draft`: package exists, but validation or independent review is incomplete.
- `draft-ready`: required files validate and Agent Tester review has no
  unresolved critical findings.
- `pilot`: at least one target project uses Workflow Process Improver and
  records a retrospective run with proposal destination evidence.
- `production`: multiple pilot records, eval evidence, privacy review, and
  independent review show the process loop improves workflow tracking.

Do not promote directly from draft to production.

## Release Checklist

Before draft-ready:

1. Agent Card JSON parses.
2. Run-record template JSON parses.
3. Harness YAML, Hermes manifest YAML, and selected pack YAML parse.
4. Markdown/YAML/JSON/TOML touched by the package has no trailing whitespace.
5. Codex and Hermes wrappers point to the same mission, sources, gates, and
   output shape.
6. Agent Tester review is recorded with no unresolved critical findings.

Before pilot:

1. At least five eval-plan scenario seeds are exercised.
2. One sample process retrospective writes proposals to Multica CLI or an
   authorized TODO/backlog fallback and records approval source, destination id,
   sanitized command shape, response `id`/`identifier`/`status`/`project_id`,
   duplicate-check result, retry count, and rollback path.
3. One information-capture gap is turned into a tracked proposal or safe
   template update.
4. Redaction process is reviewed.

Before production:

1. Pilot records cover at least two target projects or materially different
   workflows.
2. Replay/eval evidence shows reduced missing workflow evidence or better
   proposal tracking.
3. Incident and rollback process is exercised.
4. Human owner approves promotion.

## Rollback

Rollback package changes by reverting the scoped files:

- `agents/workflow-process-improver/**`
- `.agents/skills/workflow-process-improver/**`
- `.hermes/skills/workflow-process-improver.md`
- `.hermes/agents/workflow-process-improver/**`
- `packs/software-development-crew.yaml` entries for
  `workflow-process-improver`
- `plans/workflow-process-improver-scope-decision.md`

Rollback target-project process output by:

1. Reverting the TODO/backlog or Multica proposals created by the same run when
   they are incorrect and no longer apply.
2. Reverting template or workflow-capture edits named in the run record.
3. Preserving the run record and redaction summary for audit unless the owner
   requires deletion for privacy.
4. Re-opening handoffs that depended on rolled-back process evidence.

## Incident Response

Use an incident path when:

- raw private data, hidden prompts, secrets, credentials, or PII enter tracked
  process artifacts;
- untrusted logs, sessions, or issue comments change instructions;
- proposals are written to the wrong Multica project or board;
- external project-management state is changed without approval;
- product code or protocol schema is changed during a process-improvement run;
- a workflow process is promoted based on unsupported or fabricated evidence.

Response:

1. Stop new Workflow Process Improver runs for the affected project.
2. Identify the run record and all destination entries touched.
3. Quarantine or redact unsafe artifacts.
4. Notify the target owner and correct specialist owner.
5. Create an Agent Tester review task.
6. Add an eval seed for the incident failure mode after redaction.

## Release Blockers

- Missing target project or workflow scope.
- Unbounded or unredactable workflow evidence required.
- Missing Multica destination and no fallback backlog.
- Proposal not written to any durable destination.
- External side effect without approval.
- Product code or protocol changed without assignment.
- Process finding without evidence.
- Tainted content changed instructions or policy.
- Machine-readable validation failed.
- Independent Agent Tester review missing for package promotion.
