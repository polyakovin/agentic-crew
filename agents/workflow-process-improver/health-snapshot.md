# Workflow Process Improver Health Snapshot

Snapshot date: 2026-07-05.
Status: draft-ready.
Task id: `workflow-process-improver-agent-2026-07-05`.

## Scope Decision

Decision recorded before package writes in
`plans/workflow-process-improver-scope-decision.md`: create a reusable Agentic
Crew specialist package with id `workflow-process-improver`.

- Scope: `agentic-crew`.
- Runtime surfaces: A2A harness, Codex skill wrapper, Hermes skill and package,
  software-development crew routing.
- Rationale: the role is project-neutral and reusable across projects. It does
  not depend on private product paths.
- Write boundary: create/update only `agents/workflow-process-improver/**`,
  `.agents/skills/workflow-process-improver/**`,
  `.hermes/skills/workflow-process-improver.md`,
  `.hermes/agents/workflow-process-improver/**`,
  `packs/software-development-crew.yaml`, `TODO.md`, and the scope decision
  record.
- Excluded dirty work: pre-existing changes under `agents/playdate-platform-sdk/**`
  remain untouched.

## Reuse And Overlap Analysis

Checked candidates:

- `agent-tester`: owns behavioral testing of specialist agents, static checks,
  exploratory scenarios, adversarial tests, trace review, improvement backlog,
  and tester-authored TODO updates. It does not own general project workflow
  process retrospectives or Multica proposal placement.
- `agent-teacher`: owns teaching a named target agent from prior history,
  recurring failures, and domain best practices. It does not own project
  workflow process change proposals or information-capture improvements for
  project operations.
- `agent-tuner`: owns prompt, role, workflow, gate, eval, wrapper, and routing
  refinement for existing or draft agents. It does not own mining project
  workflow evidence or writing process proposals.
- `qa-reviewer`: owns product regression, correctness, usability, and test
  coverage. It does not own project workflow process retrospectives.
- `protocol-steward`: owns Agentic Crew protocol, harness, and pack governance.
  It does not own target project process proposal capture unless a protocol
  change is required.

Decision: create a new package. `workflow-process-improver` has a distinct
trigger, evidence set, output destination, and risk-reduction purpose. It hands
testing to `agent-tester`, learning to `agent-teacher`, tuning to
`agent-tuner`, protocol governance to `protocol-steward`, and implementation to
`implementation-engineer`.

Best reusable base: high-risk split harness shape from `agent-teacher`,
`agent-tester`, and `agent-tuner`, adapted for workflow timeline analysis,
Multica proposal destination discovery, and information-capture improvement.

## Multica CLI Discovery

Discovery evidence:

- Repository-local search found no Multica config, backlog file, or command
  contract in this repository.
- `which multica` found `/opt/homebrew/bin/multica`.
- `multica --version` reported `multica 0.3.38`.
- `multica issue create --help` provided the concrete issue-create command
  shape.
- `multica project list --output json` identified project
  `590c34c6-1c3c-49b3-b662-34ebd8cf34b4` (`Locksmith`).
- Follow-up proposals from this creation/review run were written to Multica as
  `POL-3` and `POL-4`.

Decision: the new specialist may treat installed CLI help/output plus an
approved project destination as concrete Multica destination evidence, but
future side-effecting `multica issue create` or `multica issue update` calls
still require explicit approval and rollback evidence. `TODO.md` mirrors the
Multica issue ids for local traceability.

## Harness Capability Inventory

