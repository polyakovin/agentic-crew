# Agent Tuner Release And Rollback

## Release Status

Current status: draft-ready for the post-creation gate.

Do not promote to pilot or production until:

- at least one pilot tuning run record exists;
- validation passes for Agentic Crew, Codex, Hermes, and pack surfaces;
- eval seeds cover role overlap, false readiness, prompt injection, wrapper
  drift, and handoff routing;
- rollback path is tested on a bounded tuning change.

## Release Blockers

- Target agent missing or only implied.
- Request is creation, independent testing, protocol governance, or product
  implementation rather than tuning.
- Source-of-truth conflict has no owner decision.
- Agent Tester review missing or unavailable for promotion.
- Agent Tester reports unresolved critical findings.
- Agent Card JSON, run-record JSON, harness YAML, Hermes manifest YAML, pack
  YAML, TOML, or whitespace validation fails.
- Unrelated dirty worktree changes prevent atomic staging, commit, or push.

## Rollback Scope

Rollback may affect:

- `agents/agent-tuner/`
- `.agents/skills/agent-tuner/`
- `.hermes/skills/agent-tuner.md`
- `.hermes/agents/agent-tuner/`
- `packs/software-development-crew.yaml` routing entries for tuning

For tuning runs against other agents, rollback only the target files named in
the run record.

## Rollback Steps

1. Identify the bad tuning change, run id, and affected surfaces.
2. Preserve the run record and validation failure evidence.
3. Revert only affected Agent Tuner files, target agent files, or routing keys.
4. Re-run JSON/YAML/TOML/Markdown whitespace validation.
5. Request Agent Tester review for the rollback when the bad change affected
   behavior, routing, or wrapper alignment.
6. Record residual risk and next owner.

## Smoke Checks

```bash
python3 -m json.tool agents/agent-tuner/agent-card.json
python3 -m json.tool agents/agent-tuner/run-record.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-tuner/harness.yaml"); YAML.load_file(".hermes/agents/agent-tuner/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); puts "yaml ok"'
git diff --check
```
