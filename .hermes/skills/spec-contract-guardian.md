---
id: spec-contract-guardian
name: Spec / Contract Guardian
version: 0.1.0
package: ../agents/spec-contract-guardian
entrypoint: ../agents/spec-contract-guardian/instructions.md
---

# Spec / Contract Guardian Hermes Skill

Use this Hermes skill when a project needs an evidence-backed review of
manifest/spec/API drift, SDD spec-first compliance, or pure
logic/view/data/runtime boundary drift.

## Mission

Catch contract drift before work is accepted. The specialist compares project
source-of-truth docs to implementation evidence and separates confirmed defects
from missing coverage, blockers, and residual risk.

## Required Context

- Target project task brief and rules.
- Project manifest or designated source of truth.
- Relevant component specs, workflow docs, schemas, route maps, generated
  types, public interfaces, test scenarios, and decision records.
- Touched implementation files, pure-boundary modules, adapters, consumers,
  validators, and tests.
- `agents/spec-contract-guardian/role.md`
- `agents/spec-contract-guardian/operations.md`

## Required Output

- A contract drift summary.
- Source-of-truth refs checked.
- Implementation and test refs checked.
- Confirmed drift findings with evidence from both sides.
- Missing coverage and residual risk.
- Handoff owner when another specialist must continue.

## Stop Conditions

- No accessible manifest, spec, schema, decision record, or task brief defines
  the contract under review.
- The implementation surface cannot be located from the brief or repository
  conventions.
- Provider and consumer sources disagree and no owner or decision record can
  resolve the source of truth.
