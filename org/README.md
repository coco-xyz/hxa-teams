# HxA-K Organization

> hxa-k org 的 Agent 团队定义 — 可用 Agent、共享规范、项目分工。

## Structure

```
org/
├── agents/           # Agent 花名册
│   ├── jessie.md
│   ├── lucy.md
│   └── boot.md
└── conventions/      # 跨项目共享规范
    ├── git-workflow.md
    ├── pr-review.md
    └── deployment.md
```

## How It Works

1. **org/** 定义可用的 Agent 和共享规范
2. **projects/** 为每个项目定义团队组合和分工
3. 项目 + 团队 = 一套合作模板实例

共享规范对所有项目生效。项目可以在自己的 `team.md` 中补充项目特定的规则，但不能违反共享规范。

## Available Agents

| Agent | Role Capability | Platform |
|-------|----------------|----------|
| [Jessie](agents/jessie.md) | Dev Lead / Architecture / Coordination | Zylos (GCP) |
| [Lucy](agents/lucy.md) | Full-Stack Development | Zylos (GCP) |
| [Boot](agents/boot.md) | QA / DevOps / Infrastructure | Zylos (GCP) |

## Human Members

| Name | Role | GitHub |
|------|------|--------|
| Kevin | Product Owner | kevinho |

## Shared Conventions

| Convention | Description |
|------------|-------------|
| [Git Workflow](conventions/git-workflow.md) | 分支策略、命名规范 |
| [PR Review](conventions/pr-review.md) | 循环 review 规则 |
| [Deployment](conventions/deployment.md) | 三环境部署模式 |
