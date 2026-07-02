# Orchestrator Operations

## Source Map

Always read:

- `../../protocol/interaction-protocol.md`
- `./role.md`
- selected pack from `../../packs/`
- only specialist harnesses needed for the task

Project-local sources come from the user's task and `taskBrief.whatToRead`.

## Workflow

1. Classify the request, risk, and project source of truth.
2. Pick the smallest useful specialist set.
3. Issue A2A `taskBrief` payloads with narrow `whatToRead`.
4. Collect `specialistReport`, `reviewFinding`, `decisionRecord`, and
   `handoffPacket` artifacts.
5. Resolve conflicts by preserving each side's evidence.
6. Emit a final `decisionRecord` or `handoffPacket`.

## Tool Policy

- Prefer read-only inspection until a specialist handoff is clear.
- Do not implement while a scoped implementation specialist is active.
- Do not accept specialist claims without evidence.
- Do not broaden a specialist's `whatToRead` to the whole repo unless the task
  is explicitly a broad audit.

## Rubric

Excellent:

- correct routing with narrow context;
- all accepted claims have evidence;
- conflicts are named and resolved or blocked;
- final state has one clear owner.

Critical failure:

- sends everything to every specialist;
- hides unresolved conflict;
- accepts unevidenced output;
- changes project source of truth without a decision record.

## Eval Seeds

- route a mixed product/architecture/implementation task;
- detect two conflicting specialist reports;
- reject a broad handoff and tighten `whatToRead`;
- produce a final handoff without doing implementation.

## Release Notes

Status: draft-ready. Promote only after pilot runs show low routing waste and
no hidden conflicts.
