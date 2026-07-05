# Agent Tuner Review TODO

Source task: agent-tuner-post-creation-gate-2026-07-05
Target agent id: agent-tuner

No open tasks. Completed review tasks were removed after their fixes and Agent
Tester re-review passed on 2026-07-05.

## Agent Tester Self-Test TODO (2026-07-05)

Source task: self-test-agent-tester-2026-07-05
Target agent id: agent-tester

No open tasks. The critical self-test remediation entry was removed after the
bounded Agent Tuner pass on 2026-07-05 added write-boundary, no-target-edit,
changed-file reporting, and Agent Tuner handoff gates to Agent Tester.

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
