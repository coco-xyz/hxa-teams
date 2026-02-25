# Agent Dev Team

> What if your dev team could run 24/7?

A framework for building **AI agent software development teams** — where multiple AI agents work alongside a human product owner to ship real software, in parallel, around the clock.

This repository documents the roles, workflow, infrastructure, and templates we use to run an autonomous multi-agent dev team. It's based on a real production setup, not theory.

---

## The Concept

Instead of one developer prompting one AI assistant, we treat AI agents as **persistent team members** with defined roles, communication channels, and development processes — just like a human team.

```
                    ┌─────────────────────────────────────┐
                    │        Human Product Owner           │
                    │   (requirements, final approval)     │
                    └──────────────┬──────────────────────┘
                                   │
                                   │  requirements
                                   ▼
                    ┌─────────────────────────────────────┐
                    │       Coordinator Agent (A)          │
                    │  (task splitting, architecture,      │
                    │   coordination, core development)    │
                    └──────┬──────────────────┬───────────┘
                           │                  │
                    assign │ Batch 1   Batch 2│ assign
                           │                  │
                    ┌──────▼─────┐     ┌──────▼──────┐
                    │ Developer  │     │ Coordinator │
                    │ Agent (B)  │     │ Agent (A)   │
                    │ (Batch 1)  │     │ (Batch 2)   │
                    └──────┬─────┘     └──────┬──────┘
                           │                  │
                           │  submit PRs      │
                           ▼                  ▼
                    ┌─────────────────────────────────────┐
                    │        QA / Review Agent (C)         │
                    │  (Codex review loop until CLEAN,     │
                    │   functional QA, CI maintenance)     │
                    └──────────────┬──────────────────────┘
                                   │
                                   │  approved
                                   ▼
                    ┌─────────────────────────────────────┐
                    │        Human Product Owner           │
                    │      (final review + merge)          │
                    └─────────────────────────────────────┘
```

## Key Principles

1. **Parallel Development** — Multiple agents work on different task batches simultaneously, multiplying throughput.
2. **Automated Review** — Every PR goes through iterative AI review (Codex CLI) until it's CLEAN, then agent QA. Humans only review what's already been vetted.
3. **Agent Coordination** — One agent acts as coordinator: splitting tasks, assigning batches, and syncing progress across the team.
4. **Persistent Agents** — Agents have memory, scheduling, and communication capabilities. They don't forget context between sessions.
5. **PRD-Driven** — All features start with a Product Requirements Document, reviewed and approved before development begins.

## Why This Works

| Aspect | Traditional Team | Agent Dev Team |
|--------|-----------------|----------------|
| Working hours | 8h/day, business days | 24/7, including weekends |
| Parallel capacity | Limited by headcount | Scale by adding agent instances |
| Review turnaround | Hours to days | Minutes (automated Codex loop) |
| Context switching | Expensive, error-prone | Agents maintain persistent context |
| Onboarding | Weeks | Configure + provide docs |
| Cost | Salaries + benefits | AI subscriptions (~$200-400/mo per agent) |

**Real outcomes from our pilot project (ClawFeed):**
- Team setup (3 agents + infrastructure) completed in one day
- CI pipeline, PR templates, contributing docs, and branch protection — all created and merged by agents
- Codex CLI iterative review achieving CLEAN pass on all PRs
- Agent-to-agent coordination via dedicated messaging threads

## Documentation

| Document | Description |
|----------|-------------|
| [Roles](docs/roles.md) | Detailed role definitions for all 4 team members |
| [Workflow](docs/workflow.md) | Complete development flow with diagrams |
| [Infrastructure](docs/infrastructure.md) | Tools, platforms, and communication setup |
| [Case Study: ClawFeed](docs/case-study-clawfeed.md) | Real project built by an agent team |
| [Getting Started](docs/getting-started.md) | Step-by-step guide to build your own agent team |

## Templates

Ready-to-use templates for projects built by agent teams:

- [`templates/CONTRIBUTING.md`](templates/CONTRIBUTING.md) — Contributing guidelines
- [`templates/PROCESS.md`](templates/PROCESS.md) — Development process document
- [`templates/prd-template.md`](templates/prd-template.md) — Product Requirements Document
- [`templates/tdd-template.md`](templates/tdd-template.md) — Technical Design Document
- [`templates/.github/pull_request_template.md`](templates/.github/pull_request_template.md) — PR template
- [`templates/.github/workflows/ci.yml`](templates/.github/workflows/ci.yml) — GitHub Actions CI

## Who Is This For?

- **Engineering leaders** exploring AI-augmented development
- **Solo founders** who want a full dev team at a fraction of the cost
- **AI enthusiasts** interested in multi-agent coordination
- **Teams** looking to add AI agents to their existing workflow

## Quick Start

1. Read the [Getting Started guide](docs/getting-started.md)
2. Copy the templates to your project
3. Set up your agents and communication channels
4. Run your first sprint

## Contributing

This is a living document. If you've built your own agent dev team and have insights to share, open an issue or PR.

## License

[MIT](LICENSE)

---

Built by [COCO](https://github.com/coco-xyz) — a team that practices what it preaches.
