# Agent Architect / Crew Builder Rubric

Use this rubric before handing off a created or reviewed agent.

## Excellent

- The new role has a unique trigger, input, output, and risk-reduction purpose.
- Creation scope is explicit and fits the target project's rules.
- Existing agents were checked for overlap.
- Existing harnesses were checked for reuse/adaptation and the best base was
  used or rejected with rationale.
- Every harness capability category from `ai-db` is reviewed and marked `use`,
  `defer`, or `reject` with rationale.
- Agent Tester review was requested after creation/update and its findings,
  backlog, or critical repair handoffs are recorded.
- Required A2A, Codex, and Hermes surfaces are present when required.
- Specialist-only agent data and tooling were moved into the owning package or
  explicitly recorded as intentionally shared.
- Pack routing is narrow and points to existing paths.
- Validation evidence is recorded.
- Scoped changes are committed and pushed, or a precise commit/push blocker is
  recorded.
- Target project status is updated only after validation passes.
- Handoff names the next owner and unresolved risks.

## Acceptable

- The harness is complete and validates.
- Runtime wrappers exist but remain `draft-ready` pending pilot evidence.
- Ownership extraction is recorded, even if no artifacts needed moving.
- Scope and reuse analysis are recorded.
- Capability inventory is recorded, with deferred/rejected capabilities tied to
  risk or maintenance rationale.
- Agent Tester review is recorded or a concrete tester-unavailable blocker is
  present.
- Some eval seeds are lightweight, but cover duplicate roles and missing
  wrappers.

## Needs Revision

- The role overlaps another specialist.
- Scope is assumed from habit instead of recorded.
- A new harness is created despite an obvious reusable base.
- Capability inventory is skipped or reduced to "use everything".
- Agent Tester review is skipped after creating/updating a specialist.
- `What To Read` is too broad.
- Codex or Hermes wrappers are missing when required.
- Quality gates are subjective or not testable.
- Specialist-owned prompts/tools/evals remain duplicated in shared project
  space without rationale.
- Pack routing uses vague keys like `misc` or `general`.

## Critical Failure

- Creates a specialist without approved target demand or decision record.
- Marks production-ready without pilot/eval/review evidence.
- Changes target application code.
- Moves product specs/source/assets/tests under the guise of agent packaging.
- Writes wrappers into a target repo that forbids local `.agents` or `.hermes`.
- Leaks private target paths or snapshots into a reusable package.
- Claims validation passed without running machine-readable checks.
- Commits or pushes unrelated dirty worktree changes.
- Reports complete while push is blocked without recording the blocker.
- Reports a created or promotion-ready specialist while Agent Tester critical
  findings remain open.

## Challenge Checklist

- Why is this an agent, not a skill?
- Which existing agent is closest, and why is it insufficient?
- Should this live in the target project, Agentic Crew, or both?
- What existing harness can be reused or adapted?
- Which runtime surfaces are mandatory?
- Which harness capabilities from `ai-db` are useful, deferred, or rejected?
- What did Agent Tester find, and which backlog items block promotion?
- Which agent-facing artifacts should move into the specialist package?
- Which artifacts are intentionally shared interfaces?
- What is the smallest `What To Read` list?
- What would make this package unsafe to publish?
- What eval catches the most likely future regression?
- What commit and push evidence proves the package was persisted?
