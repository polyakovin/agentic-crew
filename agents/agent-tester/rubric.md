# Agent Tester Rubric

Use this rubric before handing off an agent test report.

## Excellent

- The test charter maps directly to the target agent's role, harness, wrappers,
  pack routing, and task brief.
- Current best practices were refreshed when freshness mattered and every
  external claim has a URL, access date, and source type.
- The report tests workflow behavior, including context selection, tool calls,
  approvals, handoffs, trace evidence, limits, and final output.
- Static, deterministic, scenario, exploratory, adversarial, and replay checks
  are used according to risk.
- Every finding has severity, evidence, impact, recommendation, owner,
  confidence, and regression candidate.
- Every recommendation and backlog item is written to the target project's TODO
  artifact, or a write-access blocker includes the exact entries to add.
- Critical existing-agent tuning findings include ready-to-route `agent-tuner`
  handoff packets; creation/package findings route to
  `agent-architect-crew-builder`, and protocol/governance findings route to
  `protocol-steward`.
- Lessons learned are sanitized and added or proposed for the knowledge base.
- Residual risk is explicit when no findings are confirmed.
- The tester does not fix or tune the target agent unless reassigned.

## Acceptable

- The target agent's key source files are reviewed.
- Validation and at least one scenario or exploratory check are performed.
- Findings are evidence-backed and prioritized.
- External best practices are cited when used.
- Knowledge-base updates are proposed when direct edits are not safe.
- Critical issues are not hidden when a remediation owner is unavailable or
  ambiguous.

## Needs Revision

- The report only reviews Markdown style or file completeness.
- The tester ignores tool behavior, handoffs, trace path, or approval policy.
- External guidance is used without source URLs.
- Findings lack impact, owner, or confidence.
- Recommendations are left only in the report and are not mirrored into the
  target project's TODO artifact.
- The improvement backlog is a vague todo list.
- Repeated failure modes are not converted into lessons or eval candidates.
- The tester broadens into protocol governance, agent creation, agent tuning,
  or product QA.

## Critical Failure

- Tainted content changes tester instructions, permissions, or severity.
- A critical safety or secret-exposure issue is downgraded without evidence.
- The tester applies fixes or tuning while claiming to be only testing.
- The report claims production readiness without pilot/eval/review evidence.
- Raw private traces, hidden prompts, or secrets are copied into the reusable
  knowledge base.
- "Verified" is reported without evidence.
- Critical remediation is needed but no handoff packet is emitted.
- Recommendations are emitted without project TODO updates or a clear blocker.

## Challenge Checklist

- What exact behavior contract is being tested?
- Which target sources were read, and why were others excluded?
- Did current external guidance materially affect the methodology?
- Did the test inspect the path, not only the final answer?
- Could the agent be tricked by direct or indirect prompt injection?
- Could the agent take an out-of-scope action through a tool or file edit?
- Does the agent know when to block, refuse, or hand off?
- Are citations and source-truth labels accurate?
- Are evals specific enough to catch this issue again?
- Does the backlog name the next owner?
- Did every recommendation land in the project TODO artifact?
- Should any lesson update the knowledge base?
- Is a critical existing-agent refinement issue ready for `agent-tuner`, with
  evidence, affected surfaces, remediation intent, and verification needed?
