# Agent Tester Health Snapshot

Status: draft-ready package after smoke validation, pending pilot records, eval
evidence, and independent review.

## Scope Decision

- Scope: `agentic-crew`.
- Rationale: testing agents is reusable across target projects and should live
  with shared specialist harnesses, wrappers, run-record templates, and
  knowledge-base guidance.
- Runtime surfaces: A2A, Codex skill, Hermes package.
- Target-local wrappers: none requested.
- Routing/execution note: this was an explicit Crew Builder delegation, so the
  Orchestrator was not spawned. The main Codex thread spawned a live delegated
  Crew Builder worker named `Newton`
  (`019f2f97-bb39-7530-aac4-5e8ea8607169`) with the Crew Builder skill and task
  brief because no exact Crew Builder runtime role was exposed. Any nested
  delegation from that worker was unavailable in its own context, so its
  follow-up review used the provided harness context directly.

## Reuse Analysis

- `qa-reviewer`: adjacent but product-change focused; not enough because it
  does not own agent workflow testing, KB lessons, or fixer handoff.
- `protocol-steward`: adjacent but protocol/harness-governance focused; not
  enough because it does not own exploratory behavior testing or current
  best-practice refresh.
- `agent-architect-crew-builder`: best structural base for a high-risk split
  harness; adapted for testing rather than creation.
- Decision: create new package with high-risk split docs adapted from the Crew
  Builder pattern.

## Capability Inventory

| Category | Decision | Artifact | Reason | Risk if omitted | Owner |
|---|---|---|---|---|---|
| rules/system prompt | use | `role.md`, `entrypoint.md` | Stable testing boundaries prevent tester from becoming fixer. | Tester mutates the target agent or broadens into unrelated QA. | Agent Tester |
| AGENTS/project rules | use | `source-map.md`, `workflow.md` | Repo routing and evidence rules govern every test. | Tests ignore project source hierarchy or live-delegation rules. | Agent Tester |
| role card and Agent Card | use | `role.md`, `agent-card.json` | Required for A2A discovery and narrow routing. | Orchestrator cannot discover or scope the tester reliably. | Agent Tester |
| skills | use | `.agents/skills/agent-tester/SKILL.md`, `.hermes/skills/agent-tester.md` | Codex and Hermes wrappers are required for portable use. | Runtime surfaces drift or the tester is usable only as local docs. | Agent Tester |
| commands | use | `workflow.md#validation-pack` | Deterministic validators reduce false confidence. | Invalid cards, manifests, or routing can be reported as ready. | Agent Tester |
| tools/function/MCP | use | `tool-policy.md` | Tool, browser, and file use must be scoped and audited. | Unsafe tool paths hide behind acceptable final answers. | Agent Tester |
| file-system workspace | use | `knowledge-base/`, `run-record.template.json` | Durable KB and run artifacts are central to learning. | Evidence and lessons disappear between runs. | Agent Tester |
| memory tiers | use | `knowledge-base/agent-testing-lessons.md` | Project-neutral lessons prevent repeated authoring mistakes. | The same agent defects repeat without becoming guidance. | Agent Tester |
| context packing/retrieval | use | `source-map.md` | Tainted external content must be separated from instructions. | Web pages, traces, or logs can override tester behavior. | Agent Tester |
| subagents/orchestration | defer | `workflow.md#critical-fix-handoff` | Handoff is specified; live fixer is future work. | Critical repairs lack an owner until `agent-fixer` exists. | Agent Tester and future Agent Fixer |
| routing/handoff/shared state | use | `harness.yaml`, `packs/software-development-crew.yaml` | Orchestrator needs narrow routing and next-owner fields. | Agent-testing tasks route to generic QA or vanish. | Orchestrator and Agent Tester |
| hooks | defer | `release-rollback.md` | No automatic hooks until pilot validates workflow. | Premature automation could enforce uncalibrated checks. | Agent Tester |
| sandbox/security/trust boundary | use | `tool-policy.md`, `source-map.md` | Agent testing must resist prompt injection and excessive agency. | Tainted inputs can change policy or suppress findings. | Agent Tester |
| permissions/approvals/retry/rollback | use | `tool-policy.md`, `release-rollback.md` | Critical findings and writes need explicit gates. | The tester may fix, delete, or publish without approval. | Agent Tester |
| secrets/redaction/least privilege | use | `tool-policy.md` | Traces and KB updates must not leak private data. | Reusable knowledge files could expose private traces or secrets. | Agent Tester |
| observability/audit | use | `workflow.md#6-trace-and-path-review`, `run-record.template.json` | Path-level testing requires traces and audit references. | Final-answer checks miss unsafe or unreliable execution paths. | Agent Tester |
| evals/replay/adversarial | use | `eval-plan.md` | Regression and adversarial seeds are core deliverables. | Failures are not converted into repeatable tests. | Agent Tester |
| model policy/routing | use | `harness.yaml` | Strong model policy for agent-evaluation tasks. | Low-capability model choices may miss subtle workflow risks. | Orchestrator and Agent Tester |
| runtime limits/budgets/concurrency | use | `workflow.md`, `tool-policy.md` | Tests must consider cost, latency, tool budgets, and stop criteria. | Exploratory testing can sprawl or ignore production constraints. | Agent Tester |
| deployment topology | defer | `release-rollback.md` | No deployed A2A service yet. | Speculative topology would add maintenance without reducing draft risk. | Crew Builder |
| incident response/kill switch | use | `release-rollback.md` | Bad tester behavior must be rollbackable. | Unsafe tester behavior may keep receiving routed work. | Orchestrator and Protocol Steward |
| external integrations/browser/API/code/file tools | use | `tool-policy.md`, `knowledge-base/external-best-practices.md` | Internet refresh and validation are necessary but bounded. | Current-practice claims go stale or uncited. | Agent Tester |
| human handoff/escalation | use | `role.md`, `workflow.md` | Critical fixes and ambiguous KB updates need owners. | Critical issues remain vague backlog entries. | Agent Tester and future Agent Fixer |

## Current Gaps

- No pilot run against a completed target agent.
- No independent Protocol Steward review yet.
- `agent-fixer` does not exist; critical handoff packets are ready but blocked
  until that specialist is created.
- No automated hook integration.
- No commit or push was performed by this delegated pass because the worktree
  contains prior uncommitted rule changes owned by the main agent/user.

## Promotion Notes

Remain `draft-ready` until:

- smoke validation passes;
- at least one target agent test produces a reviewed run record;
- KB update flow is exercised without private-data leakage;
- independent review confirms the tester does not overlap QA Reviewer,
  Protocol Steward, or Crew Builder.

## Validation Evidence

Passed:

- `python3 -m json.tool agents/agent-tester/agent-card.json`
- `python3 -m json.tool agents/agent-tester/run-record.template.json`
- `python3 -m json.tool agents/agent-tester/knowledge-base/learning-log.template.json`
- `ruby -e 'require "yaml"; YAML.load_file("agents/agent-tester/harness.yaml"); YAML.load_file(".hermes/agents/agent-tester/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); YAML.load_file("packs/playdate-game-crew.yaml"); puts "yaml ok"'`
- `rg -n "[[:blank:]]$" agents/agent-tester .agents/skills/agent-tester .hermes/skills/agent-tester.md .hermes/agents/agent-tester packs/software-development-crew.yaml packs/playdate-game-crew.yaml`
- `git diff --check`

Notes:

- The trailing-whitespace check passed with `rg` exit code 1, meaning no
  matches were found.
- Ruby printed a local `ffi-1.15.5` extension warning before `yaml ok`; YAML
  parsing still succeeded.
