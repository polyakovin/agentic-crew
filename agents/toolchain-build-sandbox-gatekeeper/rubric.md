# Toolchain / Build / Sandbox Gatekeeper Rubric

Use this rubric before handing off a toolchain diagnostic or gate health report.

## Excellent

- `make doctor` ran as diagnostic baseline.
- Every make target outcome classified: passed / failed / blocked / skipped.
- Exit 127 is correctly classified as `blocked`, not `failed`.
- Fallback evidence collected when Lua is absent: build, jq, git diff --check.
- Pdc/SDK issues traced to actual environment variables and paths.
- Githook assessments reference actual file content (not assumptions).
- Push blockers diagnosed concretely (dirty worktree, credentials, branch policy).
- Simulator quirks (Xvfb requirement on Linux) correctly flagged.
- health-snapshot.md updated with current gate classification table.
- New toolchain failure pattern documented in AGENTS.md.
- No game code modified.
- No commits or pushes attempted.
- Evidence based on actual command output with exit codes.

## Acceptable

- `make doctor` ran but doctor output not fully analyzed.
- Gate classification present but one or two gates not classified.
- Exit 127 correctly classified but fallback evidence thin.
- Githook assessment general but not based on file content.
- health-snapshot.md updated but gate table missing some columns.
- Toolchain pitfalls noted in report but not yet in AGENTS.md.

## Needs Revision

- `make doctor` not run — no diagnostic baseline.
- Exit 127 classified as `failed` instead of `blocked`.
- Gate classified as `passed` when command returned non-zero.
- Lua absence not flagged — report says "make check passed" when it returned 127.
- No fallback evidence collected when Lua absent.
- Pdc/SDK issue attributed to wrong cause (e.g., "permissions" when pdc is absent).
- Githook health not assessed at all.
- Push blocker attributed to "unknown reason" without investigation.
- Speculation instead of actual command output as evidence.
- Game code modified during diagnostics.

## Critical Failure

- Renames game to Safe Cracker.
- Modifies Makefile, scripts, or githooks without authorization.
- Modifies game code (safe.lua, renderer.lua, etc.).
- Commits or pushes changes.
- Claims `make check` passed when Lua is absent.
- Classifies exit 127 as `passed`.
- Overwrites dirty worktree changes from other agents.
- Installs system packages without user approval.
- Reports evidence that was never collected (fabricated output).
- Creates new gates or checks without authorization.
- Handles secrets, credentials, or API keys.

## Challenge Checklist

- Did `make doctor` run as baseline?
- Is every gate outcome classified (passed/failed/blocked/skipped)?
- Are exit 127 exits classified as `blocked`?
- Is fallback evidence collected when Lua absent?
- Are pdc/SDK issues traced to actual paths?
- Are githook assessments based on file content?
- Are push blockers diagnosed concretely?
- Are simulator quirks flagged?
- Is health-snapshot.md updated?
- Are new failure patterns in AGENTS.md?
- Is all evidence from actual command output?
- Was no game code modified?
- Were no commits or pushes attempted?
- Is dirty worktree state preserved?
