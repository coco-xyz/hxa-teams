# ClawFi — Team Plan

> Generated from `dev-team` template. Project: family finance web app.

## Role Assignments

| Template Role | Assigned To | Platform | Notes |
|---------------|-------------|----------|-------|
| Product Owner | Kevin | Telegram + GitHub | Requirements, PRD approval, final merge |
| Coordinator + Lead Dev | Lisa | OpenClaw (Mac Mini) | Architecture, task splitting, core dev, code review, QA, deploy |
| Developer(s) | Sub-agents | Claude Code (spawned by Lisa) | Parallel batch development |

> Small team (2+N): Lisa covers Coordinator + QA roles. Sub-agents are ephemeral — spawned per batch, not persistent.

## Communication

| Channel | Platform | Purpose |
|---------|----------|---------|
| Kevin ↔ Lisa | Telegram DM | Requirements, status, approvals |
| Lisa ↔ Sub-agents | OpenClaw sessions_spawn | Task assignment, results |
| Code discussion | GitHub PRs | Review feedback, technical Q&A |
| Task tracking | GitHub Issues | 15 issues across 4 milestones |

## Infrastructure

| Component | Details |
|-----------|---------|
| Repo | [hxa-k/clawfi](https://github.com/hxa-k/clawfi) |
| Production | ff.kevinhe.io (Cloudflare tunnel → port 4738) |
| Staging | ff-staging.kevinhe.io |
| Process Manager | PM2 (ff-server + ff-client) |
| Project Dir | ~/clawd/clawfi/ |
| Tech Stack | React + TS + Vite / Express + SQLite / Tailwind + shadcn/ui / Recharts |
| AI Model | Claude (Anthropic) |

## Workflow

1. Kevin posts requirement → Telegram
2. Lisa writes PRD → `docs/prd/` → PR
3. Kevin approves PRD
4. Lisa splits into batches → GitHub Issues
5. Lisa spawns sub-agents for parallel dev
6. Sub-agents submit PRs → Lisa reviews
7. Kevin final review + merge
8. Lisa deploys (PM2 restart) + smoke test

## Milestones

| Milestone | Issues | Due |
|-----------|--------|-----|
| M1: MVP Polish | #1-5 | 2026-03-07 |
| M2: Core Features | #6-10 | 2026-03-14 |
| M3: Intelligence | #11-13 | 2026-03-21 |
| M4: Polish & Launch | #14-15 | 2026-03-28 |
