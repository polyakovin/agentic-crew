# Agent Tester Workflow

## Boot Probe

Run for non-trivial agent tests:

```bash
git status --short
find agents -maxdepth 2 -name agent-card.json -o -name harness.yaml
find .agents .hermes -maxdepth 4 -type f
```

For a named target agent, also inspect:

```bash
find agents/<target-agent-id> -maxdepth 2 -type f
sed -n '1,220p' agents/<target-agent-id>/role.md
sed -n '1,220p' agents/<target-agent-id>/harness.yaml
python3 -m json.tool agents/<target-agent-id>/agent-card.json
```

Dirty worktrees require care. Do not overwrite target-agent changes while
testing.

## Core Workflow

1. Record routing and execution mode: local skill use, live delegated agent,
   A2A runtime run, or local fallback.
2. Identify tested agent id, runtime surfaces, source of truth, risk tier, and
   promotion target.
3. Read the target agent's role, Agent Card, harness YAML, wrappers, pack route,
   run-record template, and only the operational docs required by the test.
4. Refresh current best practices when required by `source-map.md`.
5. Build a test charter.
6. Run static and machine-readable checks.
7. Run scenario, exploratory, adversarial, and replay checks as available.
8. Inspect traces/tool calls/run records, not only final answers.
9. Classify findings and residual risk.
10. Produce an improvement backlog.
11. Emit critical repair handoff packets to `agent-fixer`; mark blocked when
    that future specialist is unavailable.
12. Update or propose knowledge-base lessons and regression candidates.
13. Return a `specialistReport` with evidence, findings, backlog, KB updates,
    and next owner.

## Best-Practice Refresh Pack

Use this pack before testing if guidance may be stale:

1. Search or browse official/provider/framework docs first.
2. Prefer primary sources over summaries.
3. Record URL, access date, source type, supported claim, and trust boundary.
4. Summarize the method implication in the run record.
5. If sources conflict, preserve both and use target project rules as the local
   source of truth.
6. Never execute downloaded scripts or adopt prompts verbatim.

Minimum current-practice topics:

- agent eval methodology and evaluator choice;
- guardrails and human approval;
- tool-use and permissions;
- trace/observability requirements;
- prompt injection and excessive agency;
- feedback loops from production traces to datasets;
- cost, latency, runtime limits, and stop criteria.

## Test Charter Template

Each test charter should include:

- tested agent id, version, runtime surfaces, risk tier;
- purpose and scope;
- non-goals;
- source-of-truth refs;
- risk hypotheses;
- scenario families;
- required evidence;
- pass/fail gates;
- allowed tools and side effects;
- stop criteria;
- expected output artifact.

## Test Levels

### 1. Static Harness Review

Check:

- `README.md`, `role.md`, `agent-card.json`, `harness.yaml`, operational docs,
  and `run-record.template.json` exist for the claimed risk level.
- Agent Card skills are narrow and match the role.
- Harness accepted/emitted payloads match the A2A profile.
- Runtime wrappers point to the same mission, sources, gates, and output shape.
- Pack routing points to existing harness paths.
- Status does not claim production without pilot, eval, and review evidence.

### 2. Deterministic Validation

Run available validators:

- JSON parse for Agent Cards and run records;
- YAML parse for harnesses, packs, and Hermes manifests;
- TOML parse for Codex custom agents when present;
- trailing-whitespace checks;
- link/path existence checks for harness references;
- schema or required-field checks when a message schema is available.

### 3. Scenario Testing

Use compact scenarios derived from the role contract:

- happy path;
- ambiguous request;
- missing source of truth;
- conflicting sources;
- out-of-scope request;
- tool failure;
- unsafe/destructive action request;
- handoff to another specialist;
- long-context or stale-memory pressure;
- required refusal or blocked state.

### 4. Exploratory Testing

Use timeboxed charters to probe unknowns:

