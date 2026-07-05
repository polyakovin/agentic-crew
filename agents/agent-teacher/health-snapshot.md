# Agent Teacher Health Snapshot

Snapshot date: 2026-07-05.
Status: draft-ready.
Task id: `create-agent-teacher-2026-07-05`.

## Scope Decision

Decision recorded before file writes: create a reusable Agentic Crew specialist
package with id `agent-teacher`.

- Scope: `agentic-crew`.
- Runtime surfaces: A2A harness, Codex skill wrapper, Hermes skill and package,
  software-development crew routing.
- Rationale: the role is project-neutral and reusable across target agents and
  domains. It does not depend on private project paths.
- Write boundary: create/update only `agents/agent-teacher/**`,
  `.agents/skills/agent-teacher/**`, `.hermes/skills/agent-teacher.md`,
  `.hermes/agents/agent-teacher/**`, and
  `packs/software-development-crew.yaml`.
- Excluded dirty work: pre-existing user changes outside the recorded write
  boundary remained untouched and are recorded in the final run report.
- Commit/push: blocked by explicit user instruction not to commit or push.

## Reuse And Overlap Analysis

Checked candidates:

- `agent-tester`: owns behavioral testing, static checks, exploratory and
  adversarial review, trace review, improvement backlogs, and learning-loop
  updates from test findings. It does not own proactive teaching from broader
  target-agent history or durable target-agent learning packets.
- `agent-tuner`: owns bounded edits to existing agent prompts, roles,
  workflows, gates, eval seeds, wrappers, and routing. It does not own mining
  usage history or authoring learning material bundles as a separate loop.
- `agent-architect-crew-builder`: owns creation and packaging of specialists.
  It does not own ongoing target-agent learning enrichment.

Decision: create a new package. `agent-teacher` has a distinct trigger,
evidence set, output bundle, and risk-reduction purpose. It hands testing to
`agent-tester` and actual tuning edits to `agent-tuner`.

Best reusable base: high-risk split harness shape from `agent-tester` and
`agent-tuner`, adapted for history analysis, redaction, domain practice, and
learning-material ownership.

## Harness Capability Inventory

