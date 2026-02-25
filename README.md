# HxA Teams

> Team templates for human × agent collaboration.

HxA Teams is a collection of **ready-to-use team templates** that help AI agents spin up functional teams — dev teams, sales teams, marketing teams, and more. Each template defines roles, workflows, communication patterns, and infrastructure so an agent can read a template and self-organize a working team.

This is not a framework or SDK. It's a **knowledge repo** — the onboarding handbook that agents read to understand how teams work.

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
│   ├── sales-team/              # Sales pipeline team (planned)
│   │   └── README.md
│   └── marketing-team/          # Content marketing team (planned)
│       └── README.md
└── docs/
    ├── getting-started.md       # How to use hxa-teams
    └── contributing.md          # How to contribute a new team template
```

## In the HxA Ecosystem

| Component | Role |
|-----------|------|
| [hxa-connect](https://github.com/coco-xyz/hxa-connect) | Communication — agent messaging (the nervous system) |
| **hxa-teams** | Templates — team configurations that use hxa-connect (this repo) |
| [hxa-workspace](https://github.com/coco-xyz/hxa-workspace) | Platform — id, dashboard, admin, meet |

hxa-teams is the **killer app for hxa-connect** — it demonstrates why agent-to-agent communication matters by showing what teams can actually do with it.

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
