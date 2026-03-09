# Multi-Agent Development Workflow

How multiple AI agents collaborate on development work: role assignments, environment rules, handoff procedures, and conventions.

## 1. Role Assignments

| Agent | Role | Responsibilities | Primary Repos |
|-------|------|-----------------|---------------|
| **Agent A** | Tech Lead / Coordinator | PR review, code merge, staging deployment, team coordination, human lead liaison | All |
| **Agent B** | QA + Developer | Functional testing, test reports, feature development, PR review | project-alpha, project-beta |
| **Agent C** | Frontend Developer | UI components, CSS, shared layouts, frontend features | project-alpha, shared-ui |
| **Human Lead** | Product Owner | Direction, final approval, production deployment authorization | — |

### Assignment Principles

- **One PR per agent.** Do not have multiple agents modifying the same PR branch simultaneously.
- PRs with dependencies must have a defined merge order and explicit base branches.
- Agent-to-agent coordination happens on the team's hxa-connect thread.
- Issues requiring a human decision are escalated by Agent A to the human lead's preferred channel.

## 2. Staging Environment

### Deployment Rules

1. **Agent A is responsible for deploying and maintaining all staging environments.** Other agents open PRs after completing development; Agent A reviews and deploys to staging.
2. Each project maps to a staging URL:

| Project | Staging URL | Serve Method | Branch |
|---------|-------------|-------------|--------|
| project-alpha | agent-a.example.com/alpha | Node.js server | main or designated dev branch |
| project-beta | agent-a.example.com/beta | PM2 | dev branch |
| shared-ui (docs) | agent-a.example.com/docs | Static build | main or designated dev branch |

3. **Notify the team before switching branches** to avoid interrupting another agent's active validation.
4. After staging validation passes, Human Lead approves production deployment.

## 3. Agent Unavailability and Handoff

### Unavailability Scenarios

- Technical failure (server down, context overflow, session crash)
- Task timeout with no response
- Platform account issue (flagged, rate-limited, suspended)

### Handoff Procedure

1. **Detection:** Progress syncs (recommended every 10 minutes) check each agent's status. Two consecutive missed syncs = unavailable.
2. **Notification:** Agent A posts in the team thread and notifies the human lead.
3. **Reassignment:**
   - Agent A assigns the task based on priority and current agent load.
   - The replacement agent continues from the original agent's development branch (no new branch).
   - The replacement agent reads all commits and review comments on the original PR before continuing.
4. **Recovery:** When the original agent comes back online, it checks for any work that was taken over, syncs with the replacement agent, and resolves any conflicts.

### Handoff Priority Matrix

| Unavailable Agent | Primary Replacement | Secondary Replacement |
|-------------------|--------------------|-----------------------|
| Agent A (Lead) | Human Lead manually assigns | — |
| Agent B | Agent A | Agent C |
| Agent C | Agent A | Agent B |

## 4. Conventions

### Team Members

| Member | GitHub Handle | Access |
|--------|--------------|--------|
| Human Lead | your-org-owner | Owner (all repos) |
| Agent A | agent-a-bot | Admin (org projects) |
| Agent B | agent-b-bot | Write (assigned repos) |
| Agent C | agent-c-bot | Write (assigned repos) |

Replace these with your team's actual handles in `.hxa-teams.yml`.

### Repository Organization

Group repos logically by product line or domain. Example:

| Org / Group | Purpose |
|-------------|---------|
| **your-org** | Core product repos |
| **your-org-infra** | Infrastructure, tooling, shared libraries |

### Branch Conventions

| Branch | Purpose | Protection |
|--------|---------|------------|
| `main` | Production branch — PR merges only | 1 review required + enforce admins |
| `develop` | Integration branch (if used) | Optional protection |
| `feat/<name>` | New features | Agent creates |
| `fix/<name>` | Bug fixes | Agent creates |
| `<agent>/consolidated` | Agent's batch of related changes | Agent creates |

### PR Flow

```
Agent develops → self-test → open PR → Codex review → fix P1/P2 → human review → merge
```

1. **Before opening PR:** lint, build, and basic tests must pass.
2. **Codex review:** all PRs must pass a Codex review (see `docs/codex-review-workflow.md`).
3. **Human review:** request the designated reviewer.
4. **Merge:** only the human lead or explicitly authorized members merge to `main`.
5. **No force pushes** to `main`.

### Environments

| Environment | Purpose | Deploy Authority |
|-------------|---------|-----------------|
| Staging | Internal validation | Agent A (no approval needed) |
| Production | User-facing | Human Lead approval required |

- Sensitive config (API keys, tokens) must never be committed to the repo.
