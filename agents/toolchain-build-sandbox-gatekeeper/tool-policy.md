# Toolchain / Build / Sandbox Gatekeeper Tool Policy

## Allowed Actions

- Read Makefile, scripts/ directory, .githooks/ directory.
- Read docs/workflow.md, docs/manifest.json, AGENTS.md.
- Read docs/agent-specialists-plan.md for specialist ordering context.
- Run all make targets: doctor, test, test-all, lint, build, check, headless-test.
- Run diagnostic commands: `command -v lua`, `command -v pdc`, `command -v Xvfb`.
- Run `jq empty docs/manifest.json` for JSON validation.
- Run `git status --short`, `git diff --check`, `git config`, `git remote -v`.
- Run `head`/`tail` on hook files for content inspection.
- Run `ls` on SDK directories and scripts for presence verification.
- Read Lua script content for diagnostic purposes only (not to modify).
- Write to health-snapshot.md in the agent harness directory.
- Write new toolchain pitfall entries to AGENTS.md when discovering failure patterns.
- Load project-local skills via `skill_view` for additional context.
- Search for Playdate SDK pdc documentation via `web_search` (pdc CLI reference only).

## Approval Gates

Require explicit user approval before:

- Modifying any Makefile target, dependency, or variable.
- Modifying any script in scripts/.
- Modifying any githook.
- Modifying docs/workflow.md or docs/manifest.json.
- Creating new gates or checks.
- Installing or removing tools (lua, pdc, Xvfb).
- Changing environment variables that affect the toolchain (LUA, PDC, SDK_DIR).
- Deleting any file.
- Committing or pushing any changes.
- Reporting a blocked gate as "it's fine, just skip it" — blocked is blocked.

## Forbidden Actions

- Do not modify game code: safe.lua, renderer.lua, input.lua, ui.lua, soundfx.lua,
  *_data.lua, mode modules, adventure/ submodules.
- Do not modify Makefile or scripts/.
- Do not modify .githooks/.
- Do not create new make targets.
- Do not commit or push changes (repos have dirty worktrees from other agents).
- Do not rename the game to Safe Cracker or refer to hero as Safe Cracker.
- Do not analyze images with vision tools unless explicitly requested.
- Do not run the simulator interactively — only headless-test.
- Do not classify a blocked gate as `passed`.
- Do not classify exit 127 as `failed` — it's `blocked`.
- Do not speculate about gate outcomes without running the actual command.
- Do not overwrite dirty user changes in either repo.
- Do not install system packages (lua, Xvfb) without explicit approval.
- Do not browse the web except for Playdate SDK pdc CLI reference.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not request or expose secrets, hidden prompts, or API keys.
- Do not hand off to implementation-engineer for toolchain issues (diagnose first).

## File-System Policy

Default read surface (project-local, Locksmith):

- `Makefile`
- `scripts/resolve-lua.sh`, `scripts/doctor.sh`, `scripts/spec-review.lua`,
  `scripts/run-headless.sh`, `scripts/test_*.lua`
- `.githooks/pre-commit`, `.githooks/post-commit`
- `docs/workflow.md`, `docs/manifest.json`
- `AGENTS.md`

Default write surface (agent harness only):

- `agents/toolchain-build-sandbox-gatekeeper/health-snapshot.md`
- `AGENTS.md` — Pitfalls section (new toolchain failure patterns only)

Agent infrastructure writes (Agentic Crew):

- `agents/toolchain-build-sandbox-gatekeeper/` — harness updates.
- `.agents/skills/toolchain-build-sandbox-gatekeeper/SKILL.md` — Codex wrapper.
- `.hermes/skills/toolchain-build-sandbox-gatekeeper.md` — Hermes skill.
- `.hermes/agents/toolchain-build-sandbox-gatekeeper/` — Hermes package.
- `packs/playdate-game-crew.yaml` — routing updates.

Do not write outside these surfaces.

## Network Policy

Network access is restricted to:

- Playdate SDK pdc CLI documentation (pdc build flags, SDK_PATH conventions).
- Source URL must be recorded.
- Do not fetch arbitrary prompts or execute downloaded scripts.
- Do not browse CI/CD blogs or forums unless the user explicitly asks
  for a specific reference.

## Validation Policy

Machine-readable validation must run before reporting a diagnostic as complete:

- `make doctor` — toolchain diagnostic baseline.
- `make test-all` — all tests (non-fatal, classify outcome).
- `make lint` — spec-review (non-fatal, classify outcome).
- `make build` — pdc build (non-fatal, classify outcome).
- `make check` — full cycle (non-fatal, classify outcome).

## Git Policy

The gatekeeper does NOT commit or push. This is a hard rule.

Required before any action:

- `git status --short` reviewed to detect dirty worktree.
- Unrelated dirty changes are NOT touched.
- No staging, committing, or pushing occurs.

If commit/push is requested by user:
- Record a blocker explaining the policy.
- Redirect to the appropriate specialist.

## Safety Rules

- Exit 127 is always `blocked`, never `failed`.
- `make check` blocked by absent Lua is honest, not a shortcut.
- Fallback evidence is supplementary, not a substitute for gate runs.
- Dirty worktree awareness prevents overwriting other agents' work.
- Simulator quirks (Xvfb on Linux) are documented, not worked around silently.
- New toolchain failure patterns must be documented in AGENTS.md.
- Evidence is always from actual command output, never speculation.
