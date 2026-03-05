# [Project Name] Development Process

> **Template version:** dev-team/PROCESS v1.1
> **Based on:** [hxa-teams dev-team template](https://github.com/coco-xyz/hxa-teams/tree/main/teams/dev-team)
> **Last updated:** 2026-02-27

<!--
  TEMPLATE: Replace [Project Name] with your project name.
  This document defines how the agent dev team works on this project.
  Customize the roles, environments, and workflows for your setup.
  This is a living document — update it as the team learns and improves.

  VERSIONING:
  - When you fork this template into a project, keep the "Template version" line
  - When the template is updated upstream, compare your version against the latest
  - Cherry-pick improvements that apply to your project
  - If you discover improvements in your project, PR them back to hxa-teams
-->

> Goal: Ship high-quality software with an AI agent team while maintaining production stability.

## Core Principles

1. **PRD is the baseline** — All features reference a PRD. Development, testing, and acceptance all check against it.
2. **Production is the red line** — All changes go through staging first. Production must never go down.
3. **Test first** — PRDs include test cases. Review and approve test cases before development begins.
4. **Everything is rollback-safe** — Every deployment can be quickly reverted.
5. **PO should never chase** — The Coordinator owns the entire flow from requirement to merge. The PO should only need to make decisions (approve/reject), never push the process forward.
6. **Silence is a bug** — If the PO sees no updates for more than a few hours, something is broken. The Coordinator must proactively report status.

## Roles

| Role | Who | Responsibilities |
|------|-----|------------------|
| Product Owner (PO) | [Human name] | Requirements, PRD approval, staging acceptance, production sign-off |
| Coordinator + Lead Dev | [Agent A] | Write PRDs, split tasks, develop, coordinate team |
| Developer | [Agent B] | Develop assigned batches, address review feedback |
| QA + Code Review | [Agent C] | Code review (Codex + manual), testing, CI/CD maintenance |

## Complete Workflow

```
Requirement → PRD + Test Cases → PO Review → Development → Code Review → Staging Test → PO Acceptance → Production Deploy → Smoke Test
```

### 1. Requirement to PRD

- PO posts requirement in team chat
- Coordinator writes PRD (`docs/prd/feature-name.md`), including:
  - Problem description
  - Proposed solution and design
  - Impact analysis
  - Acceptance criteria
  - Test cases
  - Assigned developers
- Developer agents may add technical implementation details
- PRD submitted as a PR

**Exception:** Bug fixes and urgent hotfixes can skip the PRD. Add a post-mortem explanation.

### 2. Test Cases

- QA Agent writes test cases based on PRD acceptance criteria
- Test cases included in the PRD document
- Coverage: functional tests, edge cases, regression tests
- **PO + QA Agent review test cases together with the PRD**

### 3. PO Reviews PRD

- PO reviews PRD + test cases on the PR
- Comments with change requests → Coordinator revises
- **PO approval required before development begins**

### 4. Development

- Coordinator splits work into batches, assigns to developer agents
- Each agent creates a feature branch: `feat/feature-name`
- **Never modify production data directly**
- Database schema changes must use migration files
- Submit PR when development is complete

### 5. Code Review (PR Review Flow)

```
Developer submits PR
        │
        ▼
Coordinator broadcasts to Hub thread + team chat
        │
        ▼
QA + other agents review on GitHub
        │
        ▼
All agents approve → Coordinator notifies PO
        │
        ▼
PO reviews
        │
  ┌─────┴─────┐
  │            │
has comments  no comments
  │            │
Developer   PO merges
fixes
  │
QA re-reviews + re-approves
  │
PO final merge
```

**Steps:**
1. Developer submits PR (target branch: `develop` or `main`)
2. **Coordinator immediately broadcasts** PR link to:
   - Agent communication thread (hxa-connect) — all agents get notified
   - Team chat (Lark/Slack) — PO can see progress
3. QA runs automated review (Codex CLI or equivalent) — iterate until CLEAN
4. QA + other agents do manual review (code quality, security, PRD alignment)
5. All agent reviewers approve the PR on GitHub
6. **Coordinator notifies PO** that the PR is ready for final review
7. PO reviews:
   - No comments → merge
   - Has comments → Developer fixes → QA re-reviews → PO merges

**Notification rules:**
- The Coordinator is responsible for pushing the flow forward at every step — PO should never need to ask "where is the review?"
- When a PR is submitted, the Coordinator has **10 minutes** to broadcast it
- If an agent reviewer hasn't responded within **2 hours**, the Coordinator escalates (ping again + notify PO)
- When all reviews are done, the Coordinator sends a summary to team chat tagging PO

**Review rules:**
- Every new push dismisses stale approvals — QA must re-approve
- PO is the only one who merges
- PRD and code PRs follow the same process

### 6. Staging Test

- Integration branch auto-deploys to staging
- QA Agent runs through PRD test cases on staging:
  - [ ] PRD acceptance criteria pass
  - [ ] Pages load without errors
  - [ ] Existing features unaffected
  - [ ] Mobile experience works (if applicable)
  - [ ] Unauthenticated user view works
- Pass → notify PO for acceptance
- Fail → send back to developer

### 7. PO Acceptance

- PO tests on staging against PRD
- Approved → authorize production deployment
- Not approved → feedback to team, return to development

### 8. Production Deployment

- Merge integration branch to `main`
- Tag the release (e.g., `v1.0.0`)
- Deploy production
- Update CHANGELOG.md

### 9. Production Smoke Test

- QA Agent runs smoke tests immediately after deploy:
  - [ ] Health endpoint returns 200
  - [ ] Core user flows work
  - [ ] PRD key criteria verified
- Pass → notify PO that deployment is complete
- Fail → **immediate rollback**

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
| `develop` | Integration branch | Staging | QA merges after review |
| `main` | Production branch | Production | PO merges after acceptance |

**Rules:**
- `main` is always clean and deployable
- No direct commits to `main` or `develop`
- Staging issues don't affect production

## Branch Protection

- `develop`: CI passing + 1 review required
- `main`: PO approval required

## Environments

<!-- Customize URLs and environments for your project -->

| Environment | Branch | Purpose |
|-------------|--------|---------|
| CI | feat/* | Automated tests (lint + security audit + tests) |
| Staging | develop | Integration testing + PO acceptance |
| Production | main | Live service |

## Rollback

- `main` is tagged on every release (`v1.0.0`, `v1.1.0`, ...)
- Rollback = checkout previous tag → redeploy
- Database migrations must be forward-compatible (add-only, no destructive changes)
- If a migration is destructive, the PRD must include a rollback plan

## Incident Response

1. **Detect anomaly** → investigate immediately (don't wait for instructions)
2. **Quick fix possible** → fix, deploy, notify PO
3. **Root cause unclear** → rollback to previous tag, then investigate
4. **Post-mortem** → document in `docs/INCIDENTS.md`

## Pre-Deployment Checklist

- [ ] CI passes (lint + security audit + tests)
- [ ] Async/await properly matched
- [ ] New network calls have error handling
- [ ] No console errors in browser (for frontend changes)
- [ ] Database migrations are idempotent
- [ ] Environment variable changes synced to `.env.example`

---

## Operational SOP

> This section covers the day-to-day procedures that make the workflow actually run. The workflow above is "what happens"; this section is "who does what, when, and how."

### Agent Availability

Agents must be online and responsive. A broken agent blocks the entire flow.

**Coordinator responsibilities:**
- Verify all agents are reachable before assigning work (ping via Hub, confirm response)
- If an agent goes offline during active work, notify PO within **30 minutes**
- Reassign the offline agent's tasks to available agents if downtime exceeds **1 hour**

**All agents:**
- Respond to liveness checks (Hub ping, Lark @mention) within **5 minutes**
- If you need to go offline for maintenance, notify the Coordinator first
- After coming back online, check Hub thread for any missed assignments

**PO action required:**
- If the Coordinator reports an offline agent, decide whether to wait or reassign
- If the Coordinator itself is offline, PO contacts the platform admin

### PR Notification Protocol

This is the most common operational flow. Every PR follows this exact sequence:

```
1. Developer finishes code → submits PR on GitHub
2. Developer posts in Hub thread: "PR #{number} submitted: {title} — {link}"
3. Coordinator sees the PR (via Hub or GitHub notification)
4. Coordinator broadcasts to Hub thread: "@QA @OtherAgents please review PR #{number}"
5. Coordinator posts in team chat: "PR #{number} submitted by {developer}, under review"
6. QA + agents review on GitHub, leave comments or approve
7. Once all agent approvals are in:
   Coordinator posts in team chat: "PR #{number} reviewed and approved, ready for PO merge: {link}"
8. PO reviews and merges
9. Coordinator posts in team chat: "PR #{number} merged ✅"
```

**What if review is slow?**

| Elapsed Time | Coordinator Action |
|-------------|-------------------|
| 2 hours | Ping reviewer again in Hub thread |
| 4 hours | Escalate in team chat, tag PO: "PR #{number} waiting for review from {agent}" |
| 8 hours | PO decides: wait, reassign review, or merge with available approvals |

### Coordinator Daily Operations

The Coordinator runs these checks daily (automated via scheduler where possible):

**Morning check (start of day):**
1. Ping all agents via Hub — confirm everyone is online
2. Scan GitHub for open PRs — report status to team chat:
   - PRs waiting for review (and who's responsible)
   - PRs approved and waiting for PO merge
   - PRs with unresolved comments
3. Check for any blocked tasks and escalate

**Throughout the day:**
- Monitor Hub thread for new PR submissions
- Push notifications within 10 minutes of any PR event
- Follow up on stale reviews per the escalation table above

**End of day summary (post to team chat):**
```
Daily summary:
- PRs merged today: #12, #15
- PRs open and under review: #18 (QA reviewing)
- PRs waiting for PO: #16
- Blockers: none
- All agents online: ✅
```

### Communication Protocol

| Event | Who Acts | Where | When |
|-------|----------|-------|------|
| New requirement | PO | Team chat | When ready |
| PRD submitted | Coordinator | Hub + team chat | Within 1 hour of receiving requirement |
| PRD approved | Coordinator | Hub thread | Immediately — begin task decomposition |
| PR submitted | Developer → Coordinator | Hub → team chat | Within 10 minutes |
| Review requested | Coordinator | Hub thread | Within 10 minutes of PR |
| Review complete | Reviewer | GitHub + Hub thread | Post approval/comments |
| Ready for PO merge | Coordinator | Team chat | When all agent reviews are in |
| PR merged | Coordinator | Team chat + Hub | Immediately after merge |
| Agent offline | Coordinator | Team chat | Within 30 minutes of detection |
| Deployment complete | QA Agent | Team chat | Immediately after smoke test |
| Incident detected | Any agent | Team chat | Immediately |

### Escalation Path

When something is stuck:

```
Agent stuck → Coordinator (resolve within 1 hour)
    │
    └─ Coordinator can't resolve → PO (decision needed)
```

When infrastructure is broken:

```
Agent offline → Coordinator notifies PO (within 30 min)
    │
    └─ Coordinator offline → PO contacts platform admin directly
```

### Updating This Document

This document is a living SOP. Update it when:
- A process gap causes a problem (add the fix)
- A step is consistently skipped (remove it or make it easier)
- The team composition changes (update roles)
- A new tool is adopted (update infrastructure references)

Every update should be submitted as a PR and reviewed by the team, following the same PR flow described above.

---

## Template Changelog

| Version | Date | Changes |
|---------|------|---------|
| v1.1 | 2026-02-27 | Added Operational SOP section (agent availability, PR notification protocol, coordinator daily ops, communication protocol, escalation path). Added core principles #5 and #6 (PO should never chase, silence is a bug). Enhanced PR review flow with notification responsibilities and SLA timelines. |
| v1.0 | 2026-02-18 | Initial template — workflow, branch strategy, rollback, incident response, pre-deployment checklist. |
