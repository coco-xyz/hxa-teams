# ClawFi Development Process

> Ship a production-quality family finance app with an AI agent team.

## Core Principles

1. **PRD is the baseline** — All features reference a PRD
2. **Production is the red line** — Changes go through staging first
3. **Test first** — PRDs include test cases
4. **Everything is rollback-safe** — Every deployment can be quickly reverted

## Roles

| Role | Who | Responsibilities |
|------|-----|------------------|
| Product Owner | Kevin | Requirements, PRD approval, final merge |
| Coordinator + Lead Dev + QA | Lisa | PRDs, task splitting, dev, review, QA, deploy |
| Developer | Sub-agents (Claude Code) | Develop assigned batches |

## Workflow

```
Requirement → PRD → PO Approval → Development → Code Review → Staging → PO Acceptance → Deploy → Smoke Test
```

### Development Flow

1. Kevin posts requirement in Telegram
2. Lisa writes PRD, submits as PR
3. Kevin approves PRD → development begins
4. Lisa splits into batches, spawns sub-agents
5. Sub-agents submit PRs → Lisa reviews
6. Kevin final review + merge
7. Lisa deploys + smoke tests

### Code Review Flow

```
Sub-agent PR → Lisa review → Kevin merge
                   ↕
             iterate if needed
```

## Branch Strategy

| Branch | Deploys To | Who Merges |
|--------|------------|------------|
| `feat/xxx` | — | Developer creates |
| `main` | ff.kevinhe.io | Kevin merges |

## Environments

| Environment | URL |
|-------------|-----|
| Development | localhost:4737/4738 |
| Staging | ff-staging.kevinhe.io |
| Production | ff.kevinhe.io |

## Rollback

- Tag every release (`v0.2.0`, etc.)
- Rollback = checkout previous tag → PM2 restart
- DB migrations must be forward-compatible

## Incident Response

1. Detect → investigate immediately
2. Quick fix possible → fix, deploy, notify Kevin
3. Root cause unclear → rollback, then investigate
