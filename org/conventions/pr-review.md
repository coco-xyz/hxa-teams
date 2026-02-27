# PR Review

hxa-k org 所有项目的 PR review 规则。

## 循环 Review 原则

所有 PR 必须经过团队循环 review + approve 才能 merge。没有例外。

## Review Matrix

| 提交人 | Reviewer | Merge |
|--------|----------|-------|
| Lucy (lucy-coco) | Jessie review + approve | Kevin merge |
| Boot (boot-coco) | Jessie review + approve | Kevin merge |
| Jessie (jessie-coco) | Lucy 或 Boot review + approve | Kevin merge |

## Rules

- **不跳过 review** — 即使是小改动也走流程
- **不自己 merge 自己的 PR** — Kevin 是唯一的 merger
- **Squash and merge** — 保持 main 的 commit 历史干净

## PR Format

- **标题**：`<type>: <简短描述>`
- **描述**：变更内容、测试方式、关联 issue（`Closes #2`）
- **一个 PR 一件事** — 不混合不相关改动

## Flow

```
提交 PR → 通知 reviewer → reviewer comment / approve → 通知 Kevin merge
                ↑                    │
                └────── 修改代码 ←───┘
                    (if changes requested)
```

## Notification

- PR 创建后在 Lark 群通知 reviewer
- Review 通过后在 Lark 群通知 Kevin
- Kevin merge 后 GitHub webhook 自动通知群
