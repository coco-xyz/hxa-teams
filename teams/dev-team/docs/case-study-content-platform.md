# Case Study: Content Aggregation Platform

A real project built by an AI agent development team — from team formation to shipping code.

**Project:** An RSS-based content aggregation and digest platform
**Team:** 3 AI agents + 1 human Product Owner
**Timeline:** Team formed and operational within a single day

---

## Background

This project was chosen as the pilot for the agent dev team for several reasons:
- Greenfield project — no legacy constraints
- Well-defined scope — RSS parsing, content aggregation, digest generation
- Multiple independent components — good for parallel development
- Real product — not a toy demo; intended for actual users

The goal was to validate whether an agent team could handle a real software project end-to-end: setup, process definition, parallel development, automated review, and production deployment.

---

## Team Composition

| Role | Agent | Platform | Environment |
|------|-------|----------|-------------|
| Coordinator + Lead Dev | Agent A | Zylos | Cloud server (always-on) |
| Developer | Agent B | Zylos | Local machine (Mac Mini) |
| QA / Review | Agent C | OpenClaw | Local machine (Mac Mini) |
| Product Owner | Human Lead | — | GitHub + team chat |

**AI Subscriptions:**
- 2x Claude Max subscriptions (for Agent A and Agent B)
- 1x ChatGPT Plus subscription (for Codex CLI, used by Agent C)

---

## Phase 0: Team Setup

Everything needed to go from "we have some AI subscriptions" to "we have a functioning dev team."

### Tasks Completed

| # | Task | Owner | Result |
|---|------|-------|--------|
| 1 | Deploy Agent B instance on local machine | PO | Zylos instance running |
| 2 | Create Agent B GitHub account + add as repo collaborator | Agent B / PO | Account active, invite accepted |
| 3 | Connect Agent B to agent-to-agent messaging | Agent B | Connected to hxa-connect thread |
| 4 | Create Agent C GitHub account + add as repo collaborator | Agent C / PO | Account active, invite accepted |
| 5 | Install Codex CLI on local machine | Agent C | Codex CLI installed and verified |
| 6 | Create team coordination thread | Agent A | hxa-connect thread created |

**Time to complete Phase 0:** ~4 hours (mostly waiting for account creation and invites)

### What We Learned

- **Account setup is the bottleneck.** The actual agent deployment takes minutes; waiting for GitHub invites and token generation takes longer.
- **Test communication early.** Make sure all agents can reach each other before starting development.
- **Document everything.** Each agent needs to know: its GitHub credentials, the repo URL, the team thread ID, and the review process.

---

## Phase 1: Pilot Sprint

The first real sprint using the agent team. Goal: establish the development infrastructure and validate the workflow.

### Tasks

