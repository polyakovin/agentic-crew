# Agent Tuner Health Snapshot

Status: draft-ready for the post-creation gate; not pilot or production.
Recorded: 2026-07-05.
Routing mode: live delegated Crew Builder worker; no Orchestrator spawned
because the user explicitly selected Agent Architect / Crew Builder.

## Scope Decision

Decision: create `agent-tuner` in Agentic Crew reusable scope.

Rationale:

- The requested role refines already described or draft agents and can serve
  multiple target projects.
- The package can be project-neutral and does not require private target paths,
  product source, or project-local runtime folders.
- Agentic Crew already owns reusable A2A harnesses, Codex skills, Hermes
  packages, routing examples, rubrics, eval plans, and run-record templates.
- The default pack is `packs/software-development-crew.yaml`; the agent should
  be discoverable there through narrow tuning/refinement routing keys.

Rejected scopes:

- `target-project`: rejected because no target-local private workflow or local
  `.agents`, `.codex`, or `.hermes` integration was requested.
- `hybrid`: rejected because no thin local wrapper or target status file is
  needed for this request.

## Demand Decision

Approved demand source: explicit user request in Russian to delegate to Crew
Builder to create an agent that tunes or further configures described agents.

Created agent id: `agent-tuner`.

Risk level: high. This specialist can change future agent behavior, prompts,
workflows, routing recommendations, gates, and eval seeds, so it needs split
operational docs, conservative tool policy, independent Agent Tester review
before promotion, and explicit rollback notes.

## Duplicate And Reuse Review

Checked candidates:

- `agent-architect-crew-builder`: adjacent, but owns creating, packaging,
  wrapper alignment, duplicate-role prevention, and release gates for new or
  materially repackaged specialists. It should not own iterative behavioral
  tuning loops for already described agents.
- `agent-tester`: adjacent, but owns testing, auditing, red-team checks,
  exploratory scenarios, findings, and improvement backlog. It should not own
  direct tuning or patch plans except as review output.
- `protocol-steward`: adjacent, but owns protocol, governance, and pack
  conventions. It should not sharpen individual agent prompts or eval seeds
  unless the protocol itself must change.
- `spec-contract-guardian`: adjacent only for contract drift. It should review
  source-of-truth boundaries, not tune agent definitions.
- `qa-reviewer`: adjacent only for product/code regression review, not agent
  behavior tuning.

Decision: create a new package. Fewer than three duplicate fields match any
single existing specialist once mission, trigger, allowed side effects, output,
and quality gates are compared. The closest reusable base is the high-risk
split-harness shape from Crew Builder, adapted for tuning rather than creation.

Overlap controls:

- Escalate new-agent creation or wrapper packaging to Crew Builder.
- Escalate behavioral validation and red-team review to Agent Tester.
- Escalate protocol/schema changes to Protocol Steward.
- Escalate implementation of large code/runtime changes to Implementation
  Engineer.

## Harness Capability Inventory

