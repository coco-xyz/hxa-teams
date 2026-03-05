# Getting Started

A step-by-step guide to building your own AI agent development team. Estimated setup time: 4-8 hours.

> **New to HxA Teams?** Start with the [top-level Getting Started guide](../../../docs/getting-started.md) for a general overview of choosing a template, mapping agents to roles, and setting up communication. This page covers dev-team-specific setup.

---

## Prerequisites

Before you begin, you need:

- [ ] A GitHub account (for the human PO)
- [ ] 1-2 AI subscriptions (Claude Max/API for agents, ChatGPT Plus for Codex CLI)
- [ ] At least one machine to run agents (cloud server, local machine, or both)
- [ ] A team communication platform (Lark, Slack, Discord, or Telegram)
- [ ] A project to work on (ideally greenfield or well-scoped)

---

## Step 1: Choose Your Agents

### Minimum Team (3 members)

| Role | Platform | Where to Run |
|------|----------|-------------|
| Coordinator Agent | Zylos or Claude Code | Cloud server (recommended) or local |
| QA / Review Agent | OpenClaw or Claude Code | Local machine |
| Human Product Owner | — | Your laptop |

### Standard Team (4 members)

| Role | Platform | Where to Run |
|------|----------|-------------|
| Coordinator Agent | Zylos | Cloud server |
| Developer Agent | Zylos or Claude Code | Local machine |
| QA / Review Agent | OpenClaw | Local machine |
| Human Product Owner | — | Your laptop |

### Platform Setup

**For Zylos agents:**
```bash
# On your server or local machine
git clone https://github.com/zylos-ai/zylos-core.git
cd zylos
# Follow Zylos setup instructions
```

**For OpenClaw agents:**
```bash
# On your local machine
git clone https://github.com/openclaw/openclaw.git
cd openclaw
# Follow OpenClaw setup instructions
```

**For standalone Claude Code:**
```bash
# Install Claude Code CLI
npm install -g @anthropic-ai/claude-code
```

---

## Step 2: Set Up Communication

### Agent-to-Agent Channel

Set up a dedicated thread/channel where agents coordinate without human involvement.

**Option A: hxa-connect (recommended)**
```bash
# Set up hxa-connect
git clone https://github.com/coco-xyz/hxa-connect.git
# Follow setup instructions
# Create a team thread for your project
```

**Option B: Slack/Discord Bot Channel**
- Create a private channel for agent coordination
- Set up bot tokens for each agent
- Agents post structured messages (task assignments, progress updates)

### Human-Agent Channel

Set up a team chat where the PO can interact with agents.

1. Create a group/channel on your preferred platform (Lark, Slack, Discord)
2. Add each agent as a bot member
3. The Coordinator agent should be able to:
   - Receive messages from the PO
   - Post PR summaries and status updates
   - Tag the PO when decisions are needed

---

## Step 3: Set Up GitHub

### Create GitHub Accounts for Agents

Each agent needs its own GitHub account:

1. Create a new GitHub account (e.g., `your-org-agent-a`)
2. Use a dedicated email (e.g., `agent-a@example.com`)
3. Generate a Personal Access Token (PAT) with `repo` scope
4. Configure git identity in the agent's environment:

```bash
git config user.name "Agent A"
git config user.email "agent-a@example.com"

# Set up token-based authentication
git remote set-url origin https://x-access-token:TOKEN@github.com/OWNER/REPO.git
```

### Add Agents as Repository Collaborators

```bash
# Using GitHub CLI
gh api repos/OWNER/REPO/collaborators/agent-a-username --method PUT --field permission=write
gh api repos/OWNER/REPO/collaborators/agent-b-username --method PUT --field permission=write
gh api repos/OWNER/REPO/collaborators/agent-c-username --method PUT --field permission=write
```

### Configure Branch Protection

```bash
# Protect main branch
gh api repos/OWNER/REPO/branches/main/protection \
  --method PUT \
  --field required_status_checks='{"strict":true,"contexts":["ci"]}' \
  --field required_pull_request_reviews='{"required_approving_review_count":1,"dismiss_stale_reviews":true}' \
  --field enforce_admins=false \
  --field restrictions=null
```

### Add Templates

Copy the templates from this repo into your project:

```bash
# From the agent-dev-team repo
cp templates/CONTRIBUTING.md your-project/CONTRIBUTING.md
cp templates/PROCESS.md your-project/docs/PROCESS.md
cp -r templates/.github your-project/.github
```

### Set Up CI

Copy the CI workflow:

```bash
mkdir -p your-project/.github/workflows
cp templates/.github/workflows/ci.yml your-project/.github/workflows/ci.yml
```

Edit the workflow to match your project's tech stack (Node.js, Python, Go, etc.).

---

## Step 4: Define Roles

Write a team document (like the one in this repo) that clearly defines:

1. **Who does what** — Map agents to roles (Coordinator, Developer, QA)
2. **Communication channels** — Where each type of message goes
3. **Review process** — The exact steps from PR submission to merge
4. **Escalation rules** — When agents should ask the PO vs. decide themselves

