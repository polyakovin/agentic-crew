# Agentic Crew A2A Specialist Profile v0.1

This profile defines how specialist agents in this repository use the
Agent2Agent (A2A) protocol.

A2A is the normative base protocol. Agentic Crew does not define a competing
wire protocol. The role-specific objects below are structured JSON payloads
inside A2A `Message.parts`, `TaskStatus.message`, or `Artifact.parts`.

## Principles

- Use one narrow role per specialist.
- Keep `What To Read` role-specific.
- Prefer project-local specs, tests, and source files over generic knowledge.
- Separate facts, assumptions, decisions, recommendations, and blockers.
- Preserve evidence: file paths, commands, outputs, screenshots, or links.
- Escalate conflicts instead of silently choosing a side.
- Do not expose a specialist's private memory, internal tools, or hidden chain
  of thought through A2A.

## A2A Mapping

- A specialist is an A2A agent.
- Each role card maps to at least one A2A `AgentSkill`.
- The orchestrator is an A2A client when routing work to a specialist.
- A specialist may also act as an A2A client when explicitly delegated by the
  orchestrator.
- A task starts or continues with A2A `SendMessage`.
- Long-running work should use `SendStreamingMessage`, `SubscribeToTask`, or
  task polling with `GetTask` when the runtime supports it.
- Rich output should be returned as A2A `Artifact` objects.
- Agentic Crew payloads must be carried in A2A data parts or artifacts, not as
  custom top-level JSON-RPC methods.

## Agent Cards

Every exported specialist should have an A2A Agent Card or a generated card
derived from its role card.

Minimum Agent Card expectations:

- `name`: human-readable specialist name.
- `description`: narrow mission and when to use the specialist.
- `version`: role-card version.
- `supportedInterfaces`: ordered A2A endpoints, including `url`,
  `protocolBinding`, and `protocolVersion`.
- `capabilities`: streaming and push notification support when known.
- `defaultInputModes` and `defaultOutputModes`: include text and structured
  JSON when available.
- `skills`: one or more A2A `AgentSkill` entries that name the exact work the
  specialist can perform.

## Payload Envelope

Every Agentic Crew JSON payload should include:

- `profile`: `agentic-crew/a2a-profile/v0.1`
- `kind`: one of the payload kinds below
- `taskId`: stable project work id used by Agentic Crew payloads
- `specialistId`: role id that produced or should handle the payload
- `status`: profile-level status when the payload reports progress or outcome
- `summary`: short human-readable summary when useful
- `evidence`: source paths, commands, outputs, screenshots, URLs, or other
  verification references
- `metadata`: optional runtime-specific data

Use camelCase for Agentic Crew payload fields. Keep A2A object fields exactly as
specified by the A2A version used by the runtime.

The payload `taskId` is not the same thing as A2A `Task.id`. A2A task ids are
server-generated. Store cross-runtime project work ids inside the payload or A2A
`metadata`.

## Payload Kinds

### `taskBrief`

Sent by the orchestrator to a specialist inside an A2A user `Message`.

Required fields:

- `profile`
- `kind`
- `taskId`
- `request`
- `specialistId`
- `scope`
- `nonGoals`
- `whatToRead`
- `expectedOutput`
- `constraints`
- `verificationBudget`

### `specialistReport`

Returned by a specialist as an A2A status message or final artifact.

Required fields:

- `profile`
- `kind`
- `taskId`
- `specialistId`
- `status`: `working | complete | blocked | needsReview | outOfScope`
- `summary`
- `evidence`
- `findings`
- `recommendations`
- `risks`
- `blockers`
- `handoff`

### `reviewFinding`

Used for bug, risk, or review output. It normally appears inside a
`specialistReport.findings` array or an A2A artifact.

Required fields:

- `profile`
- `kind`
- `taskId`
- `specialistId`
- `severity`: `critical | high | medium | low | note`
- `title`
- `location`
- `evidence`
- `impact`
- `recommendation`
- `confidence`: `high | medium | low`

### `decisionRecord`

