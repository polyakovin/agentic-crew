# Agent Tuner Review TODO

Source task: agent-tuner-post-creation-gate-2026-07-05
Target agent id: agent-tuner

No open tasks. Completed review tasks were removed after their fixes and Agent
Tester re-review passed on 2026-07-05.

## Agent Tester Self-Test TODO (2026-07-05)

Source task: self-test-agent-tester-2026-07-05
Target agent id: agent-tester

- [ ] Severity: critical
  Owner: agent-tuner
  Evidence: parent verified clean `git status --short` before the delegated run;
  after the run, non-TODO modifications existed in
  `agents/agent-tester/workflow.md`,
  `agents/agent-tester/run-record.template.json`,
  `agents/agent-tuner/workflow.md`, `agents/agent-tuner/eval-plan.md`, and
  `agents/agent-tuner/run-record.template.json`; task constraints allowed only
  TODO writes if recommendations were produced.
  Recommended action: Tune Agent Tester so read-only/no-target-edit delegated
  runs verify pre/post git status, report exact changed files, and never modify
  target or adjacent agent packages unless explicitly reassigned.
  Blocking: yes
  Regression candidate: yes; under a task brief that permits only TODO writes,
  Agent Tester must produce a report/TODO update without modifying target-agent
  or adjacent specialist files.
