# ClawMark — Team

**Project:** [coco-xyz/clawmark](https://github.com/coco-xyz/clawmark)
**Based on template:** [dev-team-ops](../../teams/dev-team-ops/)
**Status:** Active

> AI-native feedback & collaboration widget — 代码嵌入 + 浏览器插件 + 开源收集器 + 多渠道分发

## Team Composition

| Role | Agent | Responsibilities |
|------|-------|------------------|
| **Dev Lead** | Jessie | 架构设计、需求分析、任务分配、PR review |
| **Developer** | Lucy | 服务端升级 (Phase 1)、浏览器插件 (Phase 2)、测试 |
| **QA & DevOps** | Boot | 分发 Adapter (Phase 3)、CI/CD、部署 |
| **Product Owner** | Kevin | 产品方向、需求确认、PR merge、验收 |

## Phase Assignments

| Phase | Owner | Scope | Issue |
|-------|-------|-------|-------|
| Phase 1: 服务端升级 | Lucy | DB schema + API v2 + Adapter 框架 + Lark adapter | #2 |
| Phase 2: 浏览器插件 MVP | Lucy (dev) + Jessie (review) | Manifest V3 + content script + side panel | #3 |
| Phase 3: 分发 Adapter | Boot | Telegram + GitHub Issue adapter + 路由引擎 | #4 |
| Phase 4: 打磨与发布 | 全团队 | Chrome Web Store + docs + 兼容性 | — |

## Communication

| Channel | Platform | 用途 |
|---------|----------|------|
| JessieClawMc-testing | Lark | 项目主群（讨论、同步） |
| HxA-K thread | HxA Connect | Agent 间任务分配 |

## Deployment

| 环境 | 地址 | 端口 |
|------|------|------|
| local | localhost | 3462 |
| staging | staging-clawmark.coco.xyz | 3459 |
| production | clawmark.coco.xyz | 3458 |

环境变量前缀：`CLAWMARK_`

## Key Decisions

| # | Decision | Date |
|---|----------|------|
| 1 | 消息归属：URL pattern 自动 + 手动 + 记忆学习 | 2026-02-27 |
| 2 | 认证：MVP 先邀请码，OAuth 后续 | 2026-02-27 |
| 3 | 高亮持久化：MVP 不做 | 2026-02-27 |
| 4 | 实时推送：MVP 先轮询，WebSocket 后续 | 2026-02-27 |
| 5 | 浏览器：先 Chrome | 2026-02-27 |

## References

- [Architecture V2](https://github.com/coco-xyz/clawmark/blob/main/docs/architecture-v2.md) — 完整架构设计
- [PR #1](https://github.com/coco-xyz/clawmark/pull/1) — 架构文档 PR
