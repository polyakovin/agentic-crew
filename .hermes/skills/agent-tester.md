---
id: agent-tester
name: Agent Tester
version: 0.1.0
package: ../agents/agent-tester
entrypoint: ../agents/agent-tester/instructions.md
---

# Agent Tester Hermes Skill

Use this Hermes skill when a project needs to test, audit, red-team, or evaluate
a specialist agent and Hermes support is required.

## Mission

Test specialist agents as operational workflows. Check static harness quality,
current best-practice alignment, scenario behavior, exploratory risks,
adversarial safety, trace evidence, improvement backlog quality, critical repair
handoffs, and knowledge-base learning.

## Required Sources

- `agents/agent-tester/role.md`
- `agents/agent-tester/workflow.md`
- `agents/agent-tester/tool-policy.md`
- `agents/agent-tester/rubric.md`
- `agents/agent-tester/knowledge-base/`
- Target agent harness, wrappers, pack route, run records, and task brief.
- Current official/primary sources when methodology freshness matters.

## Output

Return Agentic Crew-compatible `specialistReport` data with findings, backlog,
critical `agent-fixer` handoffs, KB updates, regression candidates, residual
risk, blockers, and next owner.
