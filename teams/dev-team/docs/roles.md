# Roles

The agent dev team has **4 roles**: one coordinator agent, one or more developer agents, one QA/review agent, and one human product owner. Each role has distinct responsibilities and required capabilities.

> In our real implementation, we use named agents (Jessie, Lucy, Lisa) + a human PO (Kevin). Throughout this document, we use generic labels (Agent A, Agent B, Agent C) so you can map them to your own setup.

---

## Coordinator Agent (Agent A)

**Primary function:** Task decomposition, architecture, core development, and cross-agent coordination.

### Responsibilities

- Receive requirements from the Product Owner and break them into actionable task batches
- Create GitHub issues for each task
- Assign batches to developer agents (including self)
- Make architecture decisions and design core components
- Develop assigned batches (the coordinator is also a developer)
- Monitor PR status and push notifications to the team communication channel
- Coordinate across agents — resolve blockers, answer questions, re-assign work
- Write and maintain documentation (PRDs, contributing guides, process docs)
- Handle long-running automation tasks (CI monitoring, dependency updates, scheduled checks)

### Required Capabilities

| Capability | Why |
|------------|-----|
| Persistent memory | Maintains context across sessions — knows project history, decisions, and team state |
| Scheduler / cron | Can self-wake for deferred tasks (PR monitoring, periodic checks, follow-ups) |
| External communication | Can message the team chat, post in agent-to-agent threads, and interact with GitHub |
| Code execution | Full development environment to write, test, and commit code |
| Always-on availability | 24/7 uptime so it can respond to events and coordinate asynchronously |

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a cloud server (for always-on availability)
- Any persistent agent platform with memory + scheduling + communication

---

## Developer Agent (Agent B, B2, ...)

**Primary function:** Execute assigned task batches independently.

### Responsibilities

- Accept batch assignments from the Coordinator
- Develop features on isolated feature branches
- Submit PRs following the project's PR template
- Respond to review feedback (Codex + Agent QA) and iterate until approved
- Collaborate with the Coordinator on design questions

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code execution | Full local or remote dev environment |
| Git workflow | Branch, commit, push, open PRs, respond to review comments |
| Communication | Can receive task assignments and report progress |

### Notes

- Developer agents work in parallel with the Coordinator and other developers
- Each developer works on a separate batch to avoid merge conflicts
- More developer agents = more parallel throughput (but coordinate carefully to avoid conflicts)

### Recommended Platforms

- [Zylos](https://github.com/zylos-ai/zylos-core) on a local machine (e.g., Mac Mini)
- [OpenClaw](https://github.com/openclaw/openclaw) or similar agent platforms
- Any agent with a code execution environment and GitHub access

---

## QA / Review Agent (Agent C)

**Primary function:** Code review, quality assurance, CI/CD maintenance.

### Responsibilities

- Review all PRs from all developers (code quality + functional correctness)
- Manage the Codex CLI review loop: run iterative reviews until CLEAN
- Run integration tests in a local environment
- Maintain CI/CD pipelines (GitHub Actions)
- Maintain contributing docs, PR templates, and process documentation
- Run dependency security audits (`npm audit`, Dependabot triage)
- Perform browser-based QA testing on staging environments
- Execute production smoke tests after deployment

### Required Capabilities

| Capability | Why |
|------------|-----|
| Code execution | Run tests, linting, security audits locally |
| Codex CLI access | Iterative automated code review |
| Browser automation | QA testing on staging/production |
| GitHub access | Post review comments, approve/request changes |

### Why a Separate Review Agent?

- **Independence:** The reviewer didn't write the code, so it catches different issues
- **Different AI perspective:** Using a different AI model (e.g., GPT-based Codex for review, Claude-based agents for development) provides complementary coverage
- **Double gate:** Codex CLI automated review + agent manual review = two layers of quality control

### Recommended Platforms

- [OpenClaw](https://github.com/openclaw/openclaw) on a local machine (access to Codex CLI)
- Any agent with code execution, browser, and GitHub access

---

## Human Product Owner

**Primary function:** Requirements, prioritization, final approval.

### Responsibilities

- Define requirements and priorities
- Review and approve PRDs before development starts
- Final PR review and merge (in order, handling rebase conflicts)
- Architecture decisions and technical direction
- Resource allocation and team composition
- Staging acceptance testing
- Production deployment approval

### Interaction Model

The PO interacts with the team through:

1. **Team chat** (Lark, Slack, Discord) — Post requirements, receive PR summaries, make quick decisions
2. **Direct messages** — Strategic discussions with the Coordinator
3. **GitHub** — Review PRs, approve, merge
4. **Scheduled meetings** — Optional; most coordination is async

### How Much Human Involvement?

The goal is to minimize PO involvement to high-value decisions:

| Activity | PO Involvement |
|----------|---------------|
| Writing requirements | High (this is the PO's main job) |
| Task decomposition | None (Coordinator handles) |
| Development | None (agents handle) |
| Code review | Low (agents handle; PO does final spot-check) |
| Merge | Medium (PO merges in order) |
| Incident response | Low (agents detect + fix; PO notified) |

---

## Role Interaction Map

```
                  Requirements
    PO ──────────────────────────────► Coordinator (A)
    ▲                                      │
    │                                      │ assigns batches
    │ PR approval                          │
    │ + merge                    ┌─────────┴─────────┐
    │                            ▼                   ▼
    │                     Developer (B)        Coordinator (A)
    │                      Batch 1              Batch 2
    │                            │                   │
    │                            └─────────┬─────────┘
    │                                      │ submit PRs
    │                                      ▼
    │                              QA / Review (C)
    │                           Codex loop + QA
    │                                      │
    └──────────────────────────────────────┘
                  approved PRs
```

## Scaling the Team

| Team Size | Configuration | Best For |
|-----------|--------------|----------|
| Minimum (3) | 1 Coordinator + 1 QA + 1 PO | Solo projects, early-stage startups |
| Standard (4) | 1 Coordinator + 1 Developer + 1 QA + 1 PO | Small-to-medium projects |
| Scaled (5+) | 1 Coordinator + 2-3 Developers + 1 QA + 1 PO | Larger projects with many parallel workstreams |

> Adding more developer agents increases parallel capacity but also increases coordination overhead. Start small and scale based on need.
