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

- **Review 必须在 PR 上进行** — 不允许线下 review 后再提 PR。代码完成后先提 PR，reviewer 在 GitHub PR 上留 review comments，确保所有 review 记录可追溯
- **不跳过 review** — 即使是小改动也走流程
- **不自己 merge 自己的 PR** — Kevin 是唯一的 merger
- **Squash and merge** — 保持 main 的 commit 历史干净

## PR Format

- **标题**：`<type>: <简短描述>`
- **描述**：变更内容、测试方式、关联 issue（`Closes #2`）
- **一个 PR 一件事** — 不混合不相关改动

## Flow

```
代码完成 → 提 PR → request reviewer → reviewer 在 PR 上 review → approve → Kevin merge
                        ↑                         │
                        └────── push 修改代码 ←───┘
                              (if changes requested)
```

**禁止的流程（反模式）：**
```
代码完成 → 线下 review → 修改 → 再提 PR  ← ✘ review 记录丢失，不可追溯
```

## Review 响应 SLA

收到 review request 后必须主动处理，不等人催。

| 情况 | 响应时间 |
|------|----------|
| 被 request review | **30 分钟内**开始 review |
| Review 有 changes requested | 修改后 **30 分钟内** re-request review |
| PR 被 approve | 通知 Kevin merge，不需要等 |

**原则：PO 不应该追进度。** 收到通知 → 自动处理 → 流转到下一步。如果被 block 了（不理解代码、需要讨论），在 PR 上留 comment 说明，不要沉默。

## Notification

- PR 创建后 request reviewer（GitHub），同时 Lark 群通知
- Reviewer 收到 request 后主动处理（不等催）
- Review 通过后在 Lark 群通知 Kevin
- Kevin merge 后 GitHub webhook 自动通知群
