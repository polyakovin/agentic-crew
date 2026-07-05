# Source Map — Gameplay Design / Player Experience Specialist

## Locksmith project sources this specialist reads (read-only)

| Source | Purpose |
|--------|---------|
| `AGENTS.md` | Project rules, architecture, pitfalls, Y-axis rule |
| `docs/manifest.json` | Component contracts and dependencies |
| `docs/game.md` | Game mode architecture, core loop spec |
| `docs/safe.md` | Pin tumbler physics, tolerance, difficulty constants |
| `docs/input.md` | Input capture shape (crank, D-pad, A/B) |
| `docs/ui.md` | Screen layout, UI components |
| `docs/renderer.md` | Drawing layers, lock anatomy |
| `docs/test-scenarios.md` | Test scenarios cross-reference |
| `docs/lore.md` | World, character, tone |
| `docs/economy-roadmap.md` | Economy design for progression context |
| `source/safe.lua` | Current physics code (context only) |
| `source/input.lua` | Current input code (context only) |

## Agentic Crew harness files

| File | Role |
|------|------|
| `agent-card.json` | A2A agent description |
| `role.md` | Full role definition and system prompt |
| `entrypoint.md` | Activation and work instructions |
| `source-map.md` | This file — source directory |
| `workflow.md` | Step-by-step task workflow |
| `harness.yaml` | Harness metadata |
| `README.md` | Overview and usage |
| `rubric.md` | Quality rubric |
| `eval-plan.md` | Evaluation plan |
| `run-record.template.json` | Run record template |
| `release-rollback.md` | Release and rollback notes |
| `tool-policy.md` | Tool use policy |
| `health-snapshot.md` | Health and status checks |

## Project-local wrappers

| File | Location | Role |
|------|----------|------|
| Codex SKILL.md | `/root/locksmith/.agents/skills/gameplay-design-player-experience-specialist/SKILL.md` | Codex runtime skill |
| Hermes skill | `/root/locksmith/.hermes/skills/gameplay-design-player-experience-specialist.md` | Hermes skill definition |
| Hermes package | `/root/locksmith/.hermes/agents/gameplay-design-player-experience-specialist/` | Hermes agent package |

## Conventions

- All paths are relative to `/root/agentic-crew` unless prefixed with `/root/locksmith/`.
- Locksmith project is at `/root/locksmith`.
- Agentic Crew repository is at `/root/agentic-crew`.
