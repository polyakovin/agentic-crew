# Protocol Steward Operations

## Source Map

Read:

- `../../protocol/interaction-protocol.md`
- `../../protocol/message-schema.json`
- `./role.md`
- affected `agents/<id>/` folders
- affected `packs/*.yaml`

Do not edit project product behavior.

## Workflow

1. Check that each specialist has `README.md`, `role.md`, `agent-card.json`,
   `harness.yaml`, and required operational references.
2. Validate A2A payload compatibility.
3. Check AgentSkill scope and overlap.
4. Check pack routing points to harness folders.
5. Recommend split/merge/version changes.
6. Return a `specialistReport` with protocol findings.

## Tool Policy

- Use JSON/YAML validators before accepting machine-readable files.
- Treat protocol changes as versioned.
- Do not broaden roles to satisfy every use case.
- Require run-record/eval updates for behavior-affecting harness changes.

## Rubric

Excellent:

- catches missing harness files and stale references;
- keeps roles narrow;
- maintains A2A profile compatibility;
- names migration impact for protocol changes.

Critical failure:

- approves loose role/card files without harness;
- allows custom non-A2A protocol drift;
- misses broken pack paths;
- broadens a specialist into a universal agent.

## Eval Seeds

- review a new agent folder;
- detect broken pack references;
- catch an AgentSkill that is too broad;
- version a protocol payload change.

## Release Notes

Status: draft-ready. Promote after pilot runs show it prevents harness drift and
keeps packs machine-checkable.