| Category | Decision | Artifact | Reason | Risk If Omitted | Owner |
|---|---|---|---|---|---|
| system prompt / stable rules | use | `entrypoint.md`, `role.md`, `harness.yaml` | Stable boundaries prevent drift into testing, tuning, teaching, or product edits. | Process agent overreaches or gives generic advice. | Workflow Process Improver |
| AGENTS/project rules | use | `source-map.md`, `workflow.md` | Target project rules define write scope, evidence scope, and fallback TODO policy. | Local constraints and direct-by-default rules are ignored. | Workflow Process Improver |
| role card and Agent Card | use | `role.md`, `agent-card.json` | A2A discovery needs narrow skills and output modes. | Crew routing cannot identify the specialist safely. | Workflow Process Improver |
| skills | use | `.agents/skills/workflow-process-improver/SKILL.md`, `.hermes/skills/workflow-process-improver.md` | Codex and Hermes wrappers expose the reusable harness without duplicating it. | Runtime surfaces drift or remain unavailable. | Workflow Process Improver |
| commands | use | `workflow.md#boot-probe`, `workflow.md#validation-pack` | Discovery and validation require repeatable command probes. | Multica absence and validation state become unverifiable. | Workflow Process Improver |
| tools / function calling / MCP | use | `tool-policy.md` | File, git, shell, TODO, and potential Multica CLI tools need side-effect gates. | Tool use broadens without approvals or dry-run policy. | Workflow Process Improver |
| file-system workspace and durable artifacts | use | `run-record.template.json`, `workflow.md` | Evidence inventory, proposals, and handoffs must persist outside context. | Retrospectives cannot be replayed. | Workflow Process Improver |
| memory tiers: session, project, knowledge base, user memory when allowed | use | `source-map.md`, `run-record.template.json` | Session evidence, project TODOs, and reusable harness files are separated. User memory is not used. | Private data leaks or lessons are stored in the wrong tier. | Target owner and Workflow Process Improver |
| context packing and retrieval/RAG injection | use | `source-map.md#context-economy-contract` | Logs and sessions must be bounded and treated as data. | Context overload and prompt injection risk. | Workflow Process Improver |
| subagents and multi-agent orchestration | defer | `workflow.md#handoff-routing` | Handoff packets are enough for this package; live subagents depend on runtime availability. | Less automation, but avoids false delegation claims. | Orchestrator and target owner |
| routing, handoff contracts, and shared state | use | `harness.yaml`, `packs/software-development-crew.yaml` | The specialist must be discoverable and route follow-up owners. | Process findings lose owner and destination. | Workflow Process Improver |
| hooks: boot, pre-commit, post-run, failure handling | defer | `workflow.md#boot-probe`, `release-rollback.md` | Manual probes and validation are sufficient until a runtime hook system exists. | Automation gaps remain, mitigated by checklist. | Future runtime owner |
| sandbox and trust boundaries | use | `tool-policy.md`, `source-map.md#tainted-content-boundary` | Logs, sessions, and issue comments are untrusted. | Tainted content changes instructions. | Workflow Process Improver |
| permissions, approvals, dry-run, retries, and rollback | use | `tool-policy.md`, `release-rollback.md` | Multica writes and external task-system side effects need approval and rollback. | Wrong project-board changes or unsafe writes. | Workflow Process Improver and target owner |
| security: prompt injection, secrets, redaction, least privilege | use | `source-map.md`, `tool-policy.md`, `rubric.md` | Workflow evidence can contain secrets, PII, and prompt injection. | Private data disclosure or policy bypass. | Workflow Process Improver |
| observability: traces, spans, logs, metrics, artifacts, audit records | use | `workflow.md`, `run-record.template.json` | The role directly improves observability and evidence capture. | Findings cannot be proven or future analysis stays weak. | Workflow Process Improver |
| evals: deterministic, reference, LLM-as-judge, human, replay, adversarial | use | `eval-plan.md` | Scenarios cover Multica absence, tainted logs, broad reads, and overlap. | Future changes regress destination, safety, or ownership behavior. | Workflow Process Improver and Agent Tester |
| model policy and model routing | defer | `harness.yaml runtime.model_policy` | Exact model routing belongs to deployed runtime after evals. | Runtime must decide model later. | Runtime owner |
| runtime limits: time, tokens, tool calls, cost, concurrency, retry budget | use | `harness.yaml runtime.limits` | Logs and sessions can expand without bounds. | Cost, latency, and broad-read runaway. | Workflow Process Improver |
| deployment topology: sync, background job, event-driven, approval queue | defer | `release-rollback.md` | No live service is created in this task. | Runtime integration remains future work. | Runtime owner |
| incident response, kill switch, release/rollback notes | use | `release-rollback.md` | Wrong Multica writes or private-data leaks need response. | Unsafe proposals persist or external state is wrong. | Workflow Process Improver and target owner |
| external integrations and API/browser/code/file tools | use | `tool-policy.md`, `source-map.md#multica-proposal-destination-policy` | Multica and project-management writes need explicit integration policy. | Proposals are lost or written unsafely. | Workflow Process Improver |
| human handoff and escalation UX | use | `entrypoint.md`, `role.md`, `workflow.md` | Missing CLI, broad reads, external writes, and owner conflicts need human decisions. | Agent guesses through blockers. | Workflow Process Improver |

## SOLID Ownership Extraction

Single Responsibility:

- `workflow-process-improver` owns workflow process retrospectives,
  information-capture gaps, and proposal placement. It does not own product
  implementation, agent testing, teaching, or tuning.

Open/Closed:

- The package adds a new specialist and routing entries without rewriting
  common harnesses or existing agent packages.

Liskov Substitution:

- A2A, Codex, and Hermes surfaces all return Agentic Crew `specialistReport`
  payloads and point to the same canonical harness.

Interface Segregation:

- Required reads are bounded to proposal destination and workflow evidence, not
  all project source or all history.

Dependency Inversion:

- Target projects depend on the Agent Card, pack routing keys, and handoff
  contract. The specialist depends on target evidence by reference, not copied
  private data.

Specialist-owned artifacts created:

- `agents/workflow-process-improver/**`
- `.agents/skills/workflow-process-improver/**`
- `.hermes/skills/workflow-process-improver.md`
- `.hermes/agents/workflow-process-improver/**`

Shared interfaces intentionally updated:

- `packs/software-development-crew.yaml` for narrow discoverability.
- `TODO.md` for current task fallback because no Multica CLI artifact exists.

Target-owned artifacts not moved:

- product source, private logs, private sessions, traces, project-management
  credentials, target TODOs, source-of-truth docs, and telemetry code.

Moved artifacts: none.
Deleted artifacts: none.

## Context Economy Decision

Stable context is limited to this harness and protocol. Target workflow
evidence is progressive and bounded by task id, path, date window, run id,
project-board filter, or search query. Whole-repo reads, all logs, all sessions,
raw private traces, secrets, and product source are excluded unless the user
explicitly approves a narrow reason and budget.

## Agent Tester Review State

Independent Agent Tester review completed through delegated worker
`019f31c9-5786-70d3-9587-f6c6b155c02d` with no evidence-backed
`reviewFinding` defects. Agent Tester recommended promotion to `draft-ready`
only. Pilot remains blocked on scenario and pilot run evidence.

## Promotion Status

Current status: `draft-ready`.

Promotion blockers:

- no pilot retrospective run records;
- no scenario eval evidence yet;
- approved future Multica write contract and rollback expectations remain open
  in `POL-3`;
- pilot eval/run evidence remains open in `POL-4`.
