# Locksmith Orchestrator

## Context Economy Contract

### stableContext (always loaded)

- `./role.md` — mission, scope, non-goals, guardrails, routing rules
- `locksmith/AGENTS.md` — architecture diagram, pitfalls, build commands
- `locksmith/docs/manifest.json` — component map, API contracts, agent rules

Progressive reads are triggered by the current task scope. No whole-repo reads
are required as stable context. The orchestrator reads only what is needed for
the current routing decision.

### taskBrief.whatToRead

Variants by task type:

**Code change:**
- `locksmith/docs/<component>.md` — the spec for the component being changed
- `locksmith/source/<component>.lua` or the affected source file(s)
- `locksmith/TODO.md` — if a specific task is referenced

**Spec/contract change:**
- Affected `locksmith/docs/<component>.md` spec
- `locksmith/docs/manifest.json` — for component dependency and API contract
- `locksmith/scripts/spec-review.lua` — linting rules

**Build/test failure:**
- `locksmith/Makefile` — targets and configuration
- `locksmith/scripts/test_runner.lua` — test framework
- `locksmith/scripts/*.lua` — test scripts relevant to failure output

**Cross-cutting (touches multiple components):**
- All specs and source files touched by the task
- `locksmith/docs/workflow.md` — SDD cycle reference

### progressiveReads

- Selected `agentic-crew/agents/<specialist>/` docs when routing to a specialist
- `agentic-crew/packs/playdate-game-crew.yaml` routing table
- `agentic-crew/protocol/interaction-protocol.md` — handoff contract
- `locksmith/docs/test-scenarios.md` — when a QA change is needed
- `locksmith/docs/lore.md` — when lore or character consistency matters

### excludedContext

- Whole-repo reads of locksmith/ or agentic-crew/
- Full copies of spec files in prompts (reference by path only)
- Raw Playdate SDK documentation (refer to AGENTS.md pitfalls section)
- AI-generated summaries of game code not relevant to the task
- Stale build logs from unrelated sessions
