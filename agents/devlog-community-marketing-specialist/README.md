# Devlog / Community Marketing Specialist

Specialist harness for devlog copywriting, itch.io page storytelling, social
copy, screenshot captions, trailer messaging, and community positioning for the
Playdate game Locksmith.

Status: draft portable harness.

This agent owns the public-facing narrative:

- devlog posts on itch.io (per `devlog/template.md`);
- itch.io page description, tags, and game presentation;
- release/update notes (player-facing prose — NOT packaging verification);
- short social copy: post captions, announcements, threads;
- screenshot captions for storefront and social media;
- trailer/teaser text messaging (the words, not the video);
- consistent naming: Locksmith / Silas Crane;
- community positioning and tone.

The specialist is intentionally separate from `release-storefront-packaging-specialist`.
That agent owns packaging verification, asset readiness, build evidence, and release
gates — the prerequisites for publication. This agent owns the prose, storytelling,
and how the game is presented to players.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: full role card with scope, non-goals, workflow, and handoff contract.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and automation packs.
- `source-map.md`: source hierarchy, ownership boundaries, duplicate role review.
- `workflow.md`: devlog creation workflow, boot probe, and validation commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and copy quality checklist.
- `eval-plan.md`: seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for devlog/marketing runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Handoff Targets

- **Release / Storefront Packaging Specialist**: build evidence and release packaging context for release notes.
- **Narrative / Noir Copy Specialist**: in-game canon, tone, item names, lore lines.
- **Visual Art / Asset Production Specialist**: screenshots and visual assets for devlog posts.
- **Toolchain / Build Sandbox Gatekeeper**: build evidence when claims need verification.
