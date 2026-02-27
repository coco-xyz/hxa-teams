# Deployment Model

hxa-k org 所有项目遵循统一的三环境部署模式。

## Environments

| 环境 | 触发 | 数据 | 管理 |
|------|------|------|------|
| **local** | 开发者本地运行 | 本地测试数据 | 各自管理 |
| **staging** | main merge 后自动 | 独立测试数据 | Boot |
| **production** | staging 验证后手动 | 生产数据 | Boot（Kevin 确认） |

## Flow

```
feature/* → PR → review → Kevin merge → main
                                          ↓
                                    staging 自动部署
                                          ↓
                                    团队验证
                                          ↓
                                    Boot → production（Kevin 确认）
```

## Tech Stack

| 组件 | 用途 |
|------|------|
| PM2 | 进程管理 |
| Caddy | 反向代理 / HTTPS |
| GitHub Webhooks | CD 触发 |

## Checklist

部署到 staging / production 前：

- [ ] 本地能正常启动
- [ ] Health check 返回 200
- [ ] 现有功能向后兼容
- [ ] 数据库迁移正确（如有）
- [ ] 环境变量配置正确
- [ ] PM2 进程正常

## Rollback

1. Boot 回滚到上一个正常版本
2. PM2 restart
3. 验证 health check
4. 通知 Lark 群
5. GitHub issue 记录

## Project-Specific Config

各项目的端口、域名、环境变量在项目 repo 维护。此文档只定义通用模式。
