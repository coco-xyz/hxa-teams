# Infrastructure

Everything you need to run an agent dev team: AI subscriptions, agent platforms, communication, and GitHub configuration.

---

## AI Subscriptions

You need AI model access for both the agents and the review tooling.

### For Agent Runtime

| Provider | Plan | Purpose | Est. Cost |
|----------|------|---------|-----------|
| Anthropic (Claude) | Max or API | Primary agent runtime (Coordinator + Developers) | $100-200/mo per agent |
| OpenAI (ChatGPT) | Plus or API | Codex CLI for automated code review | $20-200/mo |

**Notes:**
- Claude Max provides generous usage for persistent agents running on platforms like Zylos
- Codex CLI (powered by OpenAI) is used specifically for iterative code review
- Using different AI providers for development vs. review gives you complementary perspectives

### Recommended Configuration

| Role | AI Model | Reasoning |
|------|----------|-----------|
| Coordinator Agent | Claude (Anthropic) | Strong at architecture, coordination, long-context reasoning |
| Developer Agents | Claude (Anthropic) | Consistent code style across the team |
| QA / Review Agent | Mixed (GPT for Codex, Claude or GPT for manual review) | Different model catches different issues |
| Codex CLI | OpenAI (GPT) | Purpose-built for code review |

---

## Agent Platforms

Agents need a runtime environment that provides code execution, persistence, and communication.

### Option 1: Zylos (Recommended for Coordinator)

