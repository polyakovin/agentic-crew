# Release / Storefront Packaging Specialist Workflow

## Boot Probe

Run before any release audit or packaging task:

```bash
git status --short
cd /root/locksmith && make check 2>&1 | tail -10 || true
ls source/launcher/ source/pdxinfo 2>&1
cat source/pdxinfo
```

Interpretation:

- Dirty worktrees: note but do not block packaging audit (we don't modify code).
- `make check` failures: document which components fail; if the failing component is the one being packaged, flag as blocker.
- Missing launcher assets: flag as release blocker.
- Incorrect pdxinfo: flag as release blocker.

## Core Workflow

1. Read `AGENTS.md` → `README.md` → `TODO.md` → relevant docs.
2. Verify current pdxinfo metadata against expected values.
3. Audit launcher assets: file presence, sizes, animation.txt references.
4. Run `make check` for build evidence.
5. Check git log for changes since last release.
6. Generate release notes if requested — per `devlog/template.md`.
7. Handoff launcher art needs to Visual Art / Asset Production Specialist.
8. Handoff copy canon issues to Narrative / Noir Copy Specialist.
9. Compile release blockers list with ownership.
10. Execute prototype removal checklist if status promotion is planned.
11. Draft itch.io page updates — do NOT publish.
12. Produce packaging report with evidence and verdict.

## Release Audit Checklist

Run this checklist for every release:

### pdxinfo Verification

```bash
cat source/pdxinfo
```

Check:
- [ ] `name=Locksmith` (not Safe Cracker).
- [ ] `bundleID=com.polyakovin.locksmith`.
- [ ] `version` matches planned release version.
- [ ] `author=@polyakovin`.
- [ ] `imagePath=launcher`.

### Launcher Asset Audit

```bash
ls -la source/launcher/
file source/launcher/card.png source/launcher/icon.png source/launcher/launch-image.png
ls source/launcher/card-highlighted/
cat source/launcher/card-highlighted/animation.txt 2>/dev/null || echo "MISSING"
```

Check:
- [ ] `card.png` exists, 350×155.
- [ ] `icon.png` exists, 32×32.
- [ ] `launch-image.png` exists, 400×240.
- [ ] `card-highlighted/` directory exists with at least 2 animation frames.
- [ ] `animation.txt` exists and references all frames.

### Build Evidence

```bash
cd /root/locksmith && make check 2>&1 | tail -20
```

Check:
- [ ] Build succeeds or known failures documented.
- [ ] Test results recorded.
- [ ] Lint passes or known failures documented.

### Release Notes Generation

When release notes are requested:

1. Get git log: `git log --oneline -30 --no-merges`.
2. Read `TODO.md` for completed items.
3. Read `devlog/template.md` for format.
4. Filter to gameplay/UX changes only.
5. Formulate 1-3 most interesting changes.
6. Cross-reference claims against `docs/game.md` and build evidence.
7. Draft per template.
8. Handoff canon/tone issues to Narrative Specialist.

## Prototype Removal Checklist

Execute when status changes from Prototype to Released:

1. Verify game is named Locksmith in all metadata.
2. Draft itch.io page update: title, description, tags.
3. Document legacy URL (`polyakovin.itch.io/safe-cracker`) with migration message.
4. Verify launcher assets are production-quality (not prototype placeholders).
5. Verify release notes cover the transition.
6. Compile all release blockers — all must be resolved.
7. Present full checklist to user for explicit approval.
8. Do NOT change status without user approval.

## Itch.io Page Readiness Audit

When auditing itch.io page:

1. Check game title is "Locksmith".
2. Check description references Locksmith and Silas Crane, not Safe Cracker.
3. Check tags are appropriate: PlayDate, puzzle, lockpicking, noir, incremental.
4. Check screenshots are current and captioned.
5. Check status: Prototype or Released.
6. Document legacy URL status.
7. Draft any needed updates — user approves and posts.

## Handoff Rules

When launcher art needs generation or update:

- Handoff to Visual Art / Asset Production Specialist with:
  - Required asset type and size (from `docs/pdxinfo-launcher-images.md`).
  - Style reference (from `docs/visual-reference-pack.md`).
  - Current state and what needs changing.
  - Deadline context.

When release notes touch in-game canon:

- Handoff to Narrative / Noir Copy Specialist with:
  - Draft text that needs canon review.
  - Which in-game elements are referenced (character names, lore terms).
  - Required reading: `docs/lore.md`.

When build infrastructure fails:

- Handoff to `toolchain-build-sandbox-gatekeeper` with:
  - make check output.
  - Which component is failing.
  - Whether this blocks release.

## Validation Pack

Run from the Agentic Crew repository root:

```bash
python3 -m json.tool agents/release-storefront-packaging-specialist/agent-card.json
python3 -m json.tool agents/release-storefront-packaging-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/release-storefront-packaging-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/release-storefront-packaging-specialist
```

Also verify from Locksmith root when Hermes wrappers exist:

```bash
cd /root/locksmith && python3 -c "import yaml; yaml.safe_load(open('.hermes/agents/release-storefront-packaging-specialist/manifest.yaml'))"
```

`rg` exit code 1 for trailing-whitespace search means no matches and is a pass.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state in each affected repository.
2. Stage only scoped changes.
3. Commit with an atomic message that names the agent.
4. Push the current branch.
5. Record commit SHA, branch, remote, and push result.

Block instead of committing when validation failed or unrelated dirty files would be staged.
