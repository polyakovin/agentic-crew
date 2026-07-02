---
name: agentic-crew-author
description: Create, adapt, or review specialist-agent role cards and crew packs using the Agentic Crew shared interaction protocol. Use when designing reusable agent teams, Codex skills, Codex custom subagents, Hermes-style specialists, routing tables, or handoff protocols for software projects.
---

# Agentic Crew Author

Use this skill to create or update specialist-agent teams based on the role
cards and protocol in this repository.

## Required Sources

Read these before creating or changing a role:

1. `protocol/interaction-protocol.md`
2. Existing role cards in `roles/`
3. The target pack in `packs/`, if any
4. The target project's local specs, instructions, tests, and git history

## Role Authoring Rules

- Keep each role narrow.
- Keep `What To Read` narrow and role-specific.
- Include Mission, Use When, Scope, Non-goals, What To Read, Workflow,
  Minimum Deliverable, Quality Gates, Blockers, and Handoff Contract.
- Require `specialist_report` or another protocol message as output.
- Do not copy third-party prompts verbatim.
- Treat external frameworks as structure donors, not source of truth.

## Pack Authoring Rules

- Every pack must reference `protocol/interaction-protocol.md`.
- Every role entry needs `id`, `path`, and `required`.
- Routing keys should name work types, not vague departments.
- Prefer optional specialists unless a role is always needed.

## Review Checklist

- Does each role prevent a real failure mode?
- Is there overlap with another role?
- Is `What To Read` small enough for the role?
- Does the role have evidence-based quality gates?
- Does the role know when to block?
- Can the orchestrator route work using the pack's routing table?

