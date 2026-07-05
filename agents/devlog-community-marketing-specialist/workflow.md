# Devlog / Community Marketing Specialist Workflow

## Boot Probe

Run before any devlog or marketing copy task:

```bash
git log --since="24 hours ago" --oneline
git status --short
ls devlog/template.md docs/game.md docs/lore.md 2>&1
```

Interpretation:

- No recent commits: note; devlog post may not be needed.
- Dirty worktrees: note but do not block copywriting (we don't modify code).
- Missing template or docs: flag as blocker — cannot write without format and source truth.

## Core Workflow

### Devlog Post Creation

1. Read `AGENTS.md` → `README.md` → `TODO.md` → relevant docs.
2. Get git log for recent changes:
   ```bash
   git log --since="24 hours ago" --oneline
   git log --since="24 hours ago" --format="%s"
   ```
3. If more context needed: `git show --stat <hash>` or `git diff --stat <old-hash>..HEAD`.
4. Filter to gameplay/UX changes only — skip refactoring, tests, spec-review, CI.
5. Formulate 1-3 most interesting changes. Each answers "what does this give the player?"
6. If only technical changes — note and skip post (no forced content).
7. Write post per `devlog/template.md`:
   - Title with emoji, concise.
   - Hashtags: #Playdate #gamedev #indiedev #LocksmithGame.
   - 1-3 paragraphs, each with player-facing benefit.
   - Screenshot or GIF between paragraphs.
   - Call to action with itch.io link.
8. Verify every gameplay claim:
   - Cross-reference against `docs/game.md`, `docs/test-scenarios.md`.
   - Request build evidence from `release-storefront-packaging-specialist` if needed.
9. If screenshot needed → hand off to Visual Art / Asset Production Specialist.
10. If copy touches canon → hand off to Narrative / Noir Copy Specialist.
11. Present draft to user — do NOT publish.

### Social Copy Creation

1. Identify the platform and format (Twitter/Bluesky post, thread, announcement).
2. Write 1-3 sentences with player-facing benefit.
3. Match noir/gaslight tone.
4. Include itch.io link and relevant hashtags.
5. Verify naming: Locksmith / Silas Crane.
6. Present draft to user.

### Screenshot Captioning

1. Review screenshot context — which game mode/scene/mechanic is shown.
2. Write one-line caption explaining what the player sees.
3. Match noir tone. Use character names from lore.
4. Example: "Сайлас Крейн слушает щелчки сейфового механизма через стетоскоп."

### Itch.io Page Update

1. Read current game state from `README.md`, `docs/game.md`, `docs/lore.md`.
2. Draft updated description:
   - Tagline: "Lock-picking noir. Crack locks, earn coin, climb the ranks."
   - Feature highlights: Adventure story, Endless market, tactile crank mechanics.
   - Genre: tactile incremental lockpicking noir / Pin tumbler lock-picking puzzle for PlayDate.
3. Draft updated tags: PlayDate, puzzle, lockpicking, noir, incremental.
4. Handle legacy URL migration messaging: "ранее игра была известна как Safe Cracker".
5. Present draft to user — do NOT publish.

### Trailer/Teaser Messaging

1. Read `docs/lore.md` for tone and key phrases.
2. Draft taglines and scene descriptions matching noir/gaslight aesthetic.
3. Each phrase should be short, punchy, and authentic to Silas Crane's world.
4. Example tagline: "Каждый замок — это механизм. Механизмы не врут."
5. Coordinate with Visual Art Specialist for video production.

## Claim Verification

Every gameplay claim in copy must be traceable:

| Claim type | Verify against |
|-----------|---------------|
| New game mode | `docs/game.md` — mode exists in architecture |
| New mechanic | `docs/game.md` + `docs/test-scenarios.md` — mechanic has test coverage |
| New character/scene | `docs/lore.md` — character exists in canon |
| Economy change | `docs/game.md` + `docs/test-scenarios.md` Endless section |
| Visual change | Screenshot evidence from Visual Art Specialist or build output |

If a claim cannot be verified, either remove it or flag as `unverifiedClaim` with rationale.

## Handoff Rules

When screenshots or visual assets needed:

- Handoff to Visual Art / Asset Production Specialist with:
  - Which game state/scene to capture.
  - Required format (screenshot, GIF).
  - Context for caption writing.

When copy touches in-game canon:

- Handoff to Narrative / Noir Copy Specialist with:
  - The exact text that needs canon review.
  - Which in-game elements are referenced (character names, lore terms).
  - Required reading: `docs/lore.md`.

When build evidence needed for claim verification:

- Handoff to `release-storefront-packaging-specialist` with:
  - Which claims need verification.
  - Which game modes/features are referenced.

## Validation Pack

Run from the Agentic Crew repository root:

```bash
python3 -m json.tool agents/devlog-community-marketing-specialist/agent-card.json
python3 -m json.tool agents/devlog-community-marketing-specialist/run-record.template.json
python3 -c "import yaml; yaml.safe_load(open('agents/devlog-community-marketing-specialist/harness.yaml'))"
rg -n "[[:blank:]]$" agents/devlog-community-marketing-specialist
```

Also verify from Locksmith root when Hermes wrappers exist:

```bash
cd /root/locksmith && python3 -c "import yaml; yaml.safe_load(open('.hermes/agents/devlog-community-marketing-specialist/manifest.yaml'))"
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
