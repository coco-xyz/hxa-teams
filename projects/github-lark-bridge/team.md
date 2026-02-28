# GitHub Lark Bridge — Team

**Project:** [hxa-k/github-lark-bridge](https://github.com/hxa-k/github-lark-bridge)
**Based on template:** [dev-team-ops](../../teams/dev-team-ops/)
**Status:** Active

> GitHub → Lark + HxA Connect 双通道消息投递，支持 whitelist 过滤和 per-repo 路由

## Team Composition

| Role | Agent | Responsibilities |
|------|-------|------------------|
| **Dev Lead** | Jessie | 消息消费、流程驱动、per-repo routing、rich cards |
| **DevOps** | Boot | 消息投递（GitHub → Lark + HxA Connect）、webhook 部署、whitelist 维护 |
| **Product Owner** | Kevin | 产品方向、PR merge |

## Task Assignments

| Task | Owner | Scope | Issue |
|------|-------|-------|-------|
| Org-level webhook | Boot | 组织级 webhook 部署 | #1 ✅ |
| Smart filtering + whitelist + HxA Connect push | Boot | 双通道投递 + 项目白名单 | #2 ✅ |
| Per-repo channel routing | Jessie | 按 repo 路由到不同 Lark 群 | #3 |
| Rich Lark message cards | Jessie | 结构化 Lark 卡片消息 | #4 |

## Communication

| Channel | Platform | 用途 |
|---------|----------|------|
| JessieClawMc-testing | Lark | 项目讨论 |
| HxA-K thread | HxA Connect | Agent 间任务分配 |

## Deployment

| 环境 | 端口 | 说明 |
|------|------|------|
| production | 3471 | PM2: github-webhook, Caddy: /api/github-webhook |

## Key Decisions

| # | Decision | Date |
|---|----------|------|
| 1 | 直接调用 Lark Open API（弃用 webhook bot） | 2026-02-28 |
| 2 | 双通道投递：Lark + HxA Connect 并行 | 2026-02-28 |
| 3 | config.json 热加载（fs.watch） | 2026-02-28 |

## References

- [Project Board](https://github.com/orgs/hxa-k/projects/4)
- [PR #6](https://github.com/hxa-k/github-lark-bridge/pull/6) — Lark Open API 重写
- [PR #8](https://github.com/hxa-k/github-lark-bridge/pull/8) — 双通道 + whitelist 热加载
