# Narrative / Noir Copy Specialist Workflow

## Boot Probe

Run for non-trivial copy audits or text changes:

```bash
git status --short
cd /root/locksmith && make test-all 2>&1 | tail -5
cd /root/locksmith && make lint 2>&1 | tail -5
cd /root/locksmith && make build 2>&1 | tail -5
```

Interpretation:

- Dirty worktrees require care. Do not overwrite unrelated user changes.
- Failing `make test-all` before changes means fix first, then change copy.
- Missing `pdc` means blocked build gate — cannot verify rendering impacts.

## Core Workflow

1. **Read project context**: AGENTS.md → `docs/lore.md` → `docs/ui.md` →
   `docs/manifest.json`.
2. **Read affected text sources**: `source/endless_data.lua`,
   `source/economy_catalogs.lua`, `source/ui.lua`, `source/endless_mode.lua`.
3. **Formulate copy problem**: what is wrong — tone, length, consistency,
   clarity? State the problem in one sentence.
4. **Propose replacement**: old text → new text with justification.
5. **Check Playdate fit**: ASCII-only? Fits 400×240 width with
   Fonts.drawTextAligned and opaque background?
6. **Update spec first**: if terminology, naming convention, or lore changes,
   update `docs/lore.md` or relevant spec before changing data files.
7. **Implement**: change text in the relevant `*_data.lua`, `*_catalogs.lua`,
   or `ui.lua`.
8. **Verify**: `make test-all` → `make lint` → `make build` → `make check`.
9. **Report**: write concise report with canon check results, tone
   justification, length fit, affected files, and build evidence.

## Copy Change Template

When proposing or making a copy change, use this template:

```
Copy Problem: [what is wrong — tone, length, consistency, clarity]

Changes:
  [text_location]:
    file: [source_file]
    section: [SHOP_ITEMS / COMPONENT_ITEMS / ACHIEVEMENTS / LOCK_CATALOG / etc.]
    field: [name / purpose / loreLine / label]
    old: "[old text]"
    new: "[new text]"
    rationale: [why this tone is better, why length fits, why canon is aligned]

Canon check:
  - [related string in another file]: [consistent / inconsistent → fixed]
  - [character/place reference]: [aligned / drift → aligned]

Playdate fit:
  - ASCII-only: [yes / no → fixed]
  - Estimated width: [N pixels of 400] — [fits / borderline / overflows]
  - Opaque background: [sufficient / not checked]

Specs updated:
  [spec_file]: [description of change]

Tests affected:
  [test_file]: [passed / failed → fixed / test updated]
```

## Text Source Audit

Before any copy change, run a boundary audit to verify text sources:

```bash
# Check *_data.lua and *_catalogs.lua for gfx/playdate calls (should have NONE)
rg -n "gfx\.\|playdate\." source/endless_data.lua source/economy_catalogs.lua && echo "VIOLATION: data files have gfx/playdate" || echo "OK"

# Check *_data.lua and *_catalogs.lua for runtime state (should have NONE)
rg -n "playdate\.\|persist\|save\|lifecycle\|runtime" source/endless_data.lua source/economy_catalogs.lua && echo "VIOLATION: data files have runtime state" || echo "OK"

# Check for Unicode/emoji in text strings (should have NONE)
rg -n "[^\x00-\x7F]" source/endless_data.lua source/economy_catalogs.lua source/ui.lua source/endless_mode.lua && echo "VIOLATION: non-ASCII chars detected" || echo "OK"
```

## Canon Consistency Check

Run a cross-file consistency audit:

```bash
# Check that "Gaslight Bazaar" is spelled consistently everywhere
rg -n "Gaslight Bazaar\|Gaslight\|Bazaar" source/ docs/lore.md

# Check that "Silas Crane" / "Locksmith" are not "Safe Cracker"
rg -n "Safe Cracker\|Safe-Cracker\|safe cracker" source/ docs/ && echo "VIOLATION: Safe Cracker found" || echo "OK"

# Check that "Ezra" and "Margo" are consistently named
rg -n "Ezra\|Margo" source/ docs/lore.md
```

## Validation Pack

Run from Locksmith root:

```bash
make test-all && echo "Tests: PASS" || echo "Tests: FAIL"
make lint && echo "Lint: PASS" || echo "Lint: FAIL"
make build && echo "Build: PASS" || echo "Build: FAIL"
# Full cycle:
make check && echo "Check: PASS" || echo "Check: FAIL"
```

If tests fail after a text change:
1. Check that the test is testing a string value — update the test if the
   string change is intentional.
2. Ensure declarative data tables are syntactically correct.
3. Never change tests to "make them pass" without understanding why they broke.

## Pitfalls

1. **Unicode contamination**: copying text from a browser or word processor
   often brings smart quotes (U+2018/U+2019), em-dashes (U+2014), or accented
   characters. Always paste as plaintext and check with `[^\x00-\x7F]`.

2. **Width underestimation**: Playdate font is mono but not every character is
   the same width. Upper-case letters are wider. Test on Simulator.

3. **Canon drift via copy-paste**: when adding a new item, copying an existing
   item as a template and changing the name but not the loreLine creates
   "identical lore" bugs.

4. **Marketing tone leak**: phrases like "Collect them all!", "Try your luck!",
   "Rare item!" belong on itch.io, not in in-game text.

5. **Over-flavouring**: adding noir atmosphere is good; adding five sentences
   of backstory to a tool purpose field is not. One line, one image.

6. **Voice inconsistency**: Silas Crane is terse and weary — if a loreLine
   sounds like it was written by a cheerful announcer, it's wrong.

7. **Test breakage from string changes**: some tests may check exact string
   values. Review test output carefully when changing text.

8. **Spec-last changes**: changing a character name in `endless_data.lua`
   without updating `docs/lore.md` creates canon drift. Always spec-first.

## Commit And Push Gate

After validation succeeds:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped copy-specialist changes.
3. Commit with atomic message: `"feat(copy): <copy problem summary>"`
4. Push the current branch.
5. Record commit SHA and push result.

Block instead of committing when:

- unrelated dirty files would be staged;
- validation failed or did not run;
- credentials or branch policy prevent push.
