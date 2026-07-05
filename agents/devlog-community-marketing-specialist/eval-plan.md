# Devlog / Community Marketing Specialist Eval Plan

## Purpose

Evaluate whether the Devlog / Community Marketing Specialist correctly writes
player-facing devlog posts, social copy, screenshot captions, and trailer
messaging for Locksmith — with consistent naming, noir/gaslight tone, and
evidence-backed claims.

## Metrics

- devlog template compliance (devlog/template.md format);
- claim verification thoroughness;
- game naming consistency (Locksmith, not Safe Cracker);
- hero naming consistency (Silas Crane);
- tone authenticity (noir/gaslight indie developer voice);
- screenshot caption accuracy;
- trailer messaging alignment with lore;
- handoff specificity to other specialists;
- legacy URL migration messaging;
- user approval gate respect;
- validation honesty;
- commit/push completion.

## Seed Cases

### DM-001: Devlog Post from Git Changes

Input: Locksmith has several new commits. Write a devlog post for itch.io.

Expected:

- Git log reviewed for gameplay/UX changes only.
- 1-3 most interesting changes identified.
- Post follows `devlog/template.md`: title, hashtags, player-facing paragraphs, screenshot placeholder, CTA.
- Every claim backed by spec reference or build evidence.
- Game consistently called Locksmith, hero Silas Crane.
- Tone is indie developer, noir-tinged, not corporate.
- Draft presented — not published.

### DM-002: Social Media Announcement

Input: A new major feature (e.g., Endless market) is complete. Write a social media post.

Expected:

- One-line hook with player benefit.
- 2-3 sentences explaining the change.
- Correct hashtags: #Playdate #indiedev #gamedev #LocksmithGame.
- Itch.io link included.
- Tone matches noir/gaslight indie voice.
- Naming: Locksmith / Silas Crane.
- Draft presented — not posted.

### DM-003: Screenshot Captions

Input: New screenshots of Adventure mode safe-dial scene are available. Write captions.

Expected:

- Each caption is one line.
- Explains what the player sees and what mechanic/scene is shown.
- Matches noir tone.
- Uses correct character names from lore.
- Example: "Сайлас Крейн слушает щелчки сейфового механизма через стетоскоп."

### DM-004: Itch.io Page Update

Input: Locksmith needs an updated itch.io description reflecting current features.

Expected:

- Drafted description with tagline, feature highlights, genre.
- Updated tags: PlayDate, puzzle, lockpicking, noir, incremental.
- Game called Locksmith throughout.
- Legacy Safe Cracker URL handled as migration.
- Draft presented — not published.

### DM-005: Trailer/Teaser Messaging

Input: A trailer for Locksmith is being produced. Write the text messaging.

Expected:

- Short, punchy taglines matching noir/gaslight aesthetic.
- Scene descriptions that match game lore.
- Key phrases from lore used correctly (e.g., "Каждый замок — это механизм.").
- Naming: Locksmith / Silas Crane.
- Tone consistent with Silas Crane's world.

### DM-006: Claim Verification

Input: A devlog post claims "new Endless market with live-lot timers."

Expected:

- Claim checked against `docs/game.md` Endless section.
- Claim checked against `docs/test-scenarios.md` Endless test cases.
- Evidence cited: specific spec section or test case ID.
- If claim cannot be verified, flagged as `unverifiedClaim`.

### DM-007: Canon Handoff

Input: Devlog post mentions Margo and Ezra by name in player-facing copy.

Expected:

- Identifies which text touches in-game canon.
- Hands off the specific text segments to Narrative / Noir Copy Specialist.
- Includes required reading: `docs/lore.md`.
- Does NOT publish copy without canon review.

### DM-008: Screenshot Coordination

Input: Devlog post needs a screenshot of the new Adventure safe-dial scene.

Expected:

- Documents which scene/game state needs capturing.
- Hands off to Visual Art / Asset Production Specialist.
- Provides context for caption writing.
- Does NOT generate or fake screenshots.

### DM-009: Legacy URL Messaging

Input: Itch.io page needs text explaining the Safe Cracker → Locksmith transition.

Expected:

- Drafts migration messaging: "ранее игра была известна как Safe Cracker".
- Does NOT rename the game back to Safe Cracker.
- Maintains Locksmith branding throughout.
- Transition is framed as evolution, not apology.

### DM-010: No Technical Jargon

Input: Git log shows "refactored renderer.lua to use dynamic pin-tunnel split."

Expected:

- Translates to player language: "визуальные эффекты замка стали чище и детальнее".
- Does NOT mention Lua files, refactoring, or implementation details.
- Every sentence answers "what does this give the player?"

## Promotion Threshold

Move from `draft` to `draft-ready` after:

- all harness files exist and validate;
- Hermes and Codex wrappers created in both repos;
- routing added to playdate-game-crew.yaml.

Move to `pilot` after:

- at least one real devlog post written for Locksmith;
- at least one social copy draft with user feedback;
- screenshot captioning exercised.

Move to `production` after:

- three devlog posts published on itch.io with positive reception;
- zero naming errors (Locksmith / Silas Crane) in public copy;
- agent-tester review passed.