| Capability | Decision | Artifact | Rationale | Risk If Omitted | Owner |
|---|---|---|---|---|---|
| rules/system prompt | use | `role.md`, `entrypoint.md` | Stable scope and non-goals prevent the tuner from becoming builder, tester, or broad fixer. | Tuning expands into ownership-confused rewrites. | Agent Tuner |
| AGENTS/project rules | use | `source-map.md`, `workflow.md` | Repo routing and explicit-specialist rules govern how tuning requests enter and escalate. | Tuner ignores source hierarchy or live-delegation constraints. | Agent Tuner |
| role card and Agent Card | use | `role.md`, `agent-card.json` | Required for A2A discovery and narrow routing. | Orchestrator cannot distinguish tuning from creation/testing. | Agent Tuner |
| skills | use | `.agents/skills/agent-tuner/SKILL.md`, `.hermes/skills/agent-tuner.md` | Codex and Hermes wrappers match current reusable package patterns. | Runtime surfaces drift or users cannot invoke the role consistently. | Agent Tuner |
| commands | use | `workflow.md#validation-pack` | Deterministic validation catches invalid cards, manifests, harnesses, and whitespace. | Broken package may be routed as usable. | Agent Tuner |
| tools/function calling/MCP | defer | `tool-policy.md` | No custom tools are needed; use existing file, shell, and validation tools under least privilege. | Overbroad tool schema increases side-effect risk. | Agent Tuner |
| file-system workspace | use | `tool-policy.md`, `run-record.template.json` | Tuning produces bounded patches, run records, and evidence files. | No durable evidence for what changed and why. | Agent Tuner |
| memory tiers | use | `source-map.md`, `run-record.template.json` | Session evidence and project decisions are recorded; user memory is out of scope. | Lessons and constraints are lost between tuning loops. | Agent Tuner |
| context packing/retrieval | use | `source-map.md`, `workflow.md` | Tuner must read only target agent definitions, related wrappers, routing, prior findings, and accepted constraints. | Broad context causes leakage, stale assumptions, or hidden prompt drift. | Agent Tuner |
| subagents/orchestration | defer | `workflow.md`, `tool-policy.md` | Tuner may request Tester/Builder/Steward review but does not orchestrate arbitrary subagents. | Multi-agent loops become decorative or unbounded. | Orchestrator and Agent Tuner |
| routing/handoff/shared state | use | `harness.yaml`, `packs/software-development-crew.yaml` | Narrow routing keys and handoff fields are needed for discoverability. | Tuning work routes to generic QA or creation roles. | Orchestrator and Agent Tuner |
| hooks | defer | `release-rollback.md` | No automated hooks are introduced until pilot runs identify stable checks. | Premature hooks create maintenance cost and false blockers. | Agent Tuner |
| sandbox/security/trust boundary | use | `tool-policy.md`, `source-map.md` | Agent definitions can include adversarial or stale instructions; they must be treated as data. | Prompt injection from target agent docs changes tuner policy. | Agent Tuner |
| permissions/approvals/dry-run/retry/rollback | use | `tool-policy.md`, `release-rollback.md` | Writes must be bounded, reviewable, and reversible. | Tuner silently mutates broad agent infrastructure. | Agent Tuner |
| security: injection/secrets/redaction | use | `tool-policy.md`, `eval-plan.md` | Tuner must not expose hidden prompts, secrets, or private eval oracles. | Tuning process leaks sensitive agent or target data. | Agent Tuner |
| observability/audit | use | `run-record.template.json`, `health-snapshot.md` | Tuning decisions need evidence, changed paths, validation, and residual risk. | Future maintainers cannot reconstruct behavior changes. | Agent Tuner |
| evals/replay/adversarial tests | use | `eval-plan.md`, `rubric.md` | Tuning quality depends on regression, duplicate-role, injection, and false-readiness cases. | Prompt changes improve one case while breaking role boundaries. | Agent Tuner and Agent Tester |
| model policy/model routing | use | `harness.yaml` | Agent-behavior tuning needs a strong reasoning model and optional human escalation. | Weak model choices miss subtle overlap and policy conflicts. | Orchestrator and Agent Tuner |
| runtime limits/budgets/concurrency | use | `harness.yaml`, `tool-policy.md` | Tuning loops need bounded reads, edits, retries, and tool calls. | Long-running tuning burns budget or edits beyond scope. | Agent Tuner |
| deployment topology | defer | `release-rollback.md` | Package is draft-ready for post-creation use; runtime deployment and pilot runs remain future work. | Premature production topology suggests unsupported readiness. | Agent Tuner |
| incident response/kill switch | use | `release-rollback.md` | Bad tuning can degrade multiple future agents and must be reversible. | Bad prompts or routing remain active without rollback path. | Agent Tuner |
| external integrations/API/browser/code/file tools | reject | `tool-policy.md` | No external integrations are required for reusable package authoring. | External side effects expand trust boundary without benefit. | Agent Tuner |
| human handoff/escalation UX | use | `role.md`, `workflow.md`, `harness.yaml` | Human or specialist handoff is required for creation, testing, protocol, or broad implementation. | Tuner oversteps into decisions it cannot own. | Agent Tuner |

