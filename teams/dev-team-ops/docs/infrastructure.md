# Infrastructure

Everything you need to run the ops dev team: AI subscriptions, agent platforms, development tools, communication, and deployment setup.

---

## AI Subscriptions

You need AI model access for the agents and their development tasks.

### For Agent Runtime

| Provider | Plan | Purpose | Est. Cost |
|----------|------|---------|-----------|
| Anthropic (Claude) | Max or API | Primary agent runtime (all agents) | $100-200/mo per agent |

**Notes:**
- Claude Max provides generous usage for persistent agents running on platforms like Zylos
- Development tasks (code generation, design, deployment) are well-suited to Claude's capabilities
- All three agents can use the same AI provider for consistent code style

### Recommended Configuration

| Role | AI Model | Reasoning |
|------|----------|-----------|
| Dev & UI Lead | Claude (Anthropic) | Strong at coordination, design reasoning, deployment management |
| Developer | Claude (Anthropic) | Excellent code generation across full-stack technologies |
| Security Auditor | Claude (Anthropic) | Deep analytical reasoning for security review |

---

## Agent Platforms

Each agent needs a runtime environment with persistence, communication, and code execution.

### Recommended: Zylos (All Agents)

[Zylos](https://github.com/zylos-ai/zylos-core) is the recommended platform for all dev team agents:

**Capabilities:**
- Claude Code integration
- Persistent memory across sessions
- Task scheduler (self-wake for monitoring, follow-ups)
- External communication (hxa-connect, Telegram, deployment APIs)
- Code execution (full development environment)
- Browser automation (for UI testing, Figma interaction)
- 24/7 uptime on cloud servers

**Deployment:**

| Agent | Deployment | Why |
|-------|-----------|-----|
| Dev Lead | Cloud server | Always-on for deployment monitoring, service ops, cross-team coordination |
| Developer | Cloud or local | Development environment — can be local if latency is acceptable |
| Security Auditor | Cloud server (shared with investment team role) | Security review on demand, periodic scans |

### Minimum Viable Setup

| Component | Option |
|-----------|--------|
| 1 cloud server (shared) | All 3 agents on a single server with PM2 process management |
| hxa-connect instance | Your hxa-connect hub instance |

Agents share the server but operate in separate Zylos instances. The dev team agents may share the same server with the investment team agents.

---

## Development Tools

### Source Control

| Tool | Purpose | Setup |
|------|---------|-------|
| Git | Version control | Standard — all agents configure git identity |
| GitHub | Repository hosting, PRs | Agents need GitHub accounts with repo access |

**Git identity per agent:**
```bash
# Each agent configures its own identity
git config user.name "Agent Name"
git config user.email "agent@example.com"
```

### Development Environment

| Tool | Purpose | Used By |
|------|---------|---------|
| Node.js | JavaScript/TypeScript runtime | Developer |
| Python | Data pipelines, scripting | Developer |
| Docker | Containerized deployment | Dev Lead (deployment), Developer (development) |
| PM2 | Process management | Dev Lead (service ops) |

### Design Tools

| Tool | Purpose | Used By |
|------|---------|---------|
| Figma | UI design, component specifications | Dev Lead |
| Browser | Design review, UI testing | Dev Lead, Product Owner |

### CI/CD

| Tool | Purpose | Setup |
|------|---------|-------|
| GitHub Actions | Automated checks on PR | Lint, test, build |
| Docker / PM2 | Deployment | Managed by Dev Lead |

---

## Communication

### Agent-to-Agent Communication (hxa-connect)

**Instance:** Your hxa-connect hub instance
**Organization:** Your organization name

Channels to create:

| Channel | Members | Purpose |
|---------|---------|---------|
| `#dev` | Developer, Dev Lead | Development collaboration, code review, deployment notifications |
| `#general` | All | Cross-team announcements, product updates, acceptance |

**Note:** `#dev` and `#general` are shared with the investment team's hxa-connect setup. The same channels serve both teams.

**Direct message pairs:**
- Dev Lead ↔ Developer (task assignment, design handoff)
- Security Auditor ↔ Developer (security findings, code quality)

**Thread structure:**
Each development task or feature gets its own thread within `#dev`, with attached artifacts (designs, code links, review results).

### Human-Agent Communication

The Product Owner interacts with the team through:

| Platform | Use Case | Setup |
|----------|----------|-------|
| **hxa-connect** | Primary: product updates, acceptance, announcements | Product Owner joins the hub directly |
| **Telegram** | Quick confirmations, mobile access | Dev Lead relays via Telegram bot |

**Recommended flow:**
1. Dev Lead posts requirement summary and design to `#general`
2. Product Owner confirms via hxa-connect or Telegram relay
3. Development proceeds
4. Dev Lead posts deployment notification to `#general`
5. Product Owner reviews and accepts

---

## Deployment Infrastructure

### Server Configuration

| Component | Purpose | Managed By |
|-----------|---------|-----------|
| Application servers | Host deployed services (dashboards, APIs, tools) | Dev Lead |
| Reverse proxy (Nginx/Caddy) | HTTPS termination, routing | Dev Lead |
| Cloudflare Tunnel | Secure external access without port exposure | Dev Lead |
| PM2 | Process management for Node.js services | Dev Lead |
| Docker | Containerized deployments for isolated services | Dev Lead |

### Monitoring

| Tool | Purpose | Owner |
|------|---------|-------|
| PM2 monitoring | Process health, restart on failure | Dev Lead |
| Health endpoint checks | Service availability verification | Dev Lead (scheduled) |
| Error logging | Application error tracking | Developer (instrumentation), Dev Lead (monitoring) |

---

## Infrastructure Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Cloud Server                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Dev Lead (Zylos) — Dev & UI Lead                 │  │
│  │  - Requirement gathering, UI design (Figma)       │  │
│  │  - Deployment pipeline, service ops               │  │
│  │  - Service health monitoring (scheduler)          │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Developer (Zylos)                                │  │
│  │  - Full-stack development environment             │  │
│  │  - Data pipeline development                      │  │
│  │  - Git workflow (branch, commit, push)            │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Security Auditor (Zylos)                         │  │
│  │  - Code security review                           │  │
│  │  - Dependency vulnerability scanning              │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Deployed Services                                │  │
│  │  - Dashboards, APIs, tools (PM2 / Docker)         │  │
│  │  - Reverse proxy (Nginx/Caddy)                    │  │
│  │  - Cloudflare Tunnel                              │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                          │
                          │ hxa-connect
                          │
┌─────────────────────────────────────────────────────────┐
│                   hxa-connect Hub                        │
│  - #dev (Developer, Dev Lead)                           │
│  - #general (all)                                       │
│  - DM channels                                          │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Git push / deploy
                          │
┌─────────────────────────────────────────────────────────┐
│                      GitHub                              │
│  - Repository with branch protection                    │
│  - GitHub Actions CI (lint, test, build)                │
│  - PR reviews                                           │
└─────────────────────────────────────────────────────────┘
```

---

## Cost Estimate

| Item | Monthly Cost | Notes |
|------|-------------|-------|
| Claude Max (per agent) | $100-200 | 3 agents = $300-600/mo |
| Cloud server | $40-100 | Shared with investment team agents |
| GitHub | Free | Public repos; private repos need Pro for branch protection |
| Figma | Free-$15 | Free tier usually sufficient for agent-generated designs |
| hxa-connect (self-hosted) | $0 | Self-hosted on your server |
| **Total** | **$340-715/mo** | **For a 3-agent + 1 human team** |

**Note:** If running alongside the investment team on the same server, the cloud server cost is shared — do not double-count. The combined cost for both teams (7 agent roles across 4 agents) is roughly the same as running 4 agents on one server.
