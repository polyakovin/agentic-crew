# Toolchain / Build / Sandbox Gatekeeper Workflow

## Boot Probe

Run for every toolchain diagnostic session:

```bash
git status --short
cd /root/locksmith && make doctor 2>&1 || true
cd /root/locksmith && make test-all 2>&1 | tail -10 || true
cd /root/locksmith && make build 2>&1 | tail -5 || true
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- `make doctor` output is the diagnostic baseline.
- `make test-all` exit 127 means Lua is absent → blocked gate, not test failure.
- `make build` exit 127 means pdc absent → blocked gate.
- All non-zero exits are evidence, not emergencies.

## Gate Classification

Every gate outcome MUST be classified into one of four categories:

| Classification | Meaning | Example |
|---------------|---------|---------|
| **passed** | Gate ran and succeeded (exit 0) | make build → Locksmith.pdx created |
| **failed** | Gate ran but tests/checks failed (non-zero exit, but tool present) | make test-all → 5 tests FAIL |
| **blocked** | Gate cannot run because a required tool is missing | make lint → exit 127, lua not found |
| **skipped** | Gate not relevant to current context | headless-test skipped: no Xvfb on this system |

**Critical rule**: exit 127 (`command not found`) is ALWAYS `blocked`, NEVER `failed`.
A missing tool is not a test failure. A test failure is a logic/behaviour issue.

## Core Workflow

1. **Environment preflight**: run `make doctor` → assess toolchain state.
2. **Run gates**: test-all, lint, build, check (each with `|| true` to avoid
   cascading blocks).
3. **Classify each gate**: passed / failed / blocked / skipped.
4. **If Lua absent**: execute Lua-missing fallback sequence.
5. **If pdc/SDK absent**: execute pdc/SDK diagnostic.
6. **Check githooks**: pre-commit and post-commit presence and content.
7. **Diagnose push blockers**: dirty worktree, credentials, branch policy.
8. **Update health snapshot**: write findings to health-snapshot.md.
9. **Report**: concise classification table with evidence.

## Full Gate Cycle

```bash
# From Locksmith root
cd /root/locksmith

# 1. Diagnostic baseline (always runs, non-mutating)
make doctor 2>&1; echo "DOCTOR_EXIT=$?"

# 2. Tests (non-fatal — failures are evidence)
make test-all 2>&1 | tail -10; echo "TEST_ALL_EXIT=$?"

# 3. Lint (non-fatal)
make lint 2>&1 | tail -5; echo "LINT_EXIT=$?"

# 4. Build (non-fatal)
make build 2>&1 | tail -5; echo "BUILD_EXIT=$?"

# 5. Full check (non-fatal)
make check 2>&1 | tail -5; echo "CHECK_EXIT=$?"

# 6. Headless test (may be blocked by Xvfb)
make headless-test 2>&1 | tail -5; echo "HEADLESS_EXIT=$?"
```

After the cycle, classify each gate and produce the report.

## Lua-Missing Fallback Sequence

When `check-lua` returns exit 127 (Lua not found), `make test-all`, `make lint`,
and `make check` are all **blocked**. Collect fallback evidence:

```bash
# 1. Build — verifies the project compiles even without Lua tests
cd /root/locksmith && make build 2>&1 | tail -5
echo "BUILD_WITHOUT_LUA_EXIT=$?"

# 2. JSON validity — verifies manifest is well-formed
jq empty docs/manifest.json 2>&1
echo "JQ_EXIT=$?"

# 3. Whitespace check — basic coding style validation
git diff --check 2>&1 | head -20
echo "GIT_DIFF_CHECK_EXIT=$?"

# 4. Blocked gate note
echo "BLOCKED: Lua runtime not found. The following gates could not run:"
echo "  - make test-all (requires Lua)"
echo "  - make lint (requires Lua)"
echo "  - make check (requires Lua for test-all + lint)"
echo "Fallback evidence collected: build, jq, git diff --check"
```

**Do NOT** report test-all/lint/check as `passed` when Lua is absent.
They are **blocked**, with fallback evidence provided.

## PDC / SDK Diagnostic

When `check-pdc` returns exit 127 or `make build` fails:

```bash
# Check pdc binary
command -v pdc 2>&1 || echo "pdc not in PATH"
ls -la ~/PlaydateSDK/bin/pdc 2>&1 || echo "pdc not at ~/PlaydateSDK/bin/pdc"

# Check SDK_DIR
echo "PLAYDATE_SDK_PATH=${PLAYDATE_SDK_PATH:-not set}"
echo "SDK_DIR from Makefile:"
cd /root/locksmith && make -p 2>/dev/null | grep -E "^SDK_DIR" || echo "not resolved"

# Check SDK directory exists
ls -d ~/PlaydateSDK 2>&1 || echo "SDK directory not found"
```

Classify outcome:
- pdc in PATH → SDK available → `make build` may work.
- pdc not found, SDK_DIR wrong → `blocked`, recommend fixing SDK_PATH.
- SDK directory absent → `blocked`, recommend installing Playdate SDK.

## Githook Health Check

```bash
cd /root/locksmith