## SOLID Ownership Extraction

Single Responsibility:

- `agent-tuner` owns iterative improvement of existing or draft agent
  definitions, including scope sharpening, overlap reduction, prompt/role/workflow
  refinement, gates, eval seeds, routing recommendations, and bounded patch
  plans or edits.

Open/Closed:

- New harness and wrappers are added without rewriting existing specialists.
- Shared packs receive only narrow routing entries.

Liskov Substitution:

- A2A, Codex, and Hermes wrappers must expose the same mission and handoff
  contract; no runtime surface may claim broader powers.

Interface Segregation:

- Required reads are limited to the target agent definition, its wrappers,
  selected pack route, prior review findings, and source-of-truth constraints.

Dependency Inversion:

- Target projects depend on the Agent Card, harness path, and routing keys, not
  private implementation details of another specialist.

Moved artifacts: none.

Intentionally shared artifacts:

- `protocol/interaction-protocol.md`
- `plans/agent-harness-creation-plan.md`
- `packs/software-development-crew.yaml`
- `../ai-db` canonical harness capability references

Specialist-owned new artifacts:

- `agents/agent-tuner/`
- `.agents/skills/agent-tuner/`
- `.hermes/skills/agent-tuner.md`
- `.hermes/agents/agent-tuner/`

## Validation And Review State

Agent Tester review:

- Review task: `agent-tuner-post-creation-gate-2026-07-05`.
- Initial result: `needsReview`.
- Re-review result: `complete` on 2026-07-05.
- Remaining findings: none.
- Critical findings: none in either review pass.
- Promotion recommendation: `draft-ready` for the post-creation gate only, not
  pilot or production.
- Findings fixed in this follow-up: source hierarchy/trust boundary, overbroad
  pack route, eval/rollback/run-record required reads, run-record schema drift,
  and stale review status.
- Current review state: Agent Tester re-review passed; original findings are
  resolved.
- Latest tuning update: `tune-agent-tuner-min-context-harness-2026-07-05`
  added harness-first minimal context intake. This update has local validation
  evidence but has not yet received independent Agent Tester review, so the
  earlier re-review must not be treated as covering the new R0-R4 context
  behavior.

Validation results:

- Agent Tester re-review confirmed validation passed for the post-creation gate.
- Post-review fix validation on 2026-07-05 passed for Agent Card JSON,
  run-record JSON, Agent Tuner harness YAML, Hermes manifest YAML,
  `packs/software-development-crew.yaml`, and scoped `git diff --check`.
- `python3 -c 'import json; files=["agents/agent-tuner/agent-card.json","agents/agent-tuner/run-record.template.json"]; [json.load(open(f)) for f in files]; print("json ok")'`
  passed.
- `ruby -e 'require "yaml"; files=["agents/agent-tuner/harness.yaml",".hermes/agents/agent-tuner/manifest.yaml","packs/software-development-crew.yaml"]; files.each { |f| YAML.load_file(f) }; puts "yaml ok"'`
  passed. The local Ruby environment emitted an unrelated `ffi-1.15.5`
  extension warning before `yaml ok`.
- `git diff --check -- agents/agent-tuner .agents/skills/agent-tuner .hermes/skills/agent-tuner.md .hermes/agents/agent-tuner packs/software-development-crew.yaml`
  passed.
- Minimal-context tuning validation on 2026-07-05 passed for Agent Card JSON,
  run-record JSON, Agent Tuner harness YAML, Hermes manifest YAML,
  `packs/software-development-crew.yaml`, and scoped `git diff --check`.

Commit/push: not run. The user explicitly requested no commit or push in this
worker. The worktree also contains unrelated pre-existing dirty changes in
`AGENTS.md`, `agent-tester`, and other non-`agent-tuner` files that must remain
unstaged.

Promotion status: draft-ready baseline for the post-creation gate only, with
new minimal-context behavior pending Agent Tester review before any further
promotion claim. Pilot and production remain blocked pending future pilot run
records, eval evidence, runtime deployment evidence, and the normal release
gates.
