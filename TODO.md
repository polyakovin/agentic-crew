# Agent Tuner Review TODO

Source task: agent-tuner-post-creation-gate-2026-07-05
Target agent id: agent-tuner

No open tasks. Completed review tasks were removed after their fixes and Agent
Tester re-review passed on 2026-07-05.

## Workflow Process Improver TODO (2026-07-05)

Source task: workflow-process-improver-agent-2026-07-05
Target agent id: workflow-process-improver

Repository-local Multica CLI discovery found no config or backlog artifact in
this repository. Host-level discovery found `/opt/homebrew/bin/multica`,
confirmed `multica issue create --help`, and identified project
`590c34c6-1c3c-49b3-b662-34ebd8cf34b4` (`Locksmith`). Follow-up proposals are
now tracked in Multica as `POL-3` and `POL-4`; this TODO section mirrors them
for repository-local traceability.

- [x] [medium][agent-tester][blocking-for-promotion] Review the new Workflow
  Process Improver package before any promotion beyond `draft`. Agent Tester
  review `workflow-process-improver-agent-2026-07-05-agent-tester-review`
  completed with no unresolved critical findings; promotion recommendation is
  `draft-ready` only, not pilot or production. Reviewed surfaces:
  `agents/workflow-process-improver/**`,
  `.agents/skills/workflow-process-improver/SKILL.md`,
  `.hermes/skills/workflow-process-improver.md`,
  `.hermes/agents/workflow-process-improver/**`,
  `packs/software-development-crew.yaml`, and
  `plans/workflow-process-improver-scope-decision.md`.
- [x] [medium][workflow-process-improver][blocked-on-multica-write-contract]
  Define or approve the concrete Multica CLI proposal write destination and
  command contract for this repository, including issue/project destination,
  create/update command shape, dry-run or preview behavior, side-effect approval
  requirements, retry policy, and rollback expectations. Evidence:
  repository-local search found no Multica config or command contract; parent
  review context reports a working host-level read command and project id
  `590c34c6-1c3c-49b3-b662-34ebd8cf34b4`, which is discovery evidence but not
  approval to create or update issues. Multica issue: `POL-3`
  (`b037b5f9-2176-48b8-bcd6-2e7bef040ee6`). Approved contract v1 requires
  approval state/source, destination id, sanitized command shape, response
  `id`/`identifier`/`status`/`project_id`, duplicate-check result, retry count,
  and rollback path in WPI run records. Verification proposal: `POL-5`
  (`606f18c5-6815-4ce0-948c-235224a88dff`).
- [ ] [medium][workflow-process-improver][before-pilot] Exercise at least five
  eval-plan scenario seeds and record one sample retrospective run that writes
  proposals to the approved Multica destination or authorized fallback before
  any pilot promotion. Evidence: `agents/workflow-process-improver/eval-plan.md`
  promotion requirements and `agents/workflow-process-improver/release-rollback.md`
  pilot checklist require scenario coverage, proposal destination evidence, and
  a tracked information-capture improvement. Multica issue: `POL-4`
  (`249c6d06-db14-46c5-b79b-11c41c1c8a85`).

## Agent Tester Self-Test TODO (2026-07-05)

Source task: self-test-agent-tester-2026-07-05
Target agent id: agent-tester

No open tasks. The critical self-test remediation entry was removed after the
bounded Agent Tuner pass on 2026-07-05 added write-boundary, no-target-edit,
changed-file reporting, and Agent Tuner handoff gates to Agent Tester.

## Agent Architect / Crew Builder Test TODO (2026-07-05)

Source task: agent-architect-crew-builder-test-2026-07-05
Target agent id: agent-architect-crew-builder

ACB-001, ACB-002, and ACB-003 were resolved in the delegated Agent Tuner pass
on 2026-07-05. Promotion remains blocked until Agent Tester re-review confirms
the tuned surfaces.

- [ ] [medium][agent-tester][blocking-for-promotion] Re-review the tuned Crew
  Builder surfaces for ACB-001, ACB-002, and ACB-003 before any promotion;
  changed surfaces: `agents/agent-architect-crew-builder/workflow.md`,
  `agents/agent-architect-crew-builder/eval-plan.md`,
  `.agents/skills/agent-architect-crew-builder/SKILL.md`,
  `.hermes/agents/agent-architect-crew-builder/instructions.md`, and
  `.hermes/skills/agent-architect-crew-builder.md`.
- [ ] [medium][protocol-steward] ACB-004: Clarify pack `extends` merge
  semantics or explicitly add `agent-tuner` tuning routes to
  `playdate-game-crew`; evidence: `packs/playdate-game-crew.yaml:5` and
  `packs/software-development-crew.yaml:69`.

## Spec / Contract Guardian Agent Tester TODO (2026-07-05)

Source task: test-spec-contract-guardian-2026-07-05
Target agent id: spec-contract-guardian

SCG-AT-001, SCG-AT-002, and SCG-AT-004 were resolved in the bounded
Spec / Contract Guardian tuning pass on 2026-07-05. The remaining promotion
blocker is pilot/eval evidence.

- [ ] [medium][agent-tester][blocking-for-promotion] SCG-AT-003: Add pilot/eval
  run evidence before promoting this R2 specialist beyond `draft-ready`.
  Evidence: `agents/spec-contract-guardian/harness.yaml:15`,
  `agents/spec-contract-guardian/harness.yaml:18`,
  `agents/spec-contract-guardian/run-record.template.json:5`,
  `agents/spec-contract-guardian/operations.md:94`, and
  `find agents/spec-contract-guardian -maxdepth 3 -type f -iname '*run*' -o
  -iname '*trace*' -o -iname '*eval*' -o -iname '*record*'` returned only
  `agents/spec-contract-guardian/run-record.template.json`. Recommended action:
  run and record scenario coverage for stale manifest entries, missing API
  contracts, pure-boundary leaks, code-only SDD changes, no-finding in-sync
  evidence, and handoff routing. Regression candidate: store a sanitized run
  record proving at least one pass/fail case per core contract family.
