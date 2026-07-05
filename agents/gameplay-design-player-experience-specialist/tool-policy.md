# Tool Policy — Gameplay Design / Player Experience Specialist

## Allowed Tools

| Tool | Usage |
|------|-------|
| file (read_file) | Reading project specs, manifest, source code (context only) |
| file (write_file) | Updating docs/*.md specs with design proposals |
| file (patch) | Applying spec updates to docs/*.md |
| terminal | Running `make build`, `make test-all`, `pdc`, `jq empty manifest.json`, git operations |
| web (web_search) | Looking up Playdate SDK documentation for platform constraints |
| web (web_extract) | Reading Playdate SDK docs pages |
| skills (skill_view) | Loading specialist skills for pattern reference |

## Disallowed Tools

| Tool | Why |
|------|-----|
| image_gen | May generate images outside spec-first process; image analysis not in scope |
| vision_analyze | Explicitly prohibited unless user explicitly requests |
| mcp_reddit_* | No Reddit interaction needed |
| text_to_speech | No voice output needed |
| delegate_task | Design proposals should be self-contained; delegation may run into model limits |

## When to Request Escalation

- If code changes are needed: handoff to implementation-engineer or relevant specialist
- If crank/pin micro-mechanics: handoff to crank-feel-specialist
- If pixel/rendering layout: handoff to playdate-pixel-ui-renderer-specialist
- If economy-heavy: handoff to Endless Economy Specialist (when created)
- If cross-specialist coordination needed: handoff to locksmith-orchestrator

## Runtime Constraints

- Proposals must fit within Playdate hardware: 400×240 1-bit, crank 0-360°, D-pad, A/B, Menu button
- No RNG-dependent mechanics
- No full-color feedback
- No audio-essential feedback (audio is enhancement, not core)
- No text-heavy tutorials (1-bit screen, small font)