| Category | Decision | Artifact | Reason | Risk If Omitted | Owner |
|---|---|---|---|---|---|
| system prompt / stable rules | use | `entrypoint.md`, `role.md`, `harness.yaml` | Stable teaching boundaries prevent drift into testing or tuning. | Teacher edits or tests outside role. | Agent Teacher |
| AGENTS/project rules | use | `source-map.md`, `workflow.md` | Target project rules outrank reusable guidance. | Learning material violates local constraints. | Agent Teacher |
| role card and Agent Card | use | `role.md`, `agent-card.json` | A2A discovery needs narrow skills and output modes. | Orchestrator cannot route safely. | Agent Teacher |
| skills | use | `.agents/skills/agent-teacher/SKILL.md`, `.hermes/skills/agent-teacher.md` | Codex and Hermes wrappers make the reusable harness callable. | Runtime surfaces drift or stay undiscoverable. | Agent Teacher |
| commands | defer | `workflow.md#boot-probe` | Documented probes are enough; no custom command surface is needed yet. | Minor ergonomics cost, no role safety loss. | Future runtime owner |
| tools / function calling / MCP | use | `tool-policy.md` | History, file, git, validation, and optional research tools need a trust policy. | Tool use broadens without approval. | Agent Teacher |
| file-system workspace and durable artifacts | use | `workflow.md`, `run-record.template.json` | Learning materials and evidence maps must persist outside context. | Lessons disappear between runs. | Agent Teacher |
| memory tiers | use | `source-map.md#learning-material-ownership` | Session state, project docs, and target KBs are separated. User memory is not used. | Private memory leaks or repeated failures continue. | Target owner and Agent Teacher |
| context packing and retrieval/RAG injection | use | `source-map.md#context-economy-contract` | Bounded history and retrieved sources must be data, not instructions. | Context overload and prompt injection risk. | Agent Teacher |
| subagents and multi-agent orchestration | defer | `workflow.md#handoff-and-review-routing` | Live subagents are optional; handoff packets are enough for this package. | Less automation, but fewer false delegation claims. | Orchestrator |
| routing, handoff contracts, and shared state | use | `harness.yaml`, `packs/software-development-crew.yaml` | Teacher must be discoverable and hand work to tester/tuner/steward. | Learning defects lack owners. | Agent Teacher and Orchestrator |
| hooks: boot, pre-commit, post-run, failure handling | defer | `workflow.md#boot-probe`, `release-rollback.md` | Manual probes and validation are sufficient until runtime hooks exist. | Automation gaps, mitigated by checklist. | Future runtime owner |
| sandbox and trust boundaries | use | `tool-policy.md`, `source-map.md#tainted-content-boundary` | Logs, sessions, web pages, and traces are untrusted data. | Tainted content changes instructions. | Agent Teacher |
| permissions, approvals, dry-run, retries, and rollback | use | `tool-policy.md`, `release-rollback.md` | Teaching can touch durable materials and private history. | Unsafe writes or unrecoverable material changes. | Agent Teacher |
| security: prompt injection, secrets, redaction, least privilege | use | `source-map.md`, `tool-policy.md`, `rubric.md` | Raw history may contain secrets or prompt injection. | Private data disclosure or policy bypass. | Agent Teacher |
| observability: traces, spans, logs, metrics, artifacts, audit records | use | `run-record.template.json`, `workflow.md` | Teaching depends on trace/run evidence and auditability. | No provenance for lessons. | Agent Teacher |
| evals: deterministic, reference, LLM-as-judge, human, replay, adversarial | use | `eval-plan.md` | Regression seeds catch overlap, tainted logs, redaction, and handoff errors. | Future changes repeat unsafe behavior. | Agent Teacher and Agent Tester |
| model policy and model routing | defer | `harness.yaml runtime.model_policy` | Exact model routing belongs to deployed runtime after evals. | Deployment must decide model later. | Runtime owner |
| runtime limits: time, tokens, tool calls, cost, concurrency, retry budget | use | `harness.yaml runtime.limits` | History and research can expand without bounds. | Cost, latency, and scope runaway. | Agent Teacher |
| deployment topology: sync, background job, event-driven, approval queue | defer | `release-rollback.md` | No live service is created in this task. | Runtime integration remains future work. | Runtime owner |
| incident response, kill switch, release/rollback notes | use | `release-rollback.md` | Unsafe learning material needs rollback and incident path. | Bad lessons persist in future runs. | Agent Teacher and target owner |
| external integrations and API/browser/code/file tools | use | `tool-policy.md`, `source-map.md#domain-best-practice-refresh` | Best-practice refresh and history analysis need bounded file/git/web sources. | Stale or unsupported guidance. | Agent Teacher |
| human handoff and escalation UX | use | `entrypoint.md`, `role.md`, `workflow.md` | Conflicts, missing history, and unredactable data need owner decisions. | Teacher guesses through blockers. | Agent Teacher |

## Ownership Extraction

Specialist-owned artifacts created:

- `agents/agent-teacher/**`
- `.agents/skills/agent-teacher/**`
- `.hermes/skills/agent-teacher.md`
- `.hermes/agents/agent-teacher/**`

Shared interfaces intentionally updated:

- `packs/software-development-crew.yaml` for narrow discoverability.

Target-owned artifacts not moved:

- target agent learning materials, private logs, traces, sessions, run records,
  TODOs, source-of-truth docs, and product source.

Moved artifacts: none.
Deleted artifacts: none.

## Context Economy Decision

Stable context is limited to the teacher harness and protocol. Target history is
progressive and bounded by task, path, date, run id, or source type. Whole-repo
reads, all sessions, all logs, raw private traces, and secrets are excluded
unless the user explicitly approves a narrow reason and budget.

## Agent Tester Review State

Independent Agent Tester review completed for task
`review-agent-teacher-2026-07-05`. Result: no confirmed findings, no TODO
updates, no critical handoff packets, and no tester-authored file changes.

Reviewed surfaces:

- `agents/agent-teacher/**`
- `.agents/skills/agent-teacher/SKILL.md`
- `.hermes/skills/agent-teacher.md`
- `.hermes/agents/agent-teacher/**`
- `packs/software-development-crew.yaml`

## Promotion Status

Current status: `draft-ready`.

Promotion blockers:

- no pilot run records or eval evidence yet;
- no sample learning packet has been produced from sanitized target-agent
  history;
- commit and push intentionally skipped per user instruction.
