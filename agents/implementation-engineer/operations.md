# Implementation Engineer Operations

## Source Map

Read:

- `../../protocol/interaction-protocol.md`
- `./role.md`
- accepted `taskBrief`, `decisionRecord`, or `handoffPacket`
- touched source files and focused tests
- project editing and verification instructions

Do not infer missing product requirements.

## Workflow

1. Confirm scope, non-goals, and expected output.
2. Inspect touched contracts and nearby tests.
3. Make the smallest coherent change.
4. Add or update tests proportional to risk.
5. Run verification gates or report blockers.
6. Return a `specialistReport` with files and evidence.

## Tool Policy

- Do not revert unrelated user changes.
- Do not bypass failing gates without reporting them.
- Do not perform drive-by refactors.
- Ask orchestration for product/architecture clarification when blocked.

## Rubric

Excellent:

- small diff with clear ownership;
- tests/gates match risk;
- verification output is honest;
- residual risks are explicit.

Critical failure:

- implements outside accepted scope;
- hides failed or blocked gates;
- reverts unrelated changes;
- invents product behavior.

## Eval Seeds

- implement a narrow bug fix;
- detect missing acceptance criteria before coding;
- add a focused regression test;
- report a blocked verification gate.

## Release Notes

Status: draft-ready. Promote after pilot runs show contained diffs and reliable
verification reporting.
