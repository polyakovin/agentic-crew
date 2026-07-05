# Agent Teacher Release And Rollback

## Current Status

Status: draft-ready. Required files validate and independent Agent Tester
review `review-agent-teacher-2026-07-05` found no confirmed findings. Pilot and
production remain blocked until sample learning packets, eval evidence, and
pilot records exist.

## Promotion Path

- `draft`: package exists, but validation or independent review is incomplete.
- `draft-ready`: required files validate and Agent Tester review has no
  unresolved critical findings.
- `pilot`: at least one target project uses Agent Teacher and records a
  sanitized run record with learning materials and follow-up handoffs.
- `production`: multiple pilot records, eval evidence, privacy review, and
  independent review show the teaching loop improves target-agent behavior.

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
2. One sample learning packet is produced from sanitized evidence.
3. Redaction process is reviewed.
4. Handoff packets to Agent Tuner and Agent Tester are exercised or simulated.

Before production:

1. Pilot records cover at least two target agents or two materially different
   domains.
2. Replay/eval evidence shows reduced repeated failures.
3. Incident and rollback process is exercised.
4. Human owner approves promotion.

## Rollback

Rollback package changes by reverting the scoped files:

- `agents/agent-teacher/**`
- `.agents/skills/agent-teacher/**`
- `.hermes/skills/agent-teacher.md`
- `.hermes/agents/agent-teacher/**`
- `packs/software-development-crew.yaml` entries for `agent-teacher`

Rollback target-agent teaching output by:

1. Removing or reverting the learning material path named in the run record.
2. Reverting associated TODO/backlog entries only when they were created by the
   same run and no longer apply.
3. Preserving the run record and redaction summary for audit unless the owner
   requires deletion for privacy.
4. Re-opening any `agent-tuner` or `agent-tester` handoff that depended on the
   rolled-back material.

## Incident Response

Use an incident path when:

- raw private data, hidden prompts, secrets, or credentials enter reusable
  learning materials;
- untrusted logs or pages change the teacher's instructions;
- the teacher edits target prompts or wrappers without reassignment;
- a target agent is promoted based on teaching material without Agent Tester
  review;
- learning material causes a repeated failure or unsafe action.

Response:

1. Stop new Agent Teacher runs for the affected target agent.
2. Identify the run record and all material paths touched.
3. Remove or quarantine unsafe learning material.
4. Notify the target owner and correct specialist owner.
5. Create an Agent Tester review task.
6. Add an eval seed for the incident failure mode after redaction.

## Release Blockers

- Missing target agent or learning objective.
- Unbounded or unredactable history required.
- Unsupported or uncited best-practice claim.
- Learning material without provenance.
- Private data written to reusable material.
- Tuning or testing performed without explicit reassignment.
- Required handoff missing.
- Machine-readable validation failed.
- Independent Agent Tester review missing for package promotion.
