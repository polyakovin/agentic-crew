# Example A2A Handoff Packet

This example shows an Agentic Crew `handoffPacket` payload carried inside an
A2A artifact update. Runtime SDKs may expose this as a typed object instead of
literal JSON.

```json
{
  "taskId": "a2a-task-server-generated-id",
  "contextId": "sample-playdate-ui-review-context",
  "artifact": {
    "artifactId": "sample-playdate-ui-001-handoff",
    "name": "Playdate platform handoff",
    "description": "Routing packet from Playdate SDK review to pixel UI review.",
    "parts": [
      {
        "data": {
          "profile": "agentic-crew/a2a-profile/v0.1",
          "kind": "handoffPacket",
          "taskId": "sample-playdate-ui-001",
          "specialistId": "playdate-platform-sdk",
          "status": "needsReview",
          "currentState": "Rendering code touches drawTextAligned after setColor in a Playdate UI path.",
          "filesTouched": ["source/ui.lua"],
          "openQuestions": [
            "Does the target SDK version still reproduce the drawTextAligned draw-mode issue?"
          ],
          "nextSpecialist": "playdate-pixel-ui-renderer",
          "recommendedNextSteps": [
            "Inspect affected text paths.",
            "Add or verify screenshot/contact-sheet evidence."
          ],
          "verificationNeeded": [
            "make build",
            "screenshot harness or simulator capture"
          ]
        },
        "mediaType": "application/json"
      }
    ],
    "metadata": {
      "agenticCrewPayloadKind": "handoffPacket"
    }
  },
  "lastChunk": true
}
```
