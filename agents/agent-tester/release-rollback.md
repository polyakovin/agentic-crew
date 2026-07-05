# Agent Tester Release And Rollback

## Release Status

Draft-ready package. Not production/governed until pilot records, eval evidence,
knowledge-base review, and independent protocol review exist.

## Release Blockers

- Uncited current best-practice claim.
- Critical finding without `agent-fixer` handoff packet.
- Target-private data written into reusable knowledge base.
- Tester edits the target agent while claiming to only test.
- Production readiness recommended without pilot/eval/review evidence.
- Tainted content changes instructions, source hierarchy, tool policy, or
  severity.
- Agent Card JSON, harness YAML, Hermes manifest YAML, or run-record JSON fails
  validation.

## Rollback Scope

Rollback applies to this specialist package only:

- `agents/agent-tester/`
- `.agents/skills/agent-tester/SKILL.md`
- `.hermes/skills/agent-tester.md`
- `.hermes/agents/agent-tester/`
- routing entries in `packs/software-development-crew.yaml`

Do not revert unrelated user work or target project source files.

## Rollback Procedure

1. Identify the bad harness or wrapper change.
2. Preserve the failing run record and findings as evidence.
3. Revert only affected Agent Tester files or routing entries.
4. Re-run JSON/YAML/whitespace validation.
5. Add a KB lesson or eval candidate if the rollback reveals a new failure
   mode.

## Smoke Validation

```bash
python3 -m json.tool agents/agent-tester/agent-card.json
python3 -m json.tool agents/agent-tester/run-record.template.json
python3 -m json.tool agents/agent-tester/knowledge-base/learning-log.template.json
ruby -e 'require "yaml"; YAML.load_file("agents/agent-tester/harness.yaml"); YAML.load_file(".hermes/agents/agent-tester/manifest.yaml"); YAML.load_file("packs/software-development-crew.yaml"); puts "yaml ok"'
rg -n "[[:blank:]]$" agents/agent-tester .agents/skills/agent-tester .hermes/skills/agent-tester.md .hermes/agents/agent-tester packs/software-development-crew.yaml
git diff --check
```
