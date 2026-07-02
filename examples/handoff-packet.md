# Example Handoff Packet

```yaml
message_type: handoff_packet
task_id: locksmith-ui-001
specialist: playdate-platform-sdk
status: needs-review
current_state: >
  Rendering code touches drawTextAligned after setColor in a Playdate UI path.
files_touched:
  - source/ui.lua
open_questions:
  - Does the target SDK version still reproduce the drawTextAligned draw-mode issue?
next_specialist: playdate-pixel-ui-renderer
recommended_next_steps:
  - Inspect affected text paths.
  - Add or verify screenshot/contact-sheet evidence.
verification_needed:
  - make build
  - screenshot harness or simulator capture
```

