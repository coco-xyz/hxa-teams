# Infrastructure

Everything you need to run BMAN's dev team: AI subscriptions, agent platforms, development tools, communication, and deployment setup.

---

## AI Subscriptions

You need AI model access for the agents and their development tasks.

### For Agent Runtime

| Provider | Plan | Purpose | Est. Cost |
|----------|------|---------|-----------|
| Anthropic (Claude) | Max or API | Primary agent runtime (Roey, Joey, Zylos_ABCDE) | $100-200/mo per agent |

**Notes:**
- Claude Max provides generous usage for persistent agents running on platforms like Zylos
- Development tasks (code generation, design, deployment) are well-suited to Claude's capabilities
- All three agents can use the same AI provider for consistent code style

### Recommended Configuration

| Role | AI Model | Reasoning |
|------|----------|-----------|
| Dev & UI Lead (Roey) | Claude (Anthropic) | Strong at coordination, design reasoning, deployment management |
| Developer (Joey) | Claude (Anthropic) | Excellent code generation across full-stack technologies |
| Security Auditor (Zylos_ABCDE) | Claude (Anthropic) | Deep analytical reasoning for security review |

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
| Roey | Cloud server | Always-on for deployment monitoring, service ops, cross-team coordination |
| Joey | Cloud or local | Development environment — can be local if latency is acceptable |
| Zylos_ABCDE | Cloud server (shared with investment team role) | Security review on demand, periodic scans |

### Minimum Viable Setup

| Component | Option |
|-----------|--------|
| 1 cloud server (shared) | All 3 agents on a single server with PM2 process management |
| hxa-connect instance | `boot.hxa.net` (BotsHub) |

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
| Node.js | JavaScript/TypeScript runtime | Joey |
| Python | Data pipelines, scripting | Joey |
| Docker | Containerized deployment | Roey (deployment), Joey (development) |
| PM2 | Process management | Roey (service ops) |

### Design Tools

| Tool | Purpose | Used By |
|------|---------|---------|
| Figma | UI design, component specifications | Roey |
| Browser | Design review, UI testing | Roey, BMAN |

### CI/CD

| Tool | Purpose | Setup |
|------|---------|-------|
| GitHub Actions | Automated checks on PR | Lint, test, build |
| Docker / PM2 | Deployment | Managed by Roey |

---

## Communication

### Agent-to-Agent Communication (hxa-connect)

**Instance:** `boot.hxa.net` (BotsHub)
**Organization:** `bman`

Channels to create:

| Channel | Members | Purpose |
|---------|---------|---------|
| `#dev` | Joey, Roey | Development collaboration, code review, deployment notifications |
| `#general` | All | Cross-team announcements, product updates, acceptance |

**Note:** `#dev` and `#general` are shared with the investment team's hxa-connect setup. The same channels serve both teams.

**Direct message pairs:**
- Roey ↔ Joey (task assignment, design handoff)
- Zylos_ABCDE ↔ Joey (security findings, code quality)

**Thread structure:**
Each development task or feature gets its own thread within `#dev`, with attached artifacts (designs, code links, review results).

### Human-Agent Communication

The Product Owner (BMAN) interacts with the team through:

| Platform | Use Case | Setup |
|----------|----------|-------|
| **hxa-connect** | Primary: product updates, acceptance, announcements | BMAN joins `boot.hxa.net` directly |
| **Telegram** | Quick confirmations, mobile access | Roey relays via Telegram bot |

**Recommended flow:**
1. Roey posts requirement summary and design to `#general`
2. BMAN confirms via hxa-connect or Telegram relay
3. Development proceeds
4. Roey posts deployment notification to `#general`
5. BMAN reviews and accepts

---

## Deployment Infrastructure

### Server Configuration

| Component | Purpose | Managed By |
|-----------|---------|-----------|
| Application servers | Host deployed services (dashboards, APIs, tools) | Roey |
| Reverse proxy (Nginx/Caddy) | HTTPS termination, routing | Roey |
| Cloudflare Tunnel | Secure external access without port exposure | Roey |
| PM2 | Process management for Node.js services | Roey |
| Docker | Containerized deployments for isolated services | Roey |

### Monitoring

| Tool | Purpose | Owner |
|------|---------|-------|
| PM2 monitoring | Process health, restart on failure | Roey |
| Health endpoint checks | Service availability verification | Roey (scheduled) |
| Error logging | Application error tracking | Joey (instrumentation), Roey (monitoring) |

---

## Infrastructure Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Cloud Server                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Roey (Zylos) — Dev & UI Lead                     │  │
│  │  - Requirement gathering, UI design (Figma)       │  │
│  │  - Deployment pipeline, service ops               │  │
│  │  - Service health monitoring (scheduler)          │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Joey (Zylos) — Developer                         │  │
│  │  - Full-stack development environment             │  │
│  │  - Data pipeline development                      │  │
│  │  - Git workflow (branch, commit, push)            │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Zylos_ABCDE (Zylos) — Security Auditor           │  │
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
                          │ hxa-connect (boot.hxa.net)
                          │
┌─────────────────────────────────────────────────────────┐
│                   hxa-connect Hub                        │
│  - #dev (Joey, Roey)                                    │
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
| hxa-connect (BotsHub) | $0 | Self-hosted at boot.hxa.net |
| **Total** | **$340-715/mo** | **For a 3-agent + 1 human team** |

**Note:** If running alongside the investment team on the same server, the cloud server cost is shared — do not double-count. The combined cost for both teams (7 agent roles across 4 agents) is roughly the same as running 4 agents on one server.
