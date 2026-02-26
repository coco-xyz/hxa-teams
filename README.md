# HxA Teams

> Team context protocol and templates for human × agent collaboration.

**hxa-teams** is a team context protocol and template library built on the **HxA (Human × Agent)** philosophy — humans and agents are equal team members, collaborating rather than commanding.

MCP solves Agent-Tool. A2A solves Agent-Agent. **hxa-teams solves Agent-Team** — how an agent understands the team it belongs to: who's on the team, what each member does, how they collaborate, and what the team's goals are.

### What It Is

- A set of **declarative team templates** (Markdown/YAML) — not a framework, not an SDK
- **Tool-agnostic** — works with Zylos, OpenClaw, Claude Code, Cursor, or any 2nd-gen persistent agent
- **Based on [SKILL.md](https://github.com/anthropics/skills)** format — cross-platform by default
- **For SMBs and solo founders** — designed for 2–20 person teams (humans + agents)

### What Problem It Solves

Today, deploying persistent AI agents in a team means:
- Hardcoding team info in system prompts (hard to maintain, version chaos)
- Letting agents figure out team context on their own (slow, error-prone)
- Writing piles of YAML config (developer-facing, not collaboration-facing)

90% of agent projects fail at operationalization, not capability. hxa-teams provides the **organizational blueprint** that makes agent teams actually work.

---

## How It Works

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│   User Input     │     │     Planner       │     │   Team Template   │
│                  │────▶│                   │────▶│                   │
│ - Team type      │     │ Reads template +  │     │ - Roles           │
│ - Agent count    │     │ inputs, outputs   │     │ - Workflow         │
│ - Resources      │     │ a team plan       │     │ - Communication   │
│ - Goals          │     │                   │     │ - Infrastructure   │
└─────────────────┘     └──────────────────┘     └──────────────────┘
                                │
                                ▼
                        ┌──────────────────┐
                        │    Team Plan      │
                        │                   │
                        │ Agent executes    │
                        │ autonomously      │
                        └──────────────────┘
```

1. **Choose a team type** — dev, sales, marketing, or bring your own
2. **Provide inputs** — how many agents, what tools/resources are available, what goals to achieve
3. **Planner generates a team plan** — role assignments, communication channels, workflow steps
4. **Agent self-leads execution** — the plan is actionable, not theoretical

## Team Templates

| Template | Status | Description |
|----------|--------|-------------|
| [Dev Team](teams/dev-team/) | **Complete** | Software development team with coordinator, developers, and QA |
| [Investment Team](teams/investment-team/) | **Complete** | Autonomous investment signal monitoring, research, modeling, and decision support |
| [Dev Team (BMAN)](teams/dev-team-bman/) | **Complete** | Technical development team building tools and platforms for investment operations |
| [Sales Team](teams/sales-team/) | Planned | Outbound sales pipeline with research, outreach, and follow-up |
| [Marketing Team](teams/marketing-team/) | Planned | Content creation pipeline with research, writing, review, and publishing |

## Communication: hxa-connect

Every team template is built around [**hxa-connect**](https://github.com/coco-xyz/hxa-connect) as the communication layer. hxa-connect provides:

- Agent-to-agent messaging
- Thread-based collaboration
- Shared artifacts (documents, code, data)

Each template explicitly defines **how agents communicate** — which channels to create, message formats, escalation paths. hxa-connect is the neural system; team templates are the playbook.

> **Current implementation:** hxa-connect runs on [BotsHub](https://github.com/coco-xyz/bots-hub). See [hxa-connect repo](https://github.com/coco-xyz/hxa-connect) for migration plan.

## Planner

The [`planner/`](planner/) directory contains the system prompt that turns a team template + user inputs into an actionable team plan. Give it to any capable LLM agent and it will:

1. Read the selected team template
2. Map available agents to roles
3. Set up communication channels via hxa-connect
4. Output a step-by-step execution plan
5. Begin autonomous execution

See [`planner/system-prompt.md`](planner/system-prompt.md) for the full prompt.

## Schema

Team templates follow a [standard schema](schema/team-template-spec.md) to ensure consistency and quality. This makes it possible for external contributors to submit new team types that work out of the box.

## Directory Structure

```
hxa-teams/
├── README.md                    # This file
├── planner/
│   └── system-prompt.md         # Planner prompt for generating team plans
├── schema/
│   └── team-template-spec.md    # Standard fields for team templates
├── teams/
│   ├── dev-team/                # Software development team
│   │   ├── README.md
│   │   ├── docs/                # Roles, workflow, infra, case studies
│   │   └── templates/           # PR template, PRD, TDD, CI, etc.
│   ├── investment-team/         # Investment operations team
│   │   ├── README.md
│   │   └── docs/                # Roles, workflow, infra
│   ├── dev-team-bman/           # Dev team for investment tooling
│   │   ├── README.md
│   │   └── docs/                # Roles, workflow, infra
│   ├── sales-team/              # Sales pipeline team (planned)
│   │   └── README.md
│   └── marketing-team/          # Content marketing team (planned)
│       └── README.md
└── docs/
    ├── getting-started.md       # How to use hxa-teams
    └── contributing.md          # How to contribute a new team template
```

## In the HxA Ecosystem

| Layer | Component | Role |
|-------|-----------|------|
| **Standard** | MCP, A2A, SKILL.md | Agent-Tool, Agent-Agent, skill format |
| **Communication** | [hxa-connect](https://github.com/coco-xyz/hxa-connect) | Agent messaging (the nervous system) |
| **Team Context** | **hxa-teams** (this repo) | Team templates — roles, workflows, quality gates |
| **Platform** | [hxa-workspace](https://github.com/coco-xyz/hxa-workspace) | ID, dashboard, admin, meet |

hxa-teams fills the **Agent-Team layer** — the missing piece between agent-level tooling and platform-level infrastructure.

## Quick Start

1. Pick a team template from [`teams/`](teams/)
2. Read the template's README and docs
3. Feed the [`planner/system-prompt.md`](planner/system-prompt.md) to your agent along with the template
4. Provide your inputs (agent count, resources, goals)
5. Let the agent execute the plan

## Contributing

We welcome new team templates. See [`docs/contributing.md`](docs/contributing.md) for the template spec and submission guidelines.

## License

[MIT](LICENSE)

---

Built by [COCO](https://github.com/coco-xyz) — making human × agent collaboration real.

> **Positioning & research:** [hxa-teams-positioning.md](https://jessie.coco.site/hxa-teams-positioning.md) | [hxa-teams-landscape-research.md](https://jessie.coco.site/hxa-teams-landscape-research.md)
