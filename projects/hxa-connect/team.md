# HxA Connect — Team

**Project:** [coco-xyz/zylos-hxa-connect](https://github.com/coco-xyz/zylos-hxa-connect)
**Based on template:** [dev-team-ops](../../teams/dev-team-ops/)
**Status:** Active

> Agent 间通信基础设施 — WebSocket Hub + 多通道投递管道

## Team Composition

| Role | Agent | Responsibilities |
|------|-------|------------------|
| **Dev Lead** | Jessie | Hub 运维、C4 集成、升级部署 |
| **DevOps** | Boot | 客户端部署、webhook 投递、PM2 管理 |
| **Developer** | Lucy | 日常使用、功能测试 |
| **Infra** | Lisa | Cloudflare 管理（⚠️ GitHub restricted — see [agent profile](../../org/agents/lisa.md)） |
| **Product Owner** | Kevin | 产品方向、架构决策 |

## Architecture

- **Hub 服务端**: PM2 `bots-hub`, 端口 4800
- **客户端**: PM2 `zylos-hxa-connect`, WebSocket 长连接
- **C4 管道**: comm-bridge channel `hxa-connect`
- **认证**: agent_token (per-bot)

## Communication

| Channel | Platform | 用途 |
|---------|----------|------|
| HxA-K thread | HxA Connect | 主要协作通道 |
| JessieClawMc-testing | Lark | 问题讨论 |

## Deployment

| 环境 | 地址 | 说明 |
|------|------|------|
| Hub | jessie.coco.site/hub/ | 端口 4800, PM2: bots-hub |
| Client | local | PM2: zylos-hxa-connect |

## Key Decisions

| # | Decision | Date |
|---|----------|------|
| 1 | 全面改名 botshub → hxa-connect | 2026-02-28 |
| 2 | 登录 session 延长至 7 天 | 2026-02-28 |
| 3 | agent_token 认证（非 admin secret） | 2026-02-27 |

## References

- [PR #22](https://github.com/coco-xyz/zylos-hxa-connect/pull/22) — UUID thread fix
