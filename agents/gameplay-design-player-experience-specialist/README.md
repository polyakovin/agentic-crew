# Gameplay Design / Player Experience Specialist

## Overview

A specialist agent for the Locksmith Playdate game. Owns the moment-to-moment game feel, onboarding, feedback, difficulty, pacing, and progression loops. Works spec-first within Spec-Driven Development.

## When to Route Here

Use routing keys: `gameplay-design`, `player-experience`, `ux-design`, `difficulty-tuning`, `core-loop`, `onboarding`, `feedback-clarity`, `mode-pacing`, `progression-design`

## What It Produces

1. Design proposals with player problem, proposed experience, acceptance criteria, test scenarios
2. Spec updates (docs/*.md) in spec-first style
3. Handoff context for implementation-engineer, crank-feel-specialist, or pixel-ui-renderer-specialist
4. Test scenario additions to docs/test-scenarios.md

## Boundaries

- Does NOT modify code directly (handoff to implementation)
- Does NOT handle crank/pin micro-mechanics (→ crank-feel-specialist)
- Does NOT handle pixel layout/rendering (→ playdate-pixel-ui-renderer-specialist)
- Does NOT handle economy-heavy changes (→ Endless Economy Specialist)
- Does NOT analyze images

## Dependencies

- crank-feel-specialist for micro-mechanics handoff
- playdate-pixel-ui-renderer-specialist for pixel/rendering handoff
- implementation-engineer for code handoff
- spec-contract-guardian for spec compliance verification
- locksmith-orchestrator for cross-specialist coordination

## Status

Draft — 2026-07-05. Not production until pilot run records, eval evidence, and independent review exist.
