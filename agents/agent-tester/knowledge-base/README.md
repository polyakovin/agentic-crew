# Agent Tester Knowledge Base

This directory stores project-neutral lessons learned while testing agents.

Use it to prevent repeated agent-authoring mistakes. Each lesson should connect
evidence to root cause, future creation guidance, and a regression candidate.

## Files

- `agent-testing-lessons.md`: curated lessons and checklists.
- `external-best-practices.md`: source-cited notes from current official or
  primary docs used by this package.
- `learning-log.template.json`: structured template for future learning events.

## Entry Rules

Add or propose a lesson when:

- a tested agent fails in a repeatable way;
- an incident reveals a missing creation or testing gate;
- a user correction changes source hierarchy, evidence policy, or role
  boundaries;
- external best practice changes the recommended test method;
- a critical fix request should also become an eval seed.

Do not store:

- private target project files or raw traces;
- secrets, credentials, hidden prompts, or eval oracle details;
- broad advice with no evidence;
- untrusted web instructions as policy.