Share this document with all agents. Each agent should have it in their persistent memory or project context.

### Role Assignment Template

```markdown
# Team Roles

## Coordinator (Agent A)
- GitHub: agent-a-username
- Responsibilities: task decomposition, coordination, core development
- Communication: monitors team chat, posts in agent thread

## Developer (Agent B)
- GitHub: agent-b-username
- Responsibilities: develop assigned batches, submit PRs
- Communication: receives tasks from agent thread

## QA / Review (Agent C)
- GitHub: agent-c-username
- Responsibilities: Codex review, QA testing, CI maintenance
- Communication: reviews all PRs, posts results in agent thread

## Product Owner (You)
- GitHub: your-username
- Responsibilities: requirements, PRD approval, final merge
- Communication: team chat for requirements, GitHub for reviews
```

---

## Step 5: Run Your First Sprint

### Sprint 0: Infrastructure (Recommended First Sprint)

Don't start with feature development. Use the first sprint to set up infrastructure:

| Task | Owner | Priority |
|------|-------|----------|
| GitHub Actions CI | QA Agent | High |
| PR template | Coordinator | High |
| CONTRIBUTING.md | Coordinator | High |
| Branch protection | Coordinator / PO | High |
| Test Codex review loop | QA Agent | High |
| Test agent-to-agent communication | All agents | Medium |

This validates that the entire pipeline works before you build features.

### Sprint 1: First Feature

Pick a small, well-defined feature:

1. **PO writes requirement** in team chat
2. **Coordinator writes PRD** and submits as PR
3. **PO reviews and approves PRD**
4. **Coordinator splits into batches** (if applicable)
5. **Agents develop in parallel**
6. **QA runs Codex review loop** on each PR
7. **QA does manual review**
8. **PO merges**

**Tip:** For the first feature sprint, keep batches simple. One batch per agent. Complexity can increase as the team finds its rhythm.

---

## Common Pitfalls

### 1. Skipping the PRD

**Problem:** Jumping straight to code without a PRD leads to scope creep and misaligned implementations.

**Fix:** Always write a PRD for features. Bug fixes can skip it, but anything non-trivial needs documented requirements.

### 2. Agents Working on Overlapping Files

**Problem:** Two agents editing the same files creates merge conflicts.

**Fix:** The Coordinator should design batch splits to minimize file overlap. If overlap is unavoidable, serialize those tasks.

### 3. Not Testing Communication Early

**Problem:** You discover agents can't reach each other only after starting development.

**Fix:** Sprint 0 should include a communication test. Have each agent post a message in the agent thread and verify all agents can read it.

### 4. PO Bottleneck on Merges

**Problem:** PRs pile up waiting for PO review.

**Fix:** Set up notifications so the PO knows immediately when a PR is ready. The agent team's speed means PRs arrive faster than a human team — the PO needs to adapt.

### 5. Too Many Agents Too Soon

**Problem:** Adding 5 agents when you haven't validated the workflow with 3.

**Fix:** Start with the minimum team (Coordinator + QA + PO). Add Developer agents once the workflow is stable.

### 6. Ignoring Merge Order

**Problem:** Merging PRs out of order causes cascading rebase conflicts.

**Fix:** The PO should merge PRs in dependency order. The Coordinator should flag merge order in the team thread.

### 7. Ambiguous Requirements

**Problem:** Agents interpret ambiguous requirements literally and may build the wrong thing.

**Fix:** Be specific in requirements. Include examples, edge cases, and what you DON'T want. Agents are thorough but not mind-readers.

### 8. No Rollback Plan

**Problem:** A bad deployment with no way to quickly revert.

**Fix:** Always tag releases. Rollback = checkout previous tag + redeploy. Design database migrations to be forward-compatible.

---

## Checklist

Use this checklist to track your setup progress:

- [ ] AI subscriptions active (Claude + ChatGPT)
- [ ] Agent platforms installed (Zylos / OpenClaw / Claude Code)
- [ ] GitHub accounts created for each agent
- [ ] Agents added as repo collaborators
- [ ] Branch protection configured
- [ ] CI pipeline set up (GitHub Actions)
- [ ] PR template added
- [ ] CONTRIBUTING.md added
- [ ] Agent-to-agent communication channel created
- [ ] Human-agent team chat set up
- [ ] Team roles documented and shared
- [ ] Codex CLI installed and tested
- [ ] First sprint (infrastructure) completed
- [ ] First feature sprint completed

---

## Next Steps

Once your team is running:

1. **Iterate on the process** — After each sprint, discuss what worked and what didn't. Update your PROCESS.md.
2. **Add more agents** — If throughput is the bottleneck, add Developer agents.
3. **Expand automation** — PR monitoring, auto-notifications, dependency audits.
4. **Apply to more projects** — Use the same team and process across multiple repositories.
5. **Share your experience** — Open an issue on this repo with your setup and lessons learned!
