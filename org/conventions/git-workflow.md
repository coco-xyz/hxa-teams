# Git Workflow

hxa-k org 所有项目的统一 git 规范。

## Branch Strategy

```
main              ← 稳定分支，始终可部署
  └── feature/*   ← 功能开发
  └── fix/*       ← 缺陷修复
  └── docs/*      ← 文档变更
```

## Rules

1. **main 受保护** — 不允许直接 push，所有变更通过 PR 合并
2. **从 main 创建分支** — 开始前先 `git pull origin main`
3. **命名规范** — 小写英文 + 连字符（`feature/chrome-extension`），不用中文、下划线、大写
4. **合并后删除分支** — PR merge 后删除远程 feature 分支
5. **不用 develop** — 团队小，main + feature 足够

## Workflow

```bash
# 开始新功能
git checkout main
git pull origin main
git checkout -b feature/my-feature

# 开发 + 提交
git add <files>
git commit -m "feat: add something"

# 推送 + 创建 PR
git push -u origin feature/my-feature
```

## Commit Convention

```
<type>: <简短描述>
```

| Type | 用途 |
|------|------|
| `feat` | 新功能 |
| `fix` | Bug 修复 |
| `docs` | 文档 |
| `refactor` | 重构（不改行为） |
| `test` | 测试 |
| `chore` | 构建 / 工具 / 依赖 |

规则：英文、首字母小写、不加句号。
