# Communication Protocols

How the team communicates across platforms and agents.

> **hxa-connect** is the agent-to-agent communication layer — thread-based messaging designed for AI agent teams. See [hxa-connect repo](https://github.com/coco-xyz/hxa-connect) for setup.

## Channels

| Channel | Platform | Purpose | Participants |
|---------|----------|---------|-------------|
| Team Thread | hxa-connect | Agent-to-agent coordination | All agents |
| Human Lead Sync | Your preferred IM (Slack, Lark, etc.) | Human Lead ↔ Agents real-time updates | Human Lead, Agent A |
| Code Collaboration | GitHub / GitLab | PR review, issues | All |
| Private DMs | hxa-connect | Sensitive or bilateral discussions | As needed |

Replace the Human Lead Sync platform with whatever your team actually uses. The channel ID goes in `.hxa-teams.yml`.

## Routing Rules

1. **Human Lead instructions** arrive via the designated sync channel. Agent A is responsible for routing to the correct agent and confirming receipt.
2. **Agent-to-agent** coordination happens on the hxa-connect team thread.
3. **Code collaboration** happens on GitHub/GitLab (PRs, issues, reviews) — not in chat.
4. **Progress syncs** are posted to the team thread on a regular cadence (recommended: every 10 minutes during active work sessions).
5. **Private discussions** use hxa-connect DM threads.

## Escalation Paths

| Situation | Action |
|-----------|--------|
| Agent is blocked on a task | Post in team thread, tag Agent A |
| Decision requires Human Lead input | Agent A escalates to the Human Lead sync channel |
| Production incident | Direct message to Human Lead — do not wait for the sync cadence |
| Agent unavailable (missed 2+ syncs) | Agent A notifies team thread + Human Lead, initiates handoff |

## Message Hygiene

- Keep task-specific discussion in the relevant PR or issue, not in the team thread.
- The team thread is for coordination and status — not a substitute for a PR review.
- When a decision is made in chat, record it in the relevant issue or PR so the context isn't lost.
