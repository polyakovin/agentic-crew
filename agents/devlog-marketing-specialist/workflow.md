# Devlog / Community Marketing Specialist Workflow

## Boot Probe

Run before drafting any devlog post or release notes:

```bash
git status --short
cd /root/locksmith && make check 2>&1 | tail -10 || true
git log --oneline -20 --no-merges
```

Interpretation:

- Dirty worktrees: note but don't block — marketing copy doesn't modify code.
- Failing `make check`: flag but don't block unless the failing feature is
  the one being described.
- Short git log: may mean no recent player-facing changes to report.

## Core Workflow

1. **Read project context**: AGENTS.md → README.md → TODO.md →
   `devlog/template.md`.
2. **Check game state**: `git log --oneline -N --no-merges` for recent
   changes.
3. **Filter changes**: leave only gameplay/UX, discard:
   - refactors ("cleanup", "restructure", "rename internal")
   - test/lint/build changes
   - dependency bumps
   - CI/config changes
   - docs-only changes (unless docs describe new gameplay)
   - art/asset pipeline changes (defer to Visual Art Specialist)
4. **Read relevant specs**: for each filtered gameplay change, read the
   corresponding `docs/*.md` or `docs/game.md` section.
5. **Formulate post angle**: select 1-3 most interesting player-facing
   changes. For each, answer "что это даёт игроку?"
6. **Write draft** per `devlog/template.md` format.
7. **Verify claims**: cross-reference each gameplay claim against:
   - git log (change actually committed)
   - spec doc (feature actually specified)
   - `make check` output (build actually passes)
8. **Handoff if needed**:
   - In-game terminology/tone → Narrative / Noir Copy Specialist
   - Screenshot needs → Visual Art / Asset Production Specialist
     **Fallback**: If Visual Art / Asset Production Specialist is not available,
     document the screenshot need concretely in the report and flag to the user:
     which screen/feature, what action/state to capture, recommended framing.
     Do NOT improvise placeholder screenshots or generate AI images.
9. **Report**: devlog draft + evidence + risks.

## Devlog Post Template

Per `devlog/template.md` conventions:

```markdown
# [Post Title — descriptive, player-focused]

[Paragraph 1: the main change — what's new and what it gives the player]
[Paragraph 2: secondary change or context — how it changes the feel]
[Paragraph 3 (optional): what's coming next or call to try]

[Image: <screenshot description for Visual Art Specialist>]
[Try it: polyakovin.itch.io/safe-cracker (URL migrating to Locksmith)]
```

Rules:
- 1-3 paragraphs max.
- Russian language.
- Each paragraph = player benefit, not implementation detail.
- Screenshot references as descriptions (Visual Art Specialist produces).
- No code, no APIs, no variable names.
- Game = Locksmith throughout.

## Release Notes Template

```
Locksmith vX.Y — [Release Title]

Что нового:
- [Gameplay change 1] → [что это даёт игроку]
- [Gameplay change 2] → [что это даёт игроку]
- [Gameplay change 3] → [что это даёт игроку]

Техническое:
- [Internal changes — brief, 1-2 lines, no detail]

Попробовать: polyakovin.itch.io/safe-cracker
```

## Change Filter Audit

Before drafting, run a filter audit to ensure changes are player-facing:

```bash
# Show recent commits with messages
git log --oneline -30 --no-merges

# Manually classify each commit:
# [PLAYER] — gameplay/UX change (keep)
# [TECH] — refactor, test, build, CI, dep (discard)
# [AMBIG] — unclear from message (read the diff)
```

## Claim Verification Pack

For each gameplay claim in the post, verify:

```bash
# 1. Claim: feature exists in specs
rg -n "<feature keyword>" docs/

# 2. Claim: feature was actually committed
git log --oneline --all --grep="<feature keyword>"

# 3. Claim: build passes with this feature
cd /root/locksmith && make check 2>&1 | tail -5

# 4. Claim: feature is described in game.md
rg -n "<feature>" docs/game.md
```

If any verification fails, either:
- remove the claim from the post, or
- flag it as "ожидается в следующем обновлении" (coming soon), or
- escalate to the specialist who owns the feature.

## Validation Pack

Run from Locksmith root:

```bash
make check && echo "Check: PASS" || echo "Check: FAIL"
```

For marketing, `make check` is informational only — it doesn't block copy
work unless the feature being described is the one that's failing.

## Social Copy Template

Short form (tweets, mastodon, telegram):

```
[1-2 lines, Russian, gameplay hook]
[Link: polyakovin.itch.io/safe-cracker]
[Optional: 1 hashtag]
```

Example:
"Замок больше не прощает ошибок: теперь каждая сувальда чувствуется через
crank. Попробуйте новый Adventure Mode → polyakovin.itch.io/safe-cracker
#PlayDate"

## Itch.io Page Refresh Workflow

1. Read current page content (user provides or agent reads from
   `README.md`).
2. Read `docs/game.md` for current feature set.
3. Identify mismatches:
   - Status still says "Prototype" when game is further along.
   - Description mentions features that were removed.
   - Tags don't match current game (genre, mechanics).
   - URL still says "safe-cracker" but game is now Locksmith.
4. Propose updates:
   - Description: 2-3 sentences, what the game IS now.
   - Tags: accurate to current genre/mechanics.
   - Status: current development phase.
   - URL migration note: "Previously Safe Cracker, now Locksmith."
5. Draft new page copy.
6. Do NOT publish — user reviews and posts.

## Pitfalls

1. **Claiming unimplemented features**: writing "теперь есть Endless Mode"
   when it's only spec'd but not built. Always verify against git log.

2. **Technical jargon leak**: "переписали safe.lua на новую физику" should
   be "замок теперь реагирует на каждое движение." No source file names.

3. **Over-filtering**: discarding a real gameplay change because the commit
   message sounds technical (e.g., "adjusted crank sensitivity" IS
   player-facing — the crank FEELS different).

4. **Emoji in formal posts**: devlog and itch.io are formal channels.
   Emoji only in social snippets if requested.

5. **Writing in English by default**: this specialist writes in Russian
   unless the task explicitly says otherwise.

6. **Forgetting the handoff**: if a devlog sentence describes how Silas
   Crane feels or speaks, that's in-game territory — hand off to
   Narrative Noir Copy Specialist.

7. **Safe Cracker vs Locksmith confusion**: legacy URL is
   polyakovin.itch.io/safe-cracker — reference it as "legacy URL,
   currently migrating to Locksmith." Never call the game Safe Cracker
   in marketing copy.

8. **Posting without approval**: the specialist DRAFTS. The user reviews
   and posts to itch.io. Never claim to have published.

9. **Screenshot placeholder as final**: "скриншот главного меню" is a
   REQUEST to Visual Art Specialist, not a final element. Document what's
   needed.

10. **Missing build evidence**: if `make check` is broken, that's fine —
    but document it. Don't claim features work if you can't prove it.

## Commit And Push Gate

Marketing copy does NOT modify code. Commits only happen when:

- `AGENTS.md` Pitfalls are updated with a marketing copy bug.
- `devlog/template.md` format is updated.

For copy drafts, the deliverable is the text — no git commit needed.

If committing agent infrastructure changes:

1. Check dirty state: `git status --short` in both repos.
2. Stage only scoped marketing-specialist changes.
3. Commit with atomic message: `"feat(marketing): <change description>"`
4. Push the current branch.
5. Record commit SHA and push result.
