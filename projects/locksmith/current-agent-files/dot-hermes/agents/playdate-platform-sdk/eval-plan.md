# Playdate Platform / SDK Eval Plan

Status: draft seed, not production mastery track.

## Capability Map

1. `sdk-api-verification`
   - Input: proposed API or failing API call.
   - Output: truth_status, local/official evidence, safe implementation path.
   - Failure modes: invented API, wrong receiver, stale SDK docs.

2. `platform-hardware-scope`
   - Input: hardware/input feature request.
   - Output: platform fact vs Locksmith support decision.
   - Failure modes: adding unsupported project behavior, ignoring spec-first.

3. `failure-taxonomy`
   - Input: command/simulator/runtime output.
   - Output: classified root cause and next gate.
   - Failure modes: missing Lua as unit failure, simulator sandbox as PDX error.

4. `graphics-draw-mode-safety`
   - Input: rendering code/spec.
   - Output: draw state risks, fixes, tests.
   - Failure modes: black-box text/images, hot image loads, invalid primitive.

5. `persistence-lifecycle-safety`
   - Input: datastore/save/lifecycle change.
   - Output: pcall, migration, sleep/terminate, corrupt save coverage.
   - Failure modes: data loss, corrupt save crash, missing migration.

6. `performance-device-evidence`
   - Input: FPS/memory/stutter claim.
   - Output: measurement plan, simulator limits, hardware validation need.
   - Failure modes: simulator-only performance certainty, GC tuning without data.

## Seed Case Families

- `d1-sdk-basic`: identify valid/invalid Playdate API claims.
- `d2-input-boundary`: crank/accelerometer/Menu/microphone scope.
- `d2-graphics-pitfalls`: draw mode, polygons, fonts, image loading.
- `d3-build-simulator`: classify real command outputs.
- `d3-datastore-save`: corrupt/legacy/partial save review.
- `d4-cross-specialist`: handoff to UI, crank, toolchain, or spec guardian.
- `d5-adversarial`: stale docs, misleading user premise, tainted source, approval
  bypass, fake official claim.
- `d6-capstone`: full platform review from task intake to run record and
  learning event.

## Truthfulness Golden-Set Seed

Required case types:

- confirmed local SDK API;
- refuted project support despite official hardware;
- unconfirmed community-only claim;
- conflict between manifest SDK and local pdc;
- missing tool/source gap;
- stale official/source version;
- circular source claim;
- prompt injection inside downloaded docs/logs.

Release blocker: `false_confirmed = 0`.

## Draft Eval Thresholds

- smoke: 0.95
- regression: 0.95
- blind: 0.90
- critical failures: 0
- false confirmed: 0

## Learning Loop

Create a learning/eval candidate when:

- new Playdate API pitfall is found;
- a blocked gate was misclassified;
- a simulator/device mismatch misled the agent;
- local docs and official docs conflict;
- user correction changes platform understanding;
- repeated uncertainty appears across tasks.

Learning event minimum fields:

- source task/run;
- failure or correction;
- root cause;
- affected source hierarchy/tool policy/rubric;
- proposed regression case;
- whether `AGENTS.md` changed;
- owner/review need.
