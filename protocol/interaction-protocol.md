# Agentic Crew Interaction Protocol v0.1

This protocol defines how specialist agents communicate with an orchestrator and
with each other. Every role card in this repository must use it.

## Principles

- Use one narrow role per specialist.
- Keep `What To Read` role-specific.
- Prefer project-local specs, tests, and source files over generic knowledge.
- Separate facts, assumptions, decisions, recommendations, and blockers.
- Preserve evidence: file paths, commands, outputs, screenshots, or links.
- Escalate conflicts instead of silently choosing a side.

## Message Types

### `task_brief`

Sent by the orchestrator to a specialist.

Required fields:

- `task_id`
- `request`
- `specialist`
- `scope`
- `non_goals`
- `what_to_read`
- `expected_output`
- `constraints`
- `verification_budget`

### `specialist_report`

Returned by a specialist.

Required fields:

- `task_id`
- `specialist`
- `status`: `complete | blocked | needs-review | out-of-scope`
- `summary`
- `evidence`
- `findings`
- `recommendations`
- `risks`
- `blockers`
- `handoff`

### `review_finding`

Used for bug/risk/review output.

Required fields:

- `severity`: `critical | high | medium | low | note`
- `title`
- `location`
- `evidence`
- `impact`
- `recommendation`
- `confidence`: `high | medium | low`

### `decision_record`

Used when a tradeoff is resolved.

Required fields:

- `decision`
- `context`
- `options_considered`
- `rationale`
- `risks`
- `rollback`
- `owner`

### `handoff_packet`

Used when another specialist or implementation pass should continue.

Required fields:

- `current_state`
- `files_touched`
- `open_questions`
- `next_specialist`
- `recommended_next_steps`
- `verification_needed`

## Status Rules

- Use `complete` only when the expected output is delivered with evidence.
- Use `blocked` when required context, tool access, or a human decision is
  missing.
- Use `needs-review` when there is a material conflict, high-risk uncertainty,
  or incomplete evidence.
- Use `out-of-scope` when the request belongs to another specialist.

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

1. The orchestrator names the conflict.
2. Each side's evidence is preserved.
3. The relevant owner specialist is asked for a second pass.
4. The orchestrator records a `decision_record` before implementation or
   acceptance.

## Minimal Report Template

```md
## Specialist Report

- task_id:
- specialist:
- status:
- summary:
- evidence:
- findings:
- recommendations:
- risks:
- blockers:
- handoff:
```

