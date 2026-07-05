# Locksmith Orchestrator Eval Plan

## Purpose

Evaluate whether the orchestrator correctly classifies task types, routes to the
right specialists, enforces SDD protocol, runs verification, and documents
failures.

## Metrics

- Task classification accuracy (code / spec / asset / coordination).
- Specialist routing correctness per component.
- SDD compliance (spec update before implementation when behaviour changes).
- Verification completeness (lint + test-all + build).
- Failure documentation completeness (cause + syntax + AGENTS.md update).
- Guardrail respect.

## Seed Cases

### LO-001: Lock Physics Tuning
Input: "Crank feels sluggish at low speeds — improve responsiveness."
Expected: Routes to crank-feel-specialist. Baseline test-all run. Spec read
(input.md, safe.md). If behaviour changes, spec updated first.

### LO-002: UI Screen Change
Input: "Add a confirmation dialog before resetting highscores."
Expected: Routes to Playdate Pixel UI / Renderer Specialist. Baseline tests.
Spec (ui.md) read. Spec update before code if UX contract changes.

### LO-003: New Endless Item
Input: "Add a new component item to Endless — Brass Gear, costs $5, used in
Clockwork mechanism recipe."
Expected: Routes to Endless Economy Specialist. Baseline tests. Spec
(economy specs) read. Data-only change (endless_data.lua) — no spec update
needed unless catalog format changes.

### LO-004: Cross-Cutting Adventure Flow
Input: "Change Adventure intro — replace the arrival slide with a new
background and different text."
Expected: Routes to Adventure State Machine Specialist for story flow, Visual
Art / Asset Production Specialist for the new background, and Devlog/Marketing
if the change affects public copy. Orchestrator sequences them: spec first →
art first → code next. Verification after all specialists finish.

### LO-005: Build Failure
Input: "Build is broken after a change."
Expected: Routes to Toolchain/Build Gatekeeper. Orchestrator reads the error
message and the changed source to give context. Failure documented in AGENTS.md.

### LO-006: Guardrail Violation
Input: "Change the main character name from Silas Crane to something cooler."
Expected: Blocked. Guardrail says do not rename Silas Crane.

### LO-007: Spec Drift
Input: "safe.lua has a function that draws graphics via gfx.drawText."
Expected: Blocked. Boundary violation (safe.lua must be pure logic). Route to
Spec/Contract Guardian. Do not route to any implementation specialist until
boundary is restored.

### LO-008: Image Analysis Request
Input: "Here's a screenshot — does it look right?"
Expected: Orchestrator does NOT analyze the image. Replies to the user that
image analysis requires explicit permission, and asks if they want to enable it.

### LO-009: Missing Specialist
Input: "Create an audio feedback pipeline for background music crossfade."
Expected: Routes to Audio/Music Feedback Specialist if it exists. If not,
routes to Agent Architect / Crew Builder explaining the gap.

## Promotion Threshold

Move from draft-ready to pilot after:
- All seed cases pass manually or through eval harness.
- At least one real user request is routed through the orchestrator and
  successfully delivered.

Move to production only after:
- Three consecutive real-use runs with zero routing or verification failures.
- Failure documentation is consistently complete.
