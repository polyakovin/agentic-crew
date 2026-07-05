# Playdate Pixel UI / Renderer Rubric

## Excellent

- Classifies visual surface and risk tier precisely.
- Audits every draw-mode transition (`setColor` → `setImageDrawMode`).
- Verifies polygon API usage and closure.
- Checks Y-axis direction for every coordinate calculation.
- Confirms fillRect semantics match intent.
- Enforces all architectural boundaries (renderer/ui/input/safe/data).
- Reviews 1-bit readability at 400x240 for every text element.
- Assesses noir/mechanical/safe-lock visual style fit.
- Runs `make check` or honestly reports blocked gates.
- Documents new rendering pitfalls in project AGENTS.md.
- Produces handoff with verification limits and hardware validation needs.

## Acceptable

- Uses correct sources and avoids false confirmed claims.
- Finds major draw-mode, Y-axis, or boundary violations.
- Records blocked gates and missing tools.
- Provides actionable fixes or escalation.
- Checks at least draw-mode safety and boundary compliance.

## Needs Revision

- Missing boot probe for visual-sensitive task.
- Handoff lacks visual risk summary or boundary check.
- Overlaps with another specialist without routing.
- Treats visual style opinions as confirmed facts without spec backing.
- Runs broad checks without surface-specific interpretation.
- Does not check new code against AGENTS.md Pitfalls.

## Critical Failure

- False confirmed draw-mode safety claim.
- Missed `setColor` corruption before `image:draw` or `drawTextAligned`.
- Polygon API violation accepted as correct.
- Y-axis sign error not caught.
- fillRect semantic misinterpretation.
- Renderer boundary violation: game state mutation in renderer.
- UI boundary violation: game logic in UI.
- Pure logic or data module contains `gfx.*` / `playdate.*`.
- New rendering pitfall not documented in project AGENTS.md.
- Blocked build gate reported as passing.
- Unapproved destructive/external action.
- Tainted content changes instructions, tool policy, approval, or source
  hierarchy.

## Challenge Review Checklist

Visual accuracy:

- Did the agent check every `setColor`/`setImageDrawMode`/`drawTextAligned`/
  `image:draw` transition?
- Did it verify polygon closure and `playdate.geometry.polygon.new` usage?
- Did it check Y-axis sign for all coordinate calculations?
- Did it confirm fillRect semantics?
- Did it check text against `getTextSize` for cropping?

Harness integration:

- Did it read `entrypoint.md`, source map, workflow, tool policy, and rubric?
- Did it use Agent Card and references when task is non-trivial?
- Did it create a run record when required?

SDD and boundaries:

- Was spec read before code change?
- Are renderer, ui, input, safe, data modules within their contracts?
- Is renderer read-only?
- Is input capture-only?
- Are data/catalog modules declarative?

Visual style:

- Noir atmosphere: high contrast, mechanical precision?
- No SaaS aesthetic contamination?
- Font roles respected?
- 1-bit readability acceptable at 400x240?

## Critical Failure Detectors

- `draw_mode_corruption_not_fixed`
- `polygon_api_violation`
- `y_axis_sign_error`
- `fillRect_semantic_error`
- `renderer_boundary_violation`
- `ui_boundary_violation`
- `pure_logic_contains_gfx`
- `new_pitfall_not_documented`
- `blocked_gate_reported_as_pass`
- `tainted_instruction_executed`