| # | Task | Owner | Status |
|---|------|-------|--------|
| 1 | GitHub Actions CI (lint + test + security audit) | Agent C | Merged (PR #2) |
| 2 | PR template (`.github/pull_request_template.md`) | Agent A | Merged (PR #3) |
| 3 | CONTRIBUTING.md | Agent A | Merged (PR #3) |
| 4 | Branch protection (CI pass + 1 review required) | Agent A / PO | Configured via API |
| 5 | Codex CLI iterative review process validation | Agent C | Verified working |

### How It Went

**PR #2 — CI Pipeline (Agent C):**
- Agent C created a GitHub Actions workflow with linting, testing, and security audit
- The PR went through the full review process: Codex CLI review until CLEAN, then agent approval
- Merged by the PO after final review

**PR #3 — PR Template + Contributing Guide (Agent A):**
- Agent A created the PR template with sections for: requirement understanding, summary, type, test plan, and checklist
- Also included CONTRIBUTING.md with branch rules, workflow, code style, and review process
- Both documents were created based on the team's agreed process

**Branch Protection:**
- Configured programmatically via GitHub API
- Rules: require CI passing + 1 approving review before merge
- Stale reviews dismissed on new commits (forces re-review after changes)

### Review Process Validation

The Codex CLI iterative review loop was tested on all Phase 1 PRs:

```
PR submitted
    │
    ▼
Codex CLI review → Found 3 issues
    │
    ▼
Developer fixes issues, pushes new commits
    │
    ▼
Codex CLI re-review → Found 1 remaining issue
    │
    ▼
Developer fixes, pushes
    │
    ▼
Codex CLI re-review → CLEAN ✅
    │
    ▼
Agent C manual QA review → Approved ✅
    │
    ▼
PO final review → Merged ✅
```

**Key finding:** The iterative Codex review catches real issues — not just style nits, but logic errors, missing error handling, and security concerns. Achieving CLEAN requires genuine code quality.

---

## PRD-Driven Development

All features follow a PRD-first approach:

1. **PO states requirement** in team chat
2. **Coordinator writes PRD** including:
   - Problem description
   - Proposed solution
   - Impact analysis
   - Acceptance criteria
   - Test cases
   - Assigned developers
3. **PRD submitted as PR** for PO review
4. **PO approves PRD** → development begins
5. **Development references PRD** for scope and acceptance criteria
6. **QA tests against PRD** acceptance criteria

This prevents scope creep and ensures everyone is aligned before code is written.

---

## Parallel Batch Development

The key productivity multiplier. Here's how a typical feature is parallelized:

```
PO Requirement: "Build the content source management feature"

Coordinator splits into batches:
┌─────────────────────────────────────┐
│ Batch 1 (Agent A): Database models  │
│   + Source CRUD API                 │
│   + Data validation                 │
├─────────────────────────────────────┤
│ Batch 2 (Agent B): RSS parser       │
│   + Content fetcher                 │
│   + Feed scheduling                 │
├─────────────────────────────────────┤
│ Batch 3 (Agent A): Source mgmt UI   │
│   + Settings integration            │
│   + Error display                   │
└─────────────────────────────────────┘

Timeline:
Agent A:  [===Batch 1===]          [===Batch 3===]
Agent B:       [========Batch 2========]
                   ▲
                   └── developed in parallel
```

**Benefits observed:**
- 2 agents developing simultaneously ≈ 1.7x throughput (not 2x due to coordination overhead and sequential merge)
- Each batch is independently reviewable — smaller PRs, faster review
- Merge conflicts are rare when batches are well-designed

---

## Metrics

### Phase 1 Results

| Metric | Value |
|--------|-------|
| PRs created | 3 |
| PRs merged | 3 |
| Codex review rounds to CLEAN | 2-3 per PR |
| Time from PR to merge | < 24 hours |
| Review quality | 100% PRs through Codex CLEAN + agent QA |
| Team setup time | ~4 hours |
| Agents involved | 3 |

### Qualitative Observations

- **Agents are great at process tasks.** CI setup, templates, contributing docs — these are tedious for humans but agents handle them quickly and thoroughly.
- **The coordinator model works.** Having one agent responsible for task decomposition and coordination prevents confusion.
- **Different AI models for review adds value.** Using GPT-based Codex to review Claude-generated code catches issues that a same-model review might miss.
- **Human oversight is still essential.** The PO's judgment on requirements, priorities, and architectural direction can't be replaced — but everything else can be automated.

---

## Lessons Learned

### What Worked Well

1. **Iterative Codex review until CLEAN** — This is the highest-value automation. It catches real bugs and enforces quality without human effort.
2. **hxa-connect for agent coordination** — Agents can communicate structured task assignments and progress updates without human involvement.
3. **PRD-first approach** — Forces clarity before development. Agents are very good at writing PRDs when given a clear requirement.
4. **Branch protection rules** — Enforces the process. No shortcuts possible.

### What Needs Improvement

1. **Sequential merging** — When multiple batches are ready, they must merge one at a time (rebase conflicts). This is a bottleneck.
2. **Context handoff** — When an agent loses context (session restart), re-establishing the full project context takes time. Persistent memory helps but isn't perfect.
3. **Requirement ambiguity** — Agents interpret ambiguous requirements literally. The PO needs to be specific.
4. **Testing coverage** — Agents write good unit tests but integration testing (especially browser-based) needs more structure.

### Recommendations for Others

1. **Start with infrastructure** — CI, templates, branch protection, contributing docs. Get the process right before writing feature code.
2. **Use a pilot project** — Don't start with your most critical system. Pick a greenfield project or a well-scoped feature.
3. **Invest in the coordinator role** — The coordinator agent needs the most capabilities (memory, scheduling, communication). This is where to invest.
4. **Automate review first** — Even before parallel development, automated Codex review is immediately valuable.
5. **Keep the human in the loop** — The PO should review PRDs and final PRs. Don't try to fully automate the human judgment layer.

---

## What's Next

- **PR monitoring automation** — Coordinator scheduled to poll GitHub API and push PR status updates to team chat
- **First parallel development sprint** — Coordinator + Developer agent each taking a feature batch simultaneously
- **Cross-project expansion** — Apply the same workflow to additional repositories
- **Auto-merge on full approval** — When all checks pass (CI + Codex CLEAN + QA approval), auto-merge without waiting for PO
