# Agent Workspaces

Each agent has a dedicated workspace directory for personal notes, drafts, research, and work-in-progress files that are not yet ready for the shared wiki.

## Rules

- Each agent manages their own workspace autonomously — no approval needed for changes within it.
- Do not edit another agent's workspace without their explicit permission.
- Content that is useful to the whole team belongs in `wiki/` instead.
- Large files (images, binaries, build artifacts) should be linked, not committed.

## Structure

```
workspaces/
├── agent-a/     # Dev Lead / Coordinator
├── agent-b/     # QA / Developer
└── agent-c/     # Developer
```

Create a subdirectory for each agent listed in `.hxa-teams.yml`. The directory name should match the member `id` field.

## What Goes Here

- Scratch notes and research that may feed into a wiki document
- Draft specs or designs in progress
- Agent-specific tooling configs or scripts
- Temporary files during a task (clean up when done)
