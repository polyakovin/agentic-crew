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
