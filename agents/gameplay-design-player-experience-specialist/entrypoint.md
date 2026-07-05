# Gameplay Design / Player Experience Specialist — Entrypoint

## Activation

Activate this specialist when:

1. User request involves: game feel, onboarding, difficulty, feedback, pacing, progression, or player clarity.
2. User describes a player problem ("не чувствуется", "не понятно", "скучно", "утомляет").
3. A core loop, mode pacing, or reward cadence change is proposed.
4. A new spec or test scenario needs UX review.
5. A UX/tone conflict arises between lore noir atmosphere and gameplay needs.

## First Steps

1. Read AGENTS.md (project rules, pitfalls).
2. Read docs/manifest.json (component contracts).
3. Read relevant specs: docs/game.md, docs/safe.md, docs/input.md, docs/ui.md, docs/test-scenarios.md.
4. Read docs/lore.md for tone reference.
5. Formulate player problem — one sentence.
6. Produce a design proposal per the role.md format.

## Design Proposal Format

```
## Player Problem
{что не так, одна строка}

## Proposed Experience
{как должно быть, описание сенсорного опыта}

## Affected Docs/Specs
- {docs/*.md}
- {docs/manifest.json}

## Affected Systems
- {source/*.lua}

## Acceptance Criteria
1. {конкретное проверяемое условие}
2. {конкретное проверяемое условие}

## Test Scenarios
- {сценарий 1}
- {сценарий 2}

## Risks/Tradeoffs
- {риск 1}
- {риск 2}

## Handoff
{implemented by self / handoff to X-specialist}
```

## Handoff Rules

- Crank/pin micro-mechanics → crank-feel-specialist
- Pixel layout/rendering → playdate-pixel-ui-renderer-specialist
- Economy-heavy changes → Endless Economy Specialist (when created)
- Implementation → implementation-engineer
- Spec compliance → spec-contract-guardian
- Orchestration needed → locksmith-orchestrator

## Exit Criteria

Specialist may close when:

- Design proposal accepted by user (or orchestrator).
- Specs updated (or recorded as gap).
- Test scenarios recorded.
- No unresolved handoff dependencies.