# Check hook presence
ls -la .githooks/pre-commit 2>&1
ls -la .githooks/post-commit 2>&1

# Check hook executability
test -x .githooks/pre-commit && echo "pre-commit: executable" || echo "pre-commit: NOT executable"
test -x .githooks/post-commit && echo "post-commit: executable" || echo "post-commit: NOT executable"

# Check hooksPath config
git config core.hooksPath 2>&1

# Show hook content (first 15 lines for diagnostics)
head -15 .githooks/pre-commit 2>&1
head -15 .githooks/post-commit 2>&1
```

Assess:
- pre-commit runs pre-commit-lint.lua → make check → blocks without Lua.
- post-commit runs auto-push → may fail with dirty worktree or missing credentials.
- hooksPath points to .githooks → confirmed active.

## Push Blocker Diagnosis

When push is blocked:

```bash
# Check dirty worktree
git status --short 2>&1

# Check remote
git remote -v 2>&1

# Check current branch
git branch --show-current 2>&1

# Attempt push (dry-run if possible, or record the actual error)
git push --dry-run 2>&1 || true
```

Classify blocker:
- Dirty worktree → `blocked` by unrelated changes.
- No remote configured → `blocked`, needs `git remote add`.
- Authentication failure → `blocked`, needs credentials.
- Branch protection → `blocked`, needs policy change or different branch.

## Health Snapshot Update

After diagnostics, update `health-snapshot.md` in the agent directory:

```markdown
## Gate Health (YYYY-MM-DD)

| Gate | Outcome | Detail |
|------|---------|--------|
| make doctor | passed | Lua: found, pdc: found, SDK: /path |
| make test-all | blocked | Lua absent, exit 127 |
| make lint | blocked | Lua absent, exit 127 |
| make build | passed | Locksmith.pdx created |
| make check | blocked | depends on test-all + lint (both blocked) |
| headless-test | skipped | Xvfb not available on this Linux host |
| pre-commit hook | present | blocks without Lua (runs make check) |
| post-commit hook | present | auto-push configured |
```

Update:
- Flip checkboxes from `[ ]` to `[x]` for verified items.
- Advance status from `draft` towards `pilot` as evidence accumulates.
- Document any new toolchain failure patterns in AGENTS.md.

## Simulator Quirks

```bash
# Check Xvfb availability
command -v Xvfb 2>&1 || echo "Xvfb not installed — headless-test requires it on Linux"

# Check if headless-test can run
cd /root/locksmith && bash scripts/run-headless.sh 2>&1 | head -10 || true
```

Known quirks:
- `make headless-test` requires Xvfb on Linux (provides virtual display).
- macOS doesn't need Xvfb — simulator uses native display.
- Without Xvfb: classify as `skipped` or `blocked` depending on context.
- Simulator itself may not be installed on Linux (Playdate SDK is macOS-primary).

## Pitfalls

1. **Exit 127 is blocked, not failed.** `command not found` means the tool is
   missing, not that tests failed. Every exit 127 must be classified as `blocked`.

2. **Do not report blocked as passed.** If Lua is absent and `make check` returns
   exit 127, it is NOT "make check passed." It is "make check blocked — Lua
   not installed."

3. **Fallback evidence is NOT a substitute for real gate runs.** `make build` +
   `jq` + `git diff --check` tells you the project compiles and JSON is valid,
   but it does NOT tell you tests pass. Always note this limitation.

4. **Dirty worktree is not your problem to fix.** The gatekeeper diagnoses
   dirty state, does not clean it. Overwriting unrelated user changes is a
   critical failure.

5. **Never commit or push from diagnostics.** The repos have pre-existing
   dirty worktrees from other agents. Committing or pushing would bundle
   unrelated changes. Gatekeeper is evidence-only.

6. **`make check` depends on test-all + lint.** If Lua is absent, both
   test-all and lint are blocked → check is automatically blocked.
   Don't try to run check with `-k` flags — report the dependency chain.

7. **Simulator on Linux is always tricky.** `make headless-test` expects
   Xvfb. If Xvfb is absent, headless-test is `blocked` or `skipped`,
   depending on whether the user needs it.

8. **Post-commit auto-push may fire.** If a commit happens (by another agent)
   and post-commit hook runs, it auto-pushes. The gatekeeper must NOT commit,
   so this is not a risk from diagnostic work — but note it when assessing
   hook health.

## Commit And Push Gate

**The gatekeeper does NOT commit or push.** This specialist is evidence-only.

If asked to commit or push:
- Record a blocker: "commit/push requested but gatekeeper is evidence-only."
- Redirect to implementation-engineer or orchestrator for code changes.
- Do not commit diagnostic reports (health-snapshot.md updates are writes
  to the agent harness directory, not git commits).

Reason: both repos (`/root/agentic-crew` and `/root/locksmith`) have
pre-existing dirty worktrees from other agents' incomplete work. Committing
would bundle unrelated changes.
