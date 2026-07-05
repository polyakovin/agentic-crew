# Devlog / Community Marketing Specialist Rubric

Use this rubric before handing off a devlog post, release notes, or
itch.io page update.

## Excellent

- Git log reviewed and gameplay/UX changes filtered correctly.
- 1-3 most interesting player-facing changes selected with rationale.
- Every gameplay claim cross-referenced against: git log, spec doc,
  `make check` output.
- Devlog post matches `devlog/template.md` format (1-3 paragraphs, Russian).
- Each paragraph answers "что это даёт игроку?" with concrete benefit.
- No variable names, APIs, source files, or implementation details in copy.
- Game referred to as Locksmith consistently; hero = Silas Crane.
- Legacy Safe Cracker URL handled correctly as migration note.
- Tone is developer storyteller — passionate but not corporate.
- Screenshot/caption references are concrete enough for Visual Art
  Specialist.
- Canon/tone handoff to Narrative Noir Copy Specialist done if needed.
- All evidence attached: `make check` output, git log excerpts, spec refs.
- Residual risks documented (Simulator-verification needed, claims pending).
- Social copy provided if requested (1-2 lines, same quality).

## Acceptable

- Git log reviewed but 1-2 changes misclassified (technical flagged as
  gameplay or vice versa).
- Claims mostly backed but one minor claim lacks spec cross-reference.
- Post format mostly correct but deviates slightly from template
  (4 paragraphs instead of 3, or missing screenshot reference).
- Player benefit stated but vague ("стало лучше" without specifics).
- No variable names but occasional technical term slips through.
- Evidence partially provided — `make check` run but git log excerpt
  missing.
- Handoff mentioned but not fully specified.

## Needs Revision

- Git log not reviewed — post written from memory or assumptions.
- Claims about features not verified against specs or builds.
- Post is in English when task says Russian.
- Post reads like a technical changelog: "исправили баг в
  endless_mode.lua строка 142."
- Game called Safe Cracker in marketing copy.
- Corporate marketing tone ("уникальная игра года", "невероятные
  ощущения").
- No player benefit — post describes WHAT changed but not WHY it matters.
- Emoji in devlog post.
- Screenshot references are "скриншот игры" — too vague.
- No handoff when copy edges into in-game text territory.
- No evidence at all — claims unverified.

## Critical Failure

- Renames game to Safe Cracker in marketing copy.
- Describes features that don't exist in the game.
- Publishes to itch.io without user approval.
- Modifies game code or data files.
- Generates fake screenshots or passes AI images as game screenshots.
- Writes in-game text (item names, loreLine, character dialogue).
- Claims build passes when it doesn't (falsified evidence).
- Uses company/client name without permission.
- Commits marketing copy to the game repo as if it were code.
- Ignores explicit user instruction (e.g., "don't mention X" but
  mentions X).

## Challenge Checklist

- What changed in this update? (git log filtered to gameplay/UX)
- Which 1-3 changes are most interesting to players?
- Does each paragraph answer "что это даёт игроку?"
- Is every gameplay claim backed by a spec and `make check` evidence?
- Is the text in Russian, compressed, gameplay-first?
- Are there any variable names, APIs, or source file references?
- Is the game called Locksmith (hero: Silas Crane)?
- Is the legacy URL handled as migration, not rename?
- Does the post match `devlog/template.md` format?
- Are screenshot references concrete and actionable?
- Is the tone "developer storyteller" — not corporate, not technical?
- Is handoff clear if in-game copy/tone questions arise?
- Are residual risks documented?
- Would a player reading this understand what's new and why it matters?
- Would a player be excited to try the update?
