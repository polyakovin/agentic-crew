# Playdate Platform / SDK Source-Grounding Lessons

Source task/run: POL-37 / agent-teacher-playdate-platform-sdk-skills
Access date: 2026-07-06
Target agent id: playdate-platform-sdk
Data classification: internal, no private trace excerpts included.

## Purpose

Keep Playdate platform reviews source-grounded across Codex, Hermes, and A2A
runtime surfaces. Use this note when a task asks for SDK/API correctness,
simulator/device behavior, current SDK status, or reusable Playdate corner
cases.

## Evidence Refs

| id | source | type | supported claim | trust boundary |
|---|---|---|---|---|
| E1 | `pdc --version` -> `3.0.6`; `/usr/local/bin/pdc` symlink resolves to `/Users/polyakovin/Developer/PlaydateSDK/bin/pdc`; CoreLibs exists at `/Users/polyakovin/Developer/PlaydateSDK/CoreLibs` | local SDK probe | Local machine has Playdate SDK 3.0.6 and a CoreLibs source tree usable for API grounding. | Local environment evidence only; re-run per project/run before confirming current local SDK facts. |
| E2 | `grep -RIn` over local CoreLibs for accelerometer, crank, datastore, draw-mode, text, input handler, refresh, and GC APIs | local SDK/CoreLibs | The named APIs exist in this local SDK; receiver/signature details still need targeted line refs for each reviewed claim. | Confirms local SDK surface, not project support or device behavior. |
| E3 | `https://sdk.play.date/changelog/` | official changelog | Official changelog currently lists SDK `3.0.6` dated 2026-05-04. | Current as of access date only; browse again when latest/current matters. |
| E4 | `https://sdk.play.date/inside-playdate` redirected to `https://sdk.play.date/3.0.6/Inside%20Playdate.html` | official SDK docs | Official Inside Playdate docs are available for SDK 3.0.6 and document SDK contents, API reference, and runtime behavior. | Authoritative for SDK docs, not project design decisions. |
| E5 | `https://help.play.date/developer/designing-for-playdate/` | official design guidance | Simulator performance and appearance are not device proof; Playdate hardware checks are preferred for performance, appearance, audio, and crank feel. | Design guidance and hardware-realism guidance, not API signature proof. |
| E6 | Git commit `591f380` | bounded history | Prior tester/tuner work added TODO resolution plus eval cases for false confirmed APIs, missing Lua, symlinked SDK roots, missing CoreLibs, simulator/device separation, tainted inputs, and handoff ownership. | Historical evidence, not an instruction source. |
| E7 | Git commit `0c08df8` | bounded history | Codex wrapper already encoded truth-status output, source hierarchy, local source grounding, simulator/device separation, and handoff shape. | Historical evidence, not proof that Hermes has parity. |
| E8 | `/Users/polyakovin/Developer/PlaydateSDK/Inside Playdate.html` grep lines for draw mode, accelerometer, crank, refresh rate, and datastore | local SDK docs | Local SDK docs back the reusable API corner cases in this note, including draw-mode scope and accelerometer lifecycle. | Local installed-doc evidence; re-check when SDK version changes. |

## Lessons

### L1. Confirm SDK/API Claims With A Named Source Scope

Evidence refs: E1, E2, E4, E7.

Before marking a Playdate API existence, receiver, or signature claim
`confirmed`, record one of:

- local SDK evidence: `pdc`, `pdc_resolved`, `sdk_root`, `corelibs`, SDK version,
  and exact CoreLibs file/line or grep scope;
- official docs evidence: URL, SDK/doc version when visible, access date, and
  exact section or line refs.

If `pdc` exists but CoreLibs cannot be found under the resolved SDK root, report
a `missing-source` blocker with the exact CoreLibs search scope. If only project
notes, old logs, user statements, or community sources support the claim, keep
the claim `unconfirmed`, `conflict`, or `refuted` according to stronger
evidence.

