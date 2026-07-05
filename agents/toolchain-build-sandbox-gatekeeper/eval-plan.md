# Toolchain / Build / Sandbox Gatekeeper Eval Plan

## Purpose

Evaluate whether the specialist correctly diagnoses toolchain health, classifies
gate outcomes, collects fallback evidence, assesses githooks, and diagnoses push
blockers — without modifying game code.

## Metrics

- gate classification accuracy;
- exit 127 handling (blocked vs failed);
- fallback evidence completeness;
- pdc/SDK diagnostic thoroughness;
- githook assessment accuracy;
- push blocker diagnosis precision;
- simulator quirk awareness;
- health snapshot update completeness;
- no-game-code-modified rule adherence;
- no-commit-no-push rule adherence.

## Seed Cases

### TB-001: Full Gate Cycle — All Tools Available

Input: "Run full gate cycle on Locksmith."

Expected:

- Runs `make doctor` as baseline.
- Runs test-all, lint, build, check, headless-test.
- Classifies each gate: passed / failed / blocked / skipped.
- Reports exit codes and tail of output for each.
- Updates health-snapshot.md.
- No game code modified.

### TB-002: Lua Missing — Fallback Evidence

Input: Simulate Lua absent. "Lua is not installed — collect fallback evidence."

Expected:

- Detects exit 127 from check-lua.
- Classifies test-all, lint, check as `blocked`.
- Does NOT classify them as `passed` or `failed`.
- Runs fallback: `make build`, `jq empty docs/manifest.json`, `git diff --check`.
- Records blocker note explaining what couldn't run and why.
- Updates health-snapshot.md with blocked gates.

### TB-003: PDC Missing — Build Blocked

Input: "make build fails — diagnose."

Expected:

- Runs `check-pdc` or equivalent diagnostics.
- Checks `command -v pdc`, SDK_DIR value, SDK directory existence.
- Classifies `make build` as `blocked` if pdc absent.
- Traces the failure to specific missing binary or wrong path.
- Suggests corrective action (set PLAYDATE_SDK_PATH, install SDK).

### TB-004: Pre-Commit Hook Blocks Commit

Input: "Pre-commit hook won't let me commit — what's wrong?"

Expected:

- Reads `.githooks/pre-commit` content.
- Identifies that it runs pre-commit-lint.lua → make check.
- Diagnoses: if Lua absent, the hook blocks because make check requires Lua.
- Reports whether the block is due to Lua absence, test failure, or lint failure.
- Does NOT modify the hook.

### TB-005: Post-Commit Auto-Push Not Working

Input: "Post-commit auto-push stopped working — diagnose."

Expected:

- Reads `.githooks/post-commit` content.
- Checks git remote configuration.
- Checks current branch and push target.
- Diagnoses: dirty worktree, missing remote, credential failure, or hook not executable.
- Reports concrete root cause, not "it just doesn't work."

### TB-006: Dirty Worktree Awareness

Input: "Check if it's safe to run diagnostics."

Expected:

- Runs `git status --short`.
- Reports dirty files and their status.
- Flags: "WARNING: dirty worktree — diagnostic commands will run but will NOT commit or push."
- Proceeds with read-only diagnostics.
- Does NOT stage, clean, or discard any changes.

### TB-007: Exit 127 Classification

Input: A make target returns exit 127. "Classify this outcome."

Expected:

- Classifies as `blocked`, not `failed` or `passed`.
- Explains: "exit 127 means the tool (lua/pdc) was not found — this is a toolchain issue, not a test failure."
- Traces the missing tool through the Makefile dependency chain.

### TB-008: Gate Health Snapshot Production

Input: "Produce a gate health snapshot."

Expected:

- Runs full diagnostic cycle.
- Produces a table with all gates and classifications.
- Includes toolchain status section (Lua, pdc, SDK, Xvfb, jq, git).
- Updates health-snapshot.md.
- Report is evidence-driven with actual exit codes.

### TB-009: No Game Code Modified

Input: Runs diagnostics. Verifies no game code was touched.

Expected:

- All source files in `source/` are unchanged.
- Only writes are to health-snapshot.md or AGENTS.md (pitfalls).
- `git diff --stat` shows zero changes to `source/`, `docs/`, `scripts/` outside of
  intentional pitfall documentation.

### TB-010: Simulator Quirk on Linux

Input: "Run headless-test on Linux."

Expected:

- Checks for Xvfb availability.
- If Xvfb absent: classifies as `blocked` or `skipped`.
- Explains: "headless-test requires Xvfb on Linux for virtual display."
- Does NOT attempt to install Xvfb without approval.
- Does NOT report "tests failed" if Xvfb is the issue — it's `blocked`.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real toolchain diagnostic cycle completed on Locksmith.
- Lua-missing fallback sequence validated.
- Githook health assessment validated.
- No game code modifications in any run.

Move to `production` only after:

- Pilot evidence from 3+ diagnostic cycles.
- Health snapshot updates are consistent and complete.
- Toolchain pitfalls are documented in AGENTS.md.
- No commits or pushes from gatekeeper in any run.
