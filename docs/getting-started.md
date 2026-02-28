# Getting Started with HxA Teams

> Set up your first human × agent team in under an hour.

hxa-teams gives your agents a shared "team handbook" — who's on the team, what each member does, how they collaborate. Pick a template, map your agents to roles, and let them self-organize.

## Prerequisites

- **2+ persistent AI agents** (2nd-gen: [Zylos](https://github.com/zylos-ai/zylos-core), [OpenClaw](https://github.com/openclaw/openclaw), Claude Code, or similar — agents with memory, identity, and autonomous scheduling)
- **hxa-connect** (currently [BotsHub](https://github.com/coco-xyz/bots-hub)) for agent communication
- **A human team lead** — you, setting direction and approving output

## Step 1: Choose a Team Template

Browse the [`teams/`](../teams/) directory:

| Template | Best For |
|----------|----------|
| [Dev Team](../teams/dev-team/) | Shipping software — PRs, code review, CI/CD |
| [Sales Team](../teams/sales-team/) | Outbound pipeline — research, outreach, follow-up |
| [Marketing Team](../teams/marketing-team/) | Content creation — research, writing, multi-platform publishing |

Read the template's README to understand the roles and workflow.

## Step 2: Map Your Agents to Roles

Each template defines roles with required capabilities. Map your available agents:

**Example (Dev Team with 3 agents):**

| Role | Agent | Why |
|------|-------|-----|
| Coordinator | Jessie (Zylos) | Experienced, cloud server, always on |
| Developer | Lucy (Zylos) | Has Codex CLI, good at parallel tasks |
| QA/Reviewer | Lisa (OpenClaw) | Strong at testing, has deployment access |

**What if you have fewer agents than roles?**
- One agent can hold multiple roles (e.g., Coordinator + Developer)
- Prioritize: fill coordination and quality roles first, double up on execution roles

## Step 3: Set Up Communication

Create hxa-connect channels as defined in the template:

1. **Create a project thread** — this is your team's main channel
2. **Invite all agents** to the thread
3. **Post the kickoff message** — project goals, role assignments, first tasks

```bash
# Example: Create thread via hxa-connect (currently BotsHub API)
# See https://github.com/coco-xyz/hxa-connect for the latest API reference
curl -X POST "http://your-hub:4800/api/threads" \
  -H "Authorization: Bearer $AGENT_TOKEN" \
  -d '{"topic": "Project X - Dev Team", "type": "project"}'
```

## Step 4: Use the Planner (Optional)

For automated team setup, feed the [planner system prompt](../planner/system-prompt.md) to your coordinator agent along with:

1. The team template (full directory contents)
2. Your inputs (agents, resources, goals)

The planner will output a concrete team plan with role assignments, channels, and kickoff actions.

## Step 5: Kick Off

1. Coordinator posts first task assignments to the thread
2. Agents begin working according to the workflow
3. Quality gates ensure work meets standards before moving forward
4. Human lead reviews and approves at defined checkpoints

## Tips

- **Start with a small task** — don't launch with your most complex project. A simple feature or one piece of content lets you calibrate the team.
- **Let the workflow work** — don't shortcut quality gates just because agents are fast. The review steps exist for a reason.
- **Iterate the template** — after your first project, update the template with lessons learned. Templates are living documents.
- **Use hxa-connect for everything** — keep all coordination in threads, not in side channels. This creates an audit trail and helps new agents onboard.

## Team-Specific Setup Guides

Each team template may include its own detailed getting-started guide with team-specific setup steps:

- **Dev Team:** [teams/dev-team/docs/getting-started.md](../teams/dev-team/docs/getting-started.md) — GitHub accounts for agents, CI/CD setup, branch protection, Codex review loop

## What's Next

- Read the template's detailed docs (roles, workflow, infrastructure)
- Check out the [case study](../teams/dev-team/docs/case-study-clawfeed.md) for a real-world example
- [Contribute a new team template](contributing.md) if you build something useful

---

## Customizing for Your Project

Templates are starting points — you'll need to adapt them to your specific agents, infrastructure, and goals.

See the **[Fork & Customize Workflow](fork-workflow.md)** for the recommended approach:
1. Fork this repo
2. Create a project instance (`teams/{template}/instances/{project}/`)
3. Customize role mappings, infrastructure, and workflow
4. Keep in sync with upstream template updates
5. PR back generally-useful improvements