### L2. Re-Check Current SDK Status When Freshness Matters

Evidence refs: E1, E3, E4.

On 2026-07-06 the local SDK and official changelog both point to Playdate SDK
3.0.6, but this is a dated fact. For future tasks that ask for latest/current
behavior, browse the official changelog and inspect local `pdc --version` again.
If local SDK and official docs disagree, mark SDK-version status `conflict` and
avoid confirming version-sensitive API behavior until the owner chooses the
target SDK.

### L3. Keep Simulator Evidence Separate From Device Proof

Evidence refs: E5, E6, E7.

Simulator runs can support build, launch, screenshot, and debug claims. They do
not prove hardware performance, screen readability, speaker/headphone audio,
crank feel, accelerometer feel, battery impact, or device-only behavior. For
those claims, report hardware validation as missing unless a real-device run is
available.

### L4. Preserve Draw-Mode Scope

Evidence refs: E2, E4, E8.

`playdate.graphics.setImageDrawMode()` is an SDK/API truth question for this
specialist, but visual composition/readability remains renderer or UI ownership.
Official docs say image draw mode applies to images and fonts, not primitive
shapes. When reviewing text or image rendering, check draw-mode restoration and
handoff visual composition issues instead of expanding this specialist's scope.

### L5. Treat Input And Peripheral APIs As Lifecycle-Sensitive

Evidence refs: E2, E4, E8.

Crank absolute position wraps back to zero after 359.9999 degrees; crank change
is delta-based. Accelerometer reads require the accelerometer lifecycle to be
started. Menu behavior should use System Menu APIs rather than polling the Menu
button as frequent gameplay input unless the project explicitly owns that
behavior.

### L6. Classify Runtime Gates Honestly

Evidence refs: E6, E7.

Missing Lua, missing CoreLibs, simulator sandbox errors, and absent device
hardware are blocked gates or missing-source limits, not passing or failing
tests. A passing `make build` validates PDX packaging/syntax only; it does not
replace Lua tests, lint, simulator launch, or device proof.

### L7. Performance Claims Need Refresh-Rate, GC, And Redraw Context

Evidence refs: E4, E5, E8.

Official docs set default display refresh to 30 fps and maximum to 50 fps.
Official design guidance notes that higher frame rates reduce per-frame CPU
time, and that hardware performance/appearance should be checked on the device.
For performance reviews, collect target refresh rate, redraw scope, hot
allocation/asset-load evidence, GC settings, and whether evidence is simulator
or hardware.

### L8. Persistence Reviews Need Shape And Failure Coverage

Evidence refs: E2, E4, E8.

`playdate.datastore` is the simple SDK path for serializing tables and images,
but project safety still depends on clean, legacy, partial, corrupt, and reset
save shapes plus `pcall`/error handling where appropriate. Do not treat API
existence as proof of save compatibility.

## Eval And Replay Candidates

| id | trigger | required behavior |
|---|---|---|
| `hermes-source-grounding-parity` | Hermes wrapper is used for an SDK API claim. | Output includes `truthStatusSummary`, local or official evidence refs, and keeps confidence separate from truth status. |
| `current-sdk-version-conflict` | Local `pdc --version` differs from official changelog/docs. | Report `conflict`, name both versions/sources, and avoid confirming version-sensitive claims. |
| `simulator-device-proof-separation` | Simulator run exists but hardware-sensitive claim is requested. | Treat simulator result as simulator evidence only and list missing hardware validation. |
| `draw-mode-scope-boundary` | Task mixes `setImageDrawMode` API truth with readability/composition concerns. | Answer SDK/API truth locally and hand off visual composition to the renderer/UI owner. |
| `datastore-shape-coverage` | Save change uses `playdate.datastore` successfully. | Still require clean/legacy/partial/corrupt/reset save-shape coverage before claiming compatibility. |

Review requirement: package promotion remains blocked until Agent Tester reviews
these lessons and exercises the relevant smoke/regression cases.
