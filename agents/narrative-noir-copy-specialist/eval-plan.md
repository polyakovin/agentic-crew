# Narrative / Noir Copy Specialist Eval Plan

## Purpose

Evaluate whether the specialist correctly audits, proposes, and implements
copy changes while respecting Playdate ASCII/width constraints, noir tone,
canon-consistency, and spec-driven development.

## Metrics

- task success;
- copy problem clarity;
- tone justification quality;
- ASCII-safety enforcement;
- Playdate width fit verification;
- canon-consistency across files;
- spec update completeness;
- `make check` pass rate;
- Silas Crane voice consistency;
- marketing tone avoidance;
- risk documentation.

## Seed Cases

### NC-001: Noir Tone Audit — Shop Items

Input: "The SHOP_ITEMS loreLine strings all sound like generic RPG shop — make
them feel like a noir flea market."

Expected:

- Reads AGENTS.md → `docs/lore.md` → `source/endless_data.lua`.
- Audits all SHOP_ITEMS loreLine strings for tone.
- Identifies strings that sound like generic RPG ("A fine tool for any
  locksmith.") vs noir ("Picked off a dead guild runner. Still works.").
- Proposes replacements with noir voice justification.
- Updates `docs/lore.md` if naming conventions or tone rules change.
- Modifies `source/endless_data.lua` SHOP_ITEMS loreLine fields only.
- Verifies ASCII-only, width-fit, no marketing tone.
- `make check` passes.
- Report includes: canon check results, tone justification, affected entries.

### NC-002: Playdate Width Fit — Component Purposes

Input: "COMPONENT_ITEMS purpose strings are too long for the 1-bit screen —
trim to two words max."

Expected:

- Reads `docs/ui.md` → `source/endless_data.lua` COMPONENT_ITEMS.
- Audits all purpose strings for length.
- Trims to two words max while preserving meaning.
- Verifies each string is ASCII-only and fits opaque background.
- Modifies `source/endless_data.lua` COMPONENT_ITEMS purpose fields only.
- `make check` passes.
- Manual verification: load Endless mode, open component card, check no
  text overflow.

### NC-003: Guild Rank Noir Naming

Input: "GUILD_RANK names (level 0-5) sound like modern corporate titles — make
them guild-hierarchy noir."

Expected:

- Reads `docs/lore.md` → `source/endless_data.lua` GUILD_RANKS.
- Identifies current rank names and their modern/bland tone.
- Proposes noir guild-hierarchy names (e.g., "Tumbler," "False Gate,"
  "Master Pin," "Grand Ward") instead of "Beginner," "Intermediate,"
  "Advanced" etc.
- Updates `docs/lore.md` with guild rank naming convention if changed.
- Updates `source/endless_data.lua` GUILD_RANKS.
- Checks save compatibility — if rank names are in save data, flags risk.
- `make check` passes.

### NC-004: HUD Feedback Noir Fit

Input: "HUD feedback 'NO COINS' sounds like a mobile game — noir it up."

Expected:

- Reads `source/endless_mode.lua` → `source/ui.lua`.
- Identifies HUD/feedback strings with modern/mobile tone.
- Proposes noir alternatives (e.g., "NO COINS" → "POCKETS EMPTY").
- Checks ASCII-only, width fit on 400×240.
- Updates `source/endless_mode.lua` or `source/ui.lua`.
- `make check` passes.

### NC-005: Achievement Tone Audit

Input: "Achievement names and labels need tone audit — some sound like Xbox
achievements."

Expected:

- Reads `source/endless_data.lua` ACHIEVEMENTS.
- Audits all achievement names and labels for modern/casual tone.
- Proposes noir alternatives that are still clear about what was achieved.
- Updates ACHIEVEMENTS name and label fields.
- `make check` passes.

### NC-006: Catalog Entry Tone Drift

Input: "LOCK_CATALOG entry loreLine 'Tricky lock. Better bring coffee.' breaks
tone — no modern coffee jokes in noir."

Expected:

- Reads `source/economy_catalogs.lua` LOCK_CATALOG.
- Identifies the offending entry.
- Proposes replacement with noir voice (e.g., "Tricky lock. Bring steady hands,
  not luck.").
- Checks that no other catalog entries have modern references.
- Updates `source/economy_catalogs.lua`.
- `make check` passes.

### NC-007: Gameover Screen Noir Tone

Input: "Gameover caption 'Better luck next time!' is too cheerful — make it
noir."

Expected:

- Reads `source/ui.lua` gameover screen strings.
- Identifies cheerful/optimistic captions.
- Proposes noir alternatives (e.g., "The lock holds. For now.").
- Verifies ASCII-only and width fit.
- Updates `source/ui.lua`.
- `make check` passes.

### NC-008: Unicode Contamination Prevention

Input: Agent receives text with smart quotes "like this" from a copy-paste.

Expected:

- Agent detects Unicode characters (U+201C, U+201D) before writing.
- Converts to ASCII straight quotes before any file write.
- Runs `rg "[^\x00-\x7F]"` on all modified files before commit.
- Documents the contamination in the report as a risk avoided.

### NC-009: Canon Drift Detection

Input: Agent is asked to add a new shop item referencing "Ezra" but the
lore.md spells it "Ezra" and the agent accidentally types "Ezrah."

Expected:

- Agent cross-references `docs/lore.md` before writing.
- Detects the misspelling.
- Uses canonical spelling "Ezra."
- Documents the canon check in the report.

### NC-010: Spec-First Workflow

Input: User asks to change a character name in `endless_data.lua` without
mentioning `docs/lore.md`.

Expected:

- Agent reads `docs/lore.md` first.
- Identifies the character reference.
- Proposes updating `docs/lore.md` before changing `endless_data.lua`.
- Explains SDD workflow to user if they resist.

## Promotion Threshold

Move from `draft` to `pilot` only after:

- All seed cases pass manually or through eval harness.
- At least one real copy audit/change is implemented, tested, and deployed.
- No ASCII violations in any run.
- No canon drift in any run.

Move to `production` only after:

- Pilot evidence from 3+ copy changes.
- Spec updates are consistent and complete.
- Pitfalls are documented in AGENTS.md.
- No marketing tone leaks.
