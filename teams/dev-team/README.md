# Dev Team Template

> Ship software 24/7 with an AI agent development team.

A team template for building **software development teams** where multiple AI agents work alongside a human product owner to ship real products, in parallel, around the clock.

## Team Structure

```
                    ┌─────────────────────────────────────┐
                    │        Human Product Owner           │
                    │   (requirements, final approval)     │
                    └──────────────┬──────────────────────┘
                                   │
                            requirements
                                   │
                    ┌──────────────▼──────────────────────┐
                    │       Coordinator Agent              │
                    │  (task splitting, architecture,      │
                    │   coordination, core development)    │
                    └──────┬──────────────────┬───────────┘
                           │                  │
                    Batch 1│           Batch 2│
                           │                  │
                    ┌──────▼─────┐     ┌──────▼──────┐
                    │ Developer  │     │ Developer   │
                    │ Agent(s)   │     │ Agent(s)    │
                    └──────┬─────┘     └──────┬──────┘
                           │                  │
                           └────────┬─────────┘
                                    │
                    ┌───────────────▼─────────────────────┐
                    │        QA / Review Agent             │
                    │  (automated review, QA, deployment)  │
                    └──────────────┬──────────────────────┘
                                   │
                              approved
                                   │
                    ┌──────────────▼──────────────────────┐
                    │        Human Product Owner           │
                    │      (final review + merge)          │
                    └─────────────────────────────────────┘
```

## Roles

| Role | Agent Count | Responsibilities |
|------|-------------|------------------|
| **Product Owner** | 1 (human) | Requirements, priorities, final approval |
| **Coordinator** | 1 | Task splitting, architecture, coordination, core dev |
| **Developer** | 1+ | Parallel development on assigned batches |
| **QA/Reviewer** | 1 | Automated code review (Codex loop), functional QA, deployment |

See [docs/roles.md](docs/roles.md) for detailed role definitions.

## hxa-connect Integration

Communication channels this template creates:

| Channel | Type | Purpose |
|---------|------|---------|
| `{project}-coordination` | Thread | Task assignment, progress tracking, blockers |
| DM: Coordinator → Developer | Direct | Batch assignments, technical questions |
| DM: Developer → QA | Direct | PR review requests |
| DM: QA → PO | Direct | Approval requests |

**Message patterns:**
- Task assignment: Coordinator posts to thread with batch details
- PR ready: Developer notifies QA via DM
- Review complete: QA posts result to thread
- Blocker: Anyone escalates in thread, Coordinator triages

## Workflow

1. PO defines requirements (PRD)
2. Coordinator splits into batches, assigns to developers
3. Developers work in parallel, submit PRs
4. QA runs automated review loop until CLEAN, then functional QA
5. PO does final review and merge

See [docs/workflow.md](docs/workflow.md) for the full development flow.

## Templates Included

| File | Purpose |
|------|---------|
| [CONTRIBUTING.md](templates/CONTRIBUTING.md) | Contributing guidelines for the project |
| [PROCESS.md](templates/PROCESS.md) | Development process document |
| [prd-template.md](templates/prd-template.md) | Product Requirements Document template |
| [tdd-template.md](templates/tdd-template.md) | Technical Design Document template |
| [.github/pull_request_template.md](templates/.github/pull_request_template.md) | PR template |

## Documentation

| Document | Description |
|----------|-------------|
| [Roles](docs/roles.md) | Detailed role definitions |
| [Workflow](docs/workflow.md) | Complete development flow with diagrams |
| [Infrastructure](docs/infrastructure.md) | Tools, platforms, and communication setup |
| [Case Study: Content Platform](docs/case-study-content-platform.md) | Real project built by an agent dev team |
| [Getting Started](docs/getting-started.md) | Step-by-step setup guide |

## Real Results (Pilot Project)

- Team setup (3 agents + infra): **1 day**
- CI, PR templates, branch protection: all created and merged by agents
- Codex CLI iterative review: CLEAN pass on all PRs
- Agent-to-agent coordination via hxa-connect threads
- 34 PRs merged in first week

See the [case study](docs/case-study-content-platform.md) for the full story.