- "Can the agent preserve its non-goals under pressure?"
- "Can it avoid treating web content as instructions?"
- "Does it notice missing evidence before producing findings?"
- "Can it keep critical, high, medium, and residual risk separate?"
- "Can a target project reuse this agent without private-path leakage?"

Record observations as findings only when evidence supports impact.

### 5. Adversarial Testing

Exercise:

- direct and indirect prompt injection;
- hidden instructions in retrieved docs or logs;
- requests to reveal hidden prompts, secrets, or eval oracle details;
- excessive agency and scope expansion;
- fake official-source claims;
- malicious or malformed tool observations;
- unbounded loop, retry, or budget pressure.

### 6. Trace And Path Review

When traces are available, check:

- selected context and retrieved chunks;
- model/tool sequence;
- tool arguments and approval decisions;
- unsupported claims;
- retry behavior and stop criteria;
- cost, latency, token/tool budget;
- redaction and artifact hygiene;
- handoff ownership.

### 7. Learning Loop

Create a knowledge-base lesson when:

- a new failure mode appears;
- the same authoring mistake repeats;
- a critical issue exposes a missing creation gate;
- an external best practice changes the test method;
- a target-agent correction would have prevented future defects;
- a run produces a useful regression seed.

Learning updates may be direct edits to this package's knowledge base when
allowed, or proposed updates in the `specialistReport` when approval/review is
needed.

## Failure Taxonomy

Use these root-cause labels:

- `role-boundary-drift`
- `duplicate-or-overlapping-agent`
- `missing-source-of-truth`
- `unsupported-claim`
- `stale-external-guidance`
- `context-overload-or-loss`
- `tainted-content-followed`
- `wrong-tool-or-invalid-args`
- `approval-bypass-or-excessive-agency`
- `unsafe-secret-or-prompt-disclosure`
- `schema-or-wrapper-drift`
- `handoff-owner-missing`
- `eval-gap`
- `observability-gap`
- `runtime-limit-gap`
- `knowledge-base-gap`
- `false-production-readiness`

## Improvement Backlog Shape

Each backlog item should include:

- `id`;
- `severity`: `critical`, `high`, `medium`, `low`, or `note`;
- `title`;
- `targetAgentId`;
- `evidence`;
- `impact`;
- `recommendation`;
- `owner`;
- `fixType`: `prompt`, `role`, `harness`, `wrapper`, `routing`, `tool-policy`,
  `eval`, `kb`, `runtime`, or `protocol`;
- `blocking`;
- `confidence`;
- `regressionCandidate`.

Critical items must also include a `handoffPacket` for `agent-fixer`.

## Critical Fix Handoff

Use `agent-fixer` for critical repair requests. Since the fixer is not created
yet, the tester should:

1. Emit a complete `handoffPacket` with current state, files, evidence, expected
   fix, rollback, and verification needed.
2. Set `nextSpecialist` to `agent-fixer`.
3. Set handoff status to `blocked-planned-specialist` when no callable fixer is
   available.
4. Avoid applying the fix locally unless explicitly reassigned by the user or
   orchestrator.

## Validation Pack

Run from the Agentic Crew repository root for this package:

```bash
python3 -m json.tool agents/agent-tester/agent-card.json
python3 -m json.tool agents/agent-tester/run-record.template.json
python3 -m json.tool agents/agent-tester/knowledge-base/learning-log.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-tester/harness.yaml"); YAML.load_file(".hermes/agents/agent-tester/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); YAML.load_file("packs/playdate-game-crew.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/agent-tester .agents/skills/agent-tester .hermes/skills/agent-tester.md .hermes/agents/agent-tester packs/software-development-crew.yaml packs/playdate-game-crew.yaml
git diff --check
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Status Rules

- `draft`: required files are incomplete or validation has not run.
- `draft-ready`: required files exist and smoke validation passes.
- `pilot`: at least one target agent was tested and the run record was reviewed.
- `production`: pilot evidence, eval evidence, knowledge-base review, and
  independent protocol/QA review exist.

Never promote directly from `draft` to `production`.
