# Release / Storefront Packaging Specialist

Specialist harness for release packaging, storefront readiness, and release
blocker management for the Playdate game Locksmith.

Status: draft portable harness.

This agent owns the release packaging lifecycle:

- pdxinfo metadata verification;
- launcher asset audit (card.png, icon.png, launch-image.png, card-highlighted animation);
- build evidence collection (make check);
- release notes generation from git log and TODO.md;
- screenshot management and captions;
- trailer/teaser export coordination;
- release blocker identification;
- prototype removal checklist management;
- itch.io page readiness audit;
- legacy Safe Cracker URL migration messaging.

The specialist is intentionally separate from `devlog-marketing-specialist`.
That agent owns devlog copy writing and storytelling. This agent owns packaging
verification, asset readiness, build evidence, and release gates — the
prerequisites for publication, not the prose.

## Files

- `entrypoint.md`: mission, calibration, and package-level entry instructions.
- `role.md`: full role card with scope, non-goals, workflow, and handoff contract.
- `agent-card.json`: A2A discovery metadata.
- `harness.yaml`: local wiring, gates, runtime placeholders, and automation packs.
- `source-map.md`: source hierarchy, ownership boundaries, duplicate role review.
- `workflow.md`: release audit workflow, boot probe, and validation commands.
- `tool-policy.md`: permissions, side-effect gates, and forbidden actions.
- `rubric.md`: self-review and packaging quality checklist.
- `eval-plan.md`: seed eval cases and promotion thresholds.
- `run-record.template.json`: structured record for release packaging runs.
- `release-rollback.md`: release blockers, rollback scope, and smoke checks.
- `health-snapshot.md`: current package health and known gaps.

## Handoff Targets

- **Visual Art / Asset Production Specialist**: launcher art generation and updates.
- **Narrative / Noir Copy Specialist**: release notes sections touching in-game canon.
- **Toolchain / Build Sandbox Gatekeeper**: build infrastructure failures.
