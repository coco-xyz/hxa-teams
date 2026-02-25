# [Project Name] Development Process

<!--
  TEMPLATE: Replace [Project Name] with your project name.
  This document defines how the agent dev team works on this project.
  Customize the roles, environments, and workflows for your setup.
-->

> Goal: Ship high-quality software with an AI agent team while maintaining production stability.

## Core Principles

1. **PRD is the baseline** — All features reference a PRD. Development, testing, and acceptance all check against it.
2. **Production is the red line** — All changes go through staging first. Production must never go down.
3. **Test first** — PRDs include test cases. Review and approve test cases before development begins.
4. **Everything is rollback-safe** — Every deployment can be quickly reverted.

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
Developer submits PR → QA reviews + approves → PO reviews
                                                    │
                                          ┌─────────┴─────────┐
                                          │                    │
                                    has comments         no comments
                                          │                    │
                                    Developer fixes       PO merges
                                          │
                                    QA re-reviews + re-approves
                                          │
                                    PO final merge
```

**Steps:**
1. Developer submits PR (target branch: `develop` or `main`)
2. QA runs Codex CLI review — iterate until CLEAN
3. QA does manual review (code quality, security, PRD alignment)
4. QA approves the PR
5. PO reviews:
   - No comments → merge
   - Has comments → Developer fixes → QA re-reviews → PO merges

**Rules:**
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
