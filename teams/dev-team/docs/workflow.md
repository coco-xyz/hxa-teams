# Workflow

The complete development workflow from requirement to production. This process is designed for autonomous agent operation with human oversight at key checkpoints.

---

## Overview

```
Requirement → PRD → PO Approval → Parallel Development → Automated Review → QA → PO Merge → Deploy → Smoke Test
```

Each step is detailed below.

---

## 1. Requirement Intake

**Who:** Product Owner (PO)
**Where:** Team chat (Lark, Slack, Discord)

The PO posts a requirement in the team channel. It can be as simple as a chat message — the Coordinator agent will formalize it.

**Example:**
> "We need a digest email feature. Users should be able to subscribe to weekly digests of their saved articles."

---

## 2. PRD Creation

**Who:** Coordinator Agent
**Input:** PO's requirement
**Output:** PRD document (`docs/prd/feature-name.md`)

The Coordinator:
1. Interprets the requirement
2. Writes a PRD including: problem statement, proposed solution, impact analysis, acceptance criteria, test cases, and assigned developers
3. Submits the PRD as a PR for review

Other developer agents can add technical implementation details to the PRD.

> See [templates/prd-template.md](../templates/prd-template.md) for the PRD format.

**Exception:** Bug fixes and urgent hotfixes can skip the PRD, but must include a post-mortem explanation.

---

## 3. PO Review & Approval

**Who:** Product Owner
**Where:** GitHub PR

The PO reviews the PRD + test cases on the PR:
- Approve → development begins
- Request changes → Coordinator revises and resubmits

**No development begins until the PRD is approved.**

---

## 4. Task Decomposition & Batch Assignment

**Who:** Coordinator Agent

Once the PRD is approved:
1. Coordinator breaks the work into **batches** — groups of related tasks that can be developed independently
2. Creates GitHub issues for tracking
3. Posts batch assignments in the agent-to-agent communication thread

```
Example batch split:

PRD: Digest Email Feature
├── Batch 1 (Coordinator): Database schema + subscription API
├── Batch 2 (Developer B): Email template engine + send logic
└── Batch 3 (Coordinator): User preferences UI + settings page
```

**Key principle:** Batches should be independent enough to develop in parallel without merge conflicts. The Coordinator designs the split to minimize cross-batch dependencies.

---

## 5. Parallel Development

**Who:** Coordinator + Developer Agents
**Where:** Feature branches

```
     main
       │
       ├── feat/digest-db-schema        ◄── Coordinator (Batch 1)
       │
       ├── feat/digest-email-engine      ◄── Developer B (Batch 2)
       │
       └── feat/digest-preferences-ui    ◄── Coordinator (Batch 3)

       ▲▲▲ all developed simultaneously ▲▲▲
```

Each agent:
1. Creates a feature branch from `main` (or `develop` if using a staging branch)
2. Develops their batch independently
3. Runs local checks (`lint`, `test`)
4. Submits a PR using the project's PR template

---

## 6. Automated Review (Codex Loop)

**Who:** QA Agent + Codex CLI
**Trigger:** PR submitted

This is a key innovation. Every PR goes through iterative automated review:

```
Developer submits PR
        │
        ▼
┌──────────────────┐
│   Codex CLI       │
│   Review Pass     │──── Issues found ──→ Developer fixes ──┐
│                   │                                         │
└────────┬──────────┘                                         │
         │                                                    │
         │◄───────────────────────────────────────────────────┘
         │
    No issues
         │
         ▼
    ✅ CLEAN
```

**How it works:**
1. QA Agent runs Codex CLI against the PR diff
2. Codex reports issues (security vulnerabilities, logic errors, performance concerns, style violations)
3. Developer agent fixes the issues and pushes new commits
4. QA Agent runs Codex CLI again
5. Repeat until Codex returns **CLEAN** (no new issues)

**What Codex catches:**
- Security vulnerabilities
- Logic errors and edge cases
- Performance anti-patterns
- Code style violations
- Missing error handling

---

## 7. Agent QA Review

**Who:** QA Agent
**Trigger:** Codex CLEAN achieved

After the automated review loop passes, the QA Agent does a manual review:
- Code quality and readability
- Alignment with PRD requirements
- Test coverage
- Impact on existing functionality
- Browser-based functional testing (if applicable)

The QA Agent approves or requests changes on the PR.

---

## 8. PO Final Review & Merge

**Who:** Product Owner
**Where:** GitHub

The PO receives a notification (via team chat) that a PR is ready for final review:
- Spot-check the changes (agents have already done thorough review)
- Approve and merge

**Important:** PRs should be merged in order. If Batch 1 merges first, Batch 2 and 3 may need to rebase to resolve conflicts before merging.

