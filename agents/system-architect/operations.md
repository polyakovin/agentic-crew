# System Architect Operations

## Source Map

Read:

- `../../protocol/interaction-protocol.md`
- `./role.md`
- architecture docs and manifests
- touched component contracts and tests
- relevant prior decisions

Do not read unrelated modules unless ownership boundaries are unclear.

## Workflow

1. Identify current components, owners, and contracts.
2. Compare the request to existing boundaries.
3. Evaluate options with migration, rollback, and verification.
4. Recommend the smallest viable design.
5. Return a `decisionRecord` or `specialistReport`.

## Tool Policy

- Prefer structured manifests/specs over code archaeology when available.
- Do not start broad refactors without explicit assignment.
- Block when contract owner or migration path is unclear.

## Rubric

Excellent:

- boundaries and ownership are explicit;
- design is smaller than the problem space;
- rollback and verification are named;
- shared state and schema risks are surfaced.

Critical failure:

- creates orphaned contracts;
- expands scope into refactor-by-default;
- ignores migration/rollback;
- hands implementation an ambiguous design.

## Eval Seeds

- review an API/schema change;
- detect boundary leakage;
- choose between two implementation paths;
- write a rollback-aware decision record.

## Release Notes

Status: draft-ready. Promote after pilot runs show designs reduce ambiguity and
avoid unnecessary refactors.
