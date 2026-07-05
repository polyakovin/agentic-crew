# Release / Storefront Packaging Specialist Rubric

Use this rubric before handing off a release packaging audit or report.

## Excellent

- pdxinfo fully verified with all fields checked against expected values.
- Launcher asset audit complete: every file checked for presence and correct size.
- Card-highlighted animation verified with animation.txt cross-reference.
- Build evidence obtained from `make check` with honest reporting of failures.
- Release notes backed by implemented specs and build evidence.
- Prototype removal checklist complete with user approval documented.
- Release blockers identified with clear ownership and remediation path.
- Handoffs to Visual Art and Narrative specialists are specific and actionable.
- All game references use "Locksmith" and "Silas Crane" (not Safe Cracker).
- Legacy URL handled as status migration with appropriate messaging.
- No improvised visual assets — all coordinated through proper channels.
- Packaging verdict is evidence-based, not opinion-based.
- Validation evidence recorded.
- Scoped changes committed and pushed.

## Acceptable

- pdxinfo verified but some fields not deeply checked (e.g., description string).
- Launcher assets checked but image dimensions not verified.
- Build evidence obtained but not summarized clearly.
- Release notes drafted but some claims lack explicit spec references.
- Prototype removal checklist partially complete with remaining items documented.
- Release blockers identified but some lack clear remediation path.
- Handoffs are clear but could be more specific.
- Some minor Safe Cracker references slipped through.

## Needs Revision

- pdxinfo not verified against actual file contents.
- Launcher assets not audited at all.
- Build evidence missing or falsely reported.
- Release notes describe unimplemented features.
- Game referred to as Safe Cracker.
- Legacy URL not addressed in release notes.
- Prototype removal checklist skipped.
- Release blockers not identified before declaring "ready".
- Handoffs are vague ("someone should make launcher art").
- Visual assets improvised without coordination.
- Packaging verdict given without evidence.
- Validation not run.

## Critical Failure

- Published to itch.io without user approval.
- Modified pdxinfo, launcher assets, or game code.
- Renamed game to Safe Cracker.
- Falsified build evidence.
- Claimed release readiness with unresolved blockers.
- Generated launcher art instead of coordinating through Visual Art Specialist.
- Changed release status from Prototype to Released without user approval.
- Wrote about features that don't exist in the game.
- Skipped validation and claimed complete.
- Committed or pushed unrelated dirty worktree changes.

## Challenge Checklist

- Is pdxinfo verified against the actual file?
- Are all launcher assets present and correctly sized?
- Is card-highlighted animation verified against animation.txt?
- Is build evidence from make check recorded honestly?
- Are release notes backed by implemented specs?
- Is the game consistently called Locksmith?
- Is the legacy Safe Cracker URL handled properly?
- Is the prototype removal checklist complete?
- Are release blockers identified with ownership?
- Are handoffs to other specialists specific and actionable?
- Has the user explicitly approved any publication drafts?
- What would make this release unsafe to publish?
- What evidence proves the packaging verdict?