[Zylos](https://github.com/zylos-ai/zylos-core) is an autonomous agent platform built for persistent AI agents.

**Capabilities:**
- Claude Code integration
- Persistent memory across sessions
- Task scheduler (self-wake for deferred work)
- External communication (Lark, Telegram, GitHub, agent-to-agent messaging)
- Browser automation
- 24/7 uptime on cloud servers

**Best for:** Coordinator Agent — needs always-on availability, scheduling, and multi-channel communication.

**Deployment:**
- Cloud server (e.g., AWS, GCP, Hetzner) for the Coordinator
- Local machine (Mac Mini, Linux box) for Developer agents

### Option 2: OpenClaw

[OpenClaw](https://github.com/openclaw/openclaw) is a local-first agent platform with a skill ecosystem.

**Capabilities:**
- Local execution environment
- ClawHub skills marketplace
- Browser automation
- GPU access (for local ML tasks)

**Best for:** QA Agent — benefits from local execution for testing and browser-based QA.

### Option 3: Claude Code (Standalone)

For simpler setups, you can run Claude Code directly without a platform wrapper.

**Capabilities:**
- Code execution
- File system access
- Git operations

**Limitations:**
- No persistent memory between sessions
- No scheduling
- No built-in communication

**Best for:** Getting started quickly or adding a Developer agent without full platform setup.

### Minimum Viable Setup

| Component | Option |
|-----------|--------|
| 1 cloud server | Coordinator Agent (Zylos) |
| 1 local machine | Developer Agent + QA Agent + Codex CLI |

You can run multiple agents on the same machine — they operate in separate environments and don't interfere with each other.

---

## Communication

### Agent-to-Agent Communication

Agents need a channel to coordinate without involving the human PO.

**Recommended: [BotsHub](https://github.com/coco-xyz/bots-hub)**

BotsHub provides threaded communication between AI agents:
- Create a dedicated thread for your dev team
- Agents post task assignments, progress updates, and coordination messages
- Structured format so agents can parse each other's messages

**Alternative:** Any messaging system with API access (Slack bots, Discord bots, custom webhook-based system).

### Human-Agent Communication

The PO needs a channel to interact with the team.

| Platform | Use Case | Setup |
|----------|----------|-------|
| **Lark** | Team chat, PR notifications, requirement posting | Create a group; agents join as bots |
| **Slack** | Same as Lark | Create a channel; agents join via Slack API |
| **Discord** | Same as Lark | Create a server; agents join as bots |
| **Telegram** | Quick messages, 1:1 with Coordinator | Agent runs as a Telegram bot |

**Recommended flow:**
1. PO posts requirements in team chat
2. Coordinator acknowledges and begins work
3. Coordinator posts PR summaries and status updates to team chat
4. PO reviews and merges on GitHub

### GitHub Communication

All code-level communication happens on GitHub:
- **PR comments:** Review feedback, technical discussions
- **Issues:** Task tracking, bug reports
- **PR descriptions:** Requirement understanding, change summary

---

## GitHub Setup

### Repository Configuration

#### Branch Protection Rules

Set up branch protection on `main` (and `develop` if using):

```
main branch protection:
  ✅ Require pull request before merging
  ✅ Require approvals: 1
  ✅ Dismiss stale reviews when new commits are pushed
  ✅ Require status checks to pass (CI)
  ✅ Require branches to be up to date
```

You can configure this via the GitHub API:

```bash
gh api repos/OWNER/REPO/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":["ci"]}' \
  --field required_pull_request_reviews='{"required_approving_review_count":1,"dismiss_stale_reviews":true}' \
  --field enforce_admins=false \
  --field restrictions=null
```

#### Collaborator Access

Add each agent's GitHub account as a collaborator:

```bash
gh api repos/OWNER/REPO/collaborators/agent-github-username \
  --method PUT \
  --field permission=write
```

Each agent needs its own GitHub account with:
- Unique email address (e.g., `agent-1@example.com`)
- Personal Access Token (PAT) with `repo` scope
- Git identity configured (`user.name` and `user.email`)

#### CI Pipeline

Set up GitHub Actions for automated checks. See [`templates/.github/workflows/ci.yml`](../templates/.github/workflows/ci.yml) for a starter workflow.

Minimum CI checks:
- Linting
- Unit tests
- Security audit (`npm audit` / `pip audit`)

### GitHub Account Setup Per Agent

```bash
# Configure git identity (run in each agent's environment)
git config --global user.name "Agent Name"
git config --global user.email "agent-1@example.com"

# Set up authentication
# Option A: Personal Access Token in git URL
git remote set-url origin https://x-access-token:TOKEN@github.com/OWNER/REPO.git

# Option B: GitHub CLI
gh auth login --with-token < token.txt
```

---

## Codex CLI Setup

Codex CLI is the automated review engine. Install it on the machine where the QA Agent runs.

### Installation

```bash
# Install Codex CLI (requires Node.js)
npm install -g @openai/codex

# Verify installation
codex --version
```

### Configuration

```bash
# Set OpenAI API key (or use ChatGPT Plus login)
export OPENAI_API_KEY=your-api-key
```

### Usage in Review Flow

```bash
# Review a PR diff
codex review --diff <pr-diff-file>

# Or review specific files
codex review src/feature.js src/feature.test.js
```

The QA Agent automates this loop:
1. Fetch PR diff
2. Run `codex review`
3. If issues found → post comments on PR, notify developer
4. Developer fixes → QA Agent re-runs review
5. Repeat until CLEAN

---

## Infrastructure Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Cloud Server                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │  Coordinator Agent (Zylos)                        │  │
│  │  - Claude Code runtime                            │  │
│  │  - Persistent memory                              │  │
│  │  - Scheduler (PR monitoring, notifications)       │  │
│  │  - Communication bridge (Lark/Slack/Telegram)     │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                          │
                          │ BotsHub / API
                          │
┌─────────────────────────────────────────────────────────┐
│                    Local Machine                         │
│  ┌─────────────────┐  ┌──────────────────────────────┐  │
│  │ Developer Agent  │  │  QA Agent (OpenClaw)         │  │
│  │ (Zylos)         │  │  - Codex CLI                  │  │
│  │ - Claude Code   │  │  - Browser automation         │  │
│  │ - Local dev env │  │  - Test runner                │  │
│  └─────────────────┘  └──────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Git push / PR
                          │
┌─────────────────────────────────────────────────────────┐
│                      GitHub                              │
│  - Repository with branch protection                    │
│  - GitHub Actions CI                                     │
│  - PR reviews and approvals                              │
│  - Issue tracking                                        │
└─────────────────────────────────────────────────────────┘
                          │
                          │ Notifications
                          │
┌─────────────────────────────────────────────────────────┐
│                 Team Communication                       │
│  - Lark / Slack / Discord (human-agent)                 │
│  - BotsHub (agent-to-agent)                              │
└─────────────────────────────────────────────────────────┘
```

---

## Cost Estimate

| Item | Monthly Cost | Notes |
|------|-------------|-------|
| Claude Max (per agent) | $100-200 | Coordinator + Developer agents |
| ChatGPT Plus | $20 | For Codex CLI |
| Cloud server | $20-50 | For Coordinator (always-on) |
| Local machine | One-time | Mac Mini, Linux box, etc. |
| GitHub | Free | Public repos; private repos need Pro for branch protection |
| Communication | Free-$10 | Lark/Slack free tier usually sufficient |
| **Total** | **$240-660/mo** | **For a 3-agent + 1 human team** |

Compare this to hiring 2-3 developers: the agent team costs roughly the same as one developer's monthly coffee budget.
