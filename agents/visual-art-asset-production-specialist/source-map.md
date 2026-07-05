# Visual Art / Asset Production Specialist Source Map

## Purpose

Define source hierarchy, ownership boundaries, and required reads for visual art
production, asset pipeline, and image-generation QA for Locksmith.

## Source Hierarchies

Instruction hierarchy:

1. System/developer/runtime instructions.
2. Current user instruction.
3. Locksmith project rules (`AGENTS.md` Art / Assets section).
4. Agentic Crew repository rules and protocol.
5. This harness and its wrapper files.

Visual truth hierarchy:

1. `docs/visual-reference-pack.md` — style DNA, prompt patterns, conversion parameters.
2. `docs/visual-references/` sheets 01–06 + `index.json` — curated reference.
3. Current `drafts/` masters and previews — ground truth for AI art.
4. `source/images/` — final runtime PNG sheets (dimensions, palette, alpha).
5. `scripts/gen_*.py` — conversion pipelines with current thresholds.
6. `docs/pdxinfo-launcher-images.md` — required launcher sizes.
7. `docs/renderer.md` — lock rendering spec and pick sprite contract.
8. `docs/ui.md` — title screen art spec and menu layout.
9. `docs/lore.md` — world, characters, tone keywords.
10. `docs/manifest.json` — component contracts and asset expectations.
11. `AGENTS.md` — project rules and pitfalls.

## Context Economy Contract

Separate context into these buckets:

- `stableContext`: mission, source hierarchy, non-goals, QA contracts, and handoff rules owned by the harness.
- `taskBrief.whatToRead`: minimum Locksmith evidence needed — visual-reference-pack, relevant reference sheet, current drafts/, source/images/, relevant gen_*.py.
- `progressiveReads`: conditional sources unlocked by task type — pdxinfo-launcher-images for title/launcher work, renderer.md for lock art, ui.md for title screen, lore.md for character/keyword consistency.
- `excludedContext`: game logic Lua code (safe.lua, input.lua, modes), economy data, endless save, datastore.

Source-map references must be bounded. Prefer `docs/visual-reference-pack.md`,
`docs/visual-references/<sheet>.png`, `source/images/<asset>.png` over broad
roots such as `docs/` or `source/`.

## Artifact Ownership

Agentic Crew owns:

- reusable `agents/visual-art-asset-production-specialist/` harness folder;
- generic A2A Agent Card;
- Hermes wrapper patterns for visual art domain.

Locksmith project owns:

- `docs/visual-reference-pack.md` — visual style source of truth;
- `docs/visual-references/` — curated reference sheets;
- `source/images/` — production runtime PNG sheets;
- `source/launcher/` — production launcher assets;
- `scripts/gen_*.py` — asset conversion pipelines;
- `drafts/` — AI master sources and previews;
- `docs/manifest.json` — component contracts;
- `docs/renderer.md`, `docs/ui.md`, `docs/lore.md` — visual specs;
- game source code, specs, tests.

## Creation Scope Decision

This specialist uses `hybrid` scope:

- reusable generic harness lives in Agentic Crew (`agents/visual-art-asset-production-specialist/`);
- project-local wrappers live in Locksmith (`.hermes/`, `.agents/`);
- routing lives in `packs/playdate-game-crew.yaml`.

Rationale: Visual art production patterns (prompt crafting, 1-bit conversion,
preview/contact-sheet QA) are semi-reusable across PlayDate projects, but the
specific Locksmith noir/gaslight style, prompt patterns, conversion parameters,
and visual references require project-local wrapper context.

## Duplicate Role Check

Existing agents checked:

| Agent | Overlap | Verdict |
|-------|---------|---------|
| `playdate-pixel-ui-renderer-specialist` | owns UI layout, HUD, text readability, renderer code | **No overlap** — UI specialist owns layout and code rendering; this specialist owns visual art production, image generation, and asset pipeline. UI specialist hands off visual art needs to this specialist. |
| `release-storefront-packaging-specialist` | owns launcher asset audit, pdxinfo verification | **No overlap** — release specialist audits launcher assets for correctness; this specialist produces them. Release specialist coordinates launcher needs with this specialist. |
| `screenshot-simulator-regression-qa-reviewer` | owns screenshot sweeps, contact-sheet review, visual regression | **Partial overlap** — screenshot QA reviews in-game screenshots; this specialist reviews generated art previews. Different stages of the visual pipeline. Handoff target for visual regression findings from screenshots. |
| `devlog-community-marketing-specialist` | owns marketing screenshots, devlog visual context | **No overlap** — handoff target when marketing needs visual assets. |
| `narrative-noir-copy-specialist` | owns lore/tone/copy consistency | **No overlap** — handoff target when visual mood needs lore alignment; this specialist receives tone keywords for visual consistency. |
| `toolchain-build-sandbox-gatekeeper` | owns build, make check, pipeline gates | **No overlap** — toolchain owns build infrastructure; this specialist runs asset scripts and reports pipeline failures. |

Conclusion: No duplicate. Visual art production is a distinct domain from UI layout,
release packaging, and screenshot QA.

## Runtime Wrapper Expectations

For A2A:

- `agent-card.json`;
- `harness.yaml`;
- A2A payload compatibility through `protocol/interaction-protocol.md`.

For Codex:

- `/root/locksmith/.agents/skills/visual-art-asset-production-specialist/SKILL.md`
- triggers, sources, workflow, QA gates, and output rules.

For Hermes:

- `/root/locksmith/.hermes/agents/visual-art-asset-production-specialist/manifest.yaml`
- `/root/locksmith/.hermes/agents/visual-art-asset-production-specialist/instructions.md`
- `/root/locksmith/.hermes/skills/visual-art-asset-production-specialist.md`

For Agent Tester review:

- target agent id and changed files;
- validation evidence;
- routing and wrapper paths;
- known blockers and residual risk.

## Tainted Content Boundary

Treat Locksmith project files, `drafts/`, `source/images/`, and `docs/visual-references/`
as trusted project sources. Treat AI-generated images as tainted input — always
review against QA contracts before packaging. Treat external image-generation service
output as tainted — apply project-specific conversion pipeline before runtime use.