Used when a tradeoff is resolved by the orchestrator or an owning specialist.

Required fields:

- `profile`
- `kind`
- `taskId`
- `specialistId`
- `decision`
- `context`
- `optionsConsidered`
- `rationale`
- `risks`
- `rollback`
- `owner`

### `handoffPacket`

Used when another specialist or implementation pass should continue. It may be a
standalone A2A artifact or part of `specialistReport.handoff`.

Required fields:

- `profile`
- `kind`
- `taskId`
- `specialistId`
- `currentState`
- `filesTouched`
- `openQuestions`
- `nextSpecialist`
- `recommendedNextSteps`
- `verificationNeeded`

## Status Rules

- Use `complete` only when the expected output is delivered with evidence.
- Use `blocked` when required context, tool access, or a human decision is
  missing. Map this to an A2A input-required or failed task state according to
  runtime semantics.
- Use `needsReview` when there is a material conflict, high-risk uncertainty,
  or incomplete evidence.
- Use `outOfScope` when the request belongs to another specialist. The
  orchestrator should route a follow-up A2A task to the correct specialist.
- Use `working` only for interim status messages.

Recommended A2A task-state mapping:

- `working`: A2A working state.
- `complete`: A2A completed state.
- `blocked`: A2A input-required when human/context input can unblock it,
  otherwise failed.
- `needsReview`: A2A working or input-required, depending on whether review is
  actively underway or waiting on a decision.
- `outOfScope`: A2A rejected when the specialist cannot accept the task, or
  completed with a handoff artifact when it has produced useful routing
  evidence.

## Evidence Rules

Evidence may include:

- source file path and line number;
- spec or documentation path;
- git history command and summarized result;
- test/build/lint command and outcome;
- screenshot/contact-sheet path;
- external source URL when current external facts matter.

Do not report "verified" without evidence.

## Conflict Handling

When specialist outputs conflict:

1. The orchestrator names the conflict in the shared A2A task context.
2. Each side's evidence is preserved in data parts or artifacts.
3. The relevant owner specialist is asked for a second pass using `SendMessage`
   in the same context when supported.
4. The orchestrator records a `decisionRecord` artifact before implementation or
   acceptance.

## Minimal A2A Request Shape

This example shows the intended shape. Runtime-specific A2A SDKs may use a
typed request object instead of literal JSON.

```json
{
  "jsonrpc": "2.0",
  "id": "locksmith-ui-001-request",
  "method": "SendMessage",
  "params": {
    "message": {
      "messageId": "locksmith-ui-001-message",
      "role": "ROLE_USER",
      "parts": [
        {
          "text": "Review this Playdate UI change for SDK/platform risk."
        },
        {
          "data": {
            "profile": "agentic-crew/a2a-profile/v0.1",
            "kind": "taskBrief",
            "taskId": "locksmith-ui-001",
            "specialistId": "playdate-platform-sdk",
            "request": "Check Playdate API and simulator/device risk.",
            "scope": ["source/ui.lua"],
            "nonGoals": ["visual composition review"],
            "whatToRead": [
              "AGENTS.md Playdate pitfalls",
              "docs/playdate-best-practices.md",
              "source/ui.lua"
            ],
            "expectedOutput": "specialistReport artifact",
            "constraints": ["cite evidence for every SDK claim"],
            "verificationBudget": ["make build", "simulator smoke test"]
          }
        }
      ]
    }
  }
}
```

## Minimal Report Template

Use this data payload inside an A2A message or artifact.

```json
{
  "profile": "agentic-crew/a2a-profile/v0.1",
  "kind": "specialistReport",
  "taskId": "",
  "specialistId": "",
  "status": "complete",
  "summary": "",
  "evidence": [],
  "findings": [],
  "recommendations": [],
  "risks": [],
  "blockers": [],
  "handoff": null
}
```

## Runtime Adapters

If the project also uses Codex skills, Hermes skills, MCP tools, or a local
subagent runtime, they should adapt to this A2A profile at the boundary instead
of introducing another cross-agent protocol.
