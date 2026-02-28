# ClawFi Dev Team

> Ship ClawFi 24/7 with an AI agent development team.

## Team Structure

```
                ┌─────────────────────────────────┐
                │      Kevin (Product Owner)       │
                │  (requirements, final approval)  │
                └───────────────┬─────────────────┘
                                │
                         requirements
                                │
                ┌───────────────▼─────────────────┐
                │  Lisa (Coordinator + Lead Dev)   │
                │  task splitting, architecture,   │
                │  core dev, code review, QA       │
                └───────┬───────────────┬─────────┘
                        │               │
                 Batch 1│        Batch 2│
                        │               │
                 ┌──────▼────┐   ┌──────▼─────┐
                 │ Sub-agent │   │ Sub-agent  │
                 │ (Claude   │   │ (Claude    │
                 │  Code)    │   │  Code)     │
                 └──────┬────┘   └──────┬─────┘
                        │               │
                        └───────┬───────┘
                                │ PRs
                ┌───────────────▼─────────────────┐
                │      Lisa (Review + QA)          │
                └───────────────┬─────────────────┘
                                │ approved
                ┌───────────────▼─────────────────┐
                │      Kevin (final merge)         │
                └─────────────────────────────────┘
```

## Roles

| Role | Who | Platform | Responsibilities |
|------|-----|----------|------------------|
| **Product Owner** | Kevin | Telegram + GitHub | Requirements, priorities, PRD approval, final merge |
| **Coordinator** | Lisa | OpenClaw (Mac Mini) | Task splitting, architecture, core dev, review, QA, deploy |
| **Developer** | Sub-agents | Claude Code (spawned) | Parallel development on assigned batches |

## Communication

| Channel | Purpose |
|---------|---------|
| Telegram (Kevin ↔ Lisa) | Requirements, status updates, approvals |
| GitHub PRs | Code review, technical discussions |
| GitHub Issues | Task tracking |

## Infrastructure

| Component | Details |
|-----------|---------|
| Runtime | Mac Mini (always-on, headless) |
| Agent Platform | OpenClaw |
| AI Model | Claude (Anthropic) |
| Repo | [hxa-k/clawfi](https://github.com/hxa-k/clawfi) |
| Production | ff.kevinhe.io (port 4738, Cloudflare tunnel) |
| Staging | ff-staging.kevinhe.io |
| Process Manager | PM2 (ff-server + ff-client) |
| Tech Stack | React + TS + Vite / Express + SQLite / Tailwind + shadcn/ui / Recharts |

## Milestones

| Milestone | Issues | Due |
|-----------|--------|-----|
| M1: MVP Polish | #1-5 (edit buttons, color scheme, i18n, rename, mobile) | 2026-03-07 |
| M2: Core Features | #6-10 (recurring, multi-currency, OAuth, insurance, charts) | 2026-03-14 |
| M3: Intelligence | #11-13 (analysis, AI chat, goals/tools) | 2026-03-21 |
| M4: Polish & Launch | #14-15 (reports, family collab) | 2026-03-28 |