---

## 9. Staging Verification

**Who:** QA Agent
**Where:** Staging environment

After merge to the integration branch:
1. QA Agent runs through the PRD acceptance criteria on staging
2. Checks that existing functionality still works (regression)
3. Reports results to PO

**Checklist:**
- [ ] PRD acceptance criteria pass
- [ ] Pages load without errors
- [ ] Existing features unaffected
- [ ] Mobile layout works (if applicable)
- [ ] Unauthenticated user experience works

---

## 10. Production Deployment

**Who:** Product Owner (approval) + QA Agent (execution)

1. PO approves deployment
2. Merge integration branch to `main`
3. Tag the release (`v1.0.0`, `v1.1.0`, etc.)
4. Deploy to production
5. QA Agent runs smoke tests immediately

**Smoke test checklist:**
- [ ] Health endpoint returns 200
- [ ] Core user flows work
- [ ] PRD key acceptance criteria verified
- [ ] No console errors

If smoke tests fail → **immediate rollback** to the previous release tag.

---

## Branch Strategy

```
feat/xxx  ──PR──→  develop  ──auto deploy──→  staging
                      │
                      │  (PO acceptance)
                      ▼
                    main  ──release tag──→  production
```

| Branch | Purpose | Deploys To | Who Merges |
|--------|---------|------------|------------|
| `feat/xxx` | Feature development | — | Developer creates |
| `develop` | Integration branch | Staging | QA Agent merges after review |
| `main` | Production branch | Production | PO merges after acceptance |

**Rules:**
- `main` is always clean and deployable
- No direct commits to `main` or `develop` — all changes go through PRs
- Staging issues don't affect production

**Simplified variant:** For smaller projects, you can skip the `develop` branch and merge feature branches directly into `main` with staging verification done before merge.

---

## Communication Patterns

### Channels

| Channel | Purpose | Participants |
|---------|---------|-------------|
| **Agent Thread** | Task assignment, progress sync, batch coordination | All agents |
| **Team Chat** | Daily coordination, PR notifications, quick decisions | All (PO + agents) |
| **PO Direct Message** | Strategic discussions, sensitive topics | PO + Coordinator |
| **GitHub PR Comments** | Code-level discussions, review feedback | All agents + PO |
| **GitHub Issues** | Task tracking, bug reports | All agents + PO |

### Notification Flow

```
PR submitted ──→ Coordinator notifies team chat
                      │
Review complete ──→ Coordinator notifies team chat
                      │
Ready for merge ──→ Coordinator notifies PO
                      │
Deployed ──→ QA Agent notifies team chat
```

---

## Automation Layer

| Automation | Owner | Mechanism |
|-----------|-------|-----------|
| PR status monitoring | Coordinator | Scheduler polls GitHub API every 30 min |
| CI pipeline | QA Agent | GitHub Actions on every PR |
| Codex iterative review | QA Agent | Local Codex CLI, triggered on PR submission |
| Team notifications | Coordinator | Scheduler pushes PR events to team chat |
| Dependency audit | QA Agent | Weekly scheduled `npm audit` / `pip audit` |
| Progress sync | Coordinator | Posts batch updates in agent thread |

---

## Incident Response

When production issues are detected:

1. **Detect** — Agent discovers the issue (monitoring, smoke test failure, user report)
2. **Act immediately** — Don't wait for PO instruction
3. **Quick fix possible?** → Fix it, deploy, notify PO
4. **Root cause unclear?** → Rollback to previous release tag, then investigate
5. **Post-mortem** — Document in `docs/INCIDENTS.md`

**Rollback procedure:**
```bash
# Checkout the previous release
git checkout v1.0.0

# Redeploy
# (your deployment command here)

# Notify the team
# "Rolled back to v1.0.0 due to [issue]. Investigating."
```

---

## Complete Flow Diagram

```
PO posts requirement (team chat)
         │
         ▼
Coordinator writes PRD ──PR──→ PO approves
         │
         ▼
Coordinator splits into batches
         │
    ┌────┴────────────┐
    ▼                 ▼
 Agent A           Agent B
 Batch 1           Batch 2         ◄── Parallel development
    │                 │
    ▼                 ▼
 Submit PR         Submit PR
    │                 │
    ▼                 ▼
┌─────────────────────────────┐
│  Codex CLI Review Loop      │    ◄── Iterate until CLEAN
│  Fix → Re-review → Fix → …  │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│  QA Agent Review             │    ◄── Manual QA + functional testing
└──────────────┬──────────────┘
               │
               ▼
   PO final review + merge
               │
               ▼
   Staging verification (QA)
               │
               ▼
   PO approves deployment
               │
               ▼
   Production deploy + smoke test
```
