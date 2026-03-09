# HxA 协作范式规范

> 版本：v3.0 | 日期：2026-03-05

---

## 修订记录

| 版本 | 日期 | 变更摘要 |
|------|------|---------|
| v1.0 | 2026-03-03 | 初版：基础角色、任务流、Git 工作流 |
| v2.0 | 2026-03-04 | 组件抽象 / 角色可轮换 / HxA Friendly / H+A 守则 / Review Loop / 变更通知 / PM 方法论集成 / 双平台 Issue / Dashboard 目标形态 / 自建判断规则 / 项目与 Repo 分离 |
| v3.0 | 2026-03-05 | 全文脱敏泛化 / HxA 原生度量体系 / 任务持久化驱动 / HxA Workspace 集成 / 多通道通信架构 / Workflow Template 抽象 / Living Rules 机制 / Variables 表 / 规范与灵活度平衡 |

---

## 变量与当前值（Variables & Current Values）

> 本表维护规范中的可变配置项。当工具或参数变更时，只需更新此表，无需修改正文。

| 变量 | 当前值 | 备注 |
|------|--------|------|
| 最佳代码审查工具 | Codex | 自动 Review，检查 P1/P2/P3 |
| 主要代码托管平台 | GitLab（自托管） + GitHub | GitLab 为 Agent 主战场，GitHub 为对外门面 |
| 核心通信工具 | HxA Connect | A-A / H-A 核心通道 |
| 任务存储（Phase 1） | Markdown + Git | Phase 2 迁移至 @hxa/tasks |
| 看板工具（Phase 1） | GitLab Issue Board / Markdown 表格 | Phase 2 迁移至 @hxa/dashboard |
| 事件总线（Phase 1） | Webhook (GitHub/GitLab) | Phase 2 迁移至 @hxa/events |
| 身份系统（Phase 1） | 手动登记 | Phase 2 迁移至 @hxa/id |
| Agent WIP 上限 | 2 (in_progress) | 可按项目覆盖 |
| Lead 管理 P0 上限 | 1 | — |
| Review 超时提醒 | 1 小时 | — |
| Review 超时升级 | 4 小时 | — |
| Agent 阻塞上报时限 | 1 小时 | — |
| Cycle 初始 Task 数 | 5-8 | 首个 Cycle 实测 |

---

## 1. 概述

本规范定义 HxA（Human x Agent）团队在多项目并行场景下的协作范式。

**解决的核心问题：**

- 多个 H/A 同时参与多个项目，缺乏统一的调度与可见性机制
- H 和 A 需要各以自己舒服的方式了解职责、任务、规范、流程（HxA Friendly）
- 项目信息散落在不同工具中，缺乏统一的抽象层
- 团队成员（H 或 A）可能轮换、掉线，工作不能因此中断
- 各种工作流程（测试、发版、入职等）缺乏统一抽象，难以复用
- 长期积累的规则和制度需要一套维护和演进机制

**设计原则：**

1. **组件抽象**：不绑定任何特定工具。项目信息可以是 Markdown 文件、GitLab Project、Notion 页面、Lark 文档——相互可转化，规范只定义信息结构，不规定载体。每类组件有最佳实践推荐，但允许替代。
2. **角色可轮换**：所有角色（PO、Lead、Member）都可以由 H 或 A 担任，且可轮换。H 不等于 PO，A 不等于 Member。
3. **HxA Friendly**：H 通过 Dashboard/Lark/浏览器 了解状态；A 通过 API/CLI/消息队列 了解状态。同一份信息，两种呈现方式。
4. **自驱动**：事件驱动 + 定时轮询，最小化 H 干预。Agent 能自主完成的绝不等指令。
5. **制度保障**：守则和规范由机制（而非自觉）驱动遵守——Review Loop、自动门禁、告警。
6. **实用优先**：规范服务于执行，不为规范而规范。自建基础设施前必须想清楚：有没有意义，能否做出差异化。
7. **规范与灵活并存**：制度是骨架，HxA 是血肉。规范提供标准路径，同时为每条规则保留显式的变通机制。组织因此有生命力——不是死守条文，而是在框架内持续进化。

**与已有文档的关系：**

| 已有文档 | 关系 |
|---------|------|
| Git 工作流规范 | 本规范的 Git 部分在其基础上扩展 |
| Git 分支策略文档 | 本规范直接引用其分支策略 |
| 代码审查工作流文档 | 本规范将自动代码审查纳入质量 Loop |
| Agent Team Ops 文档 | 本规范复用其六大机制 |
| 多 Agent 开发工作流 | 本规范在其基础上增加项目层和优先级 |
| WorkLoop 文档 | 本规范的任务追踪层依赖 WorkLoop |
| HxA Workspace 文档 | 本规范集成 HxA Workspace 组件套件 |

---

## 2. 术语定义

| 术语 | 定义 |
|------|------|
| **H（Human）** | 团队中的人类成员。可以担任任何角色（PO、Lead、Member）。 |
| **A（Agent）** | 团队中的 AI Agent。可以担任任何角色（Lead、Member，未来可能是 PO）。 |
| **PO（Product Owner）** | 项目所有者。定义目标、最终决策。当前通常是 H，但角色可转移。 |
| **Lead** | 每个项目的主责人（H 或 A）。负责任务分解、成员协调、进展追踪、日常决策。 |
| **Member** | Lead 以外参与该项目的成员（H 或 A）。执行具体任务，向 Lead 汇报状态。 |
| **项目（Project）** | 有明确目标和生命周期的工作单元。不一定有 Repo——可以是一组文档、一个调研、一次活动。 |
| **项目信息载体** | 承载项目信息的具体工具：Markdown + Git / GitLab Project / Notion / Lark Doc / 其他。规范定义结构，不绑定载体。 |
| **里程碑（Milestone）** | 项目内的重要交付节点。对应可对外演示或发布的版本。 |
| **Cycle** | HxA 原生工作周期。一个 Cycle 由任务完成速度决定而非固定日历时间（见 2.1）。 |
| **任务（Task）** | Cycle 内的最小可执行单元。有明确的 owner、输入、输出、验收标准。 |
| **优先级（Priority）** | P0（本 Cycle 必须完成）/ P1（下 1-2 个 Cycle 内）/ P2（本 Milestone 内）/ P3（backlog）。 |
| **质量 Loop** | Review -> Change -> Approve 的闭环。不是一次性动作，而是可反复的循环。 |
| **交接包（Handoff Package）** | 成员轮换时必须提供的上下文集合。 |
| **Workflow Template** | 可复用的工作流程模板（测试、发版、入职等），定义步骤 + 角色 + 门禁。 |
| **Living Rule** | 带版本号、有效期、Owner 的规则。可被提案修改或废弃，不是一成不变的。 |

### 2.1 HxA 原生度量体系

传统项目管理用"1-2 周"定义 Sprint，但 HxA 团队中 Agent 的工作节奏与人类不同——Agent 不休息、不通勤、可并行，但有会话长度限制和上下文窗口约束。因此 HxA 需要自己的度量体系。

**HxA 时间单位：**

| 单位 | 定义 | 与日历时间的关系 |
|------|------|-----------------|
| **Tick** | 一次原子操作的时间（一个 PR、一次部署、一个 bug fix） | A: 分钟到小时；H: 小时到天 |
| **Cycle** | 一组相关 Task 完成的周期，即传统 Sprint 的 HxA 替代 | 不固定，由 Velocity 决定 |
| **Milestone** | 可对外交付的版本节点 | 多个 Cycle |

**Velocity（速度）度量：**

- **Tick Rate**：每 Cycle 完成的 Tick 数（= 完成的原子任务数）
- **Cycle Length**：一个 Cycle 从开始到结束的实际耗时。首个 Cycle 需要实测；后续基于历史 Velocity 预估
- **Throughput**：单位时间内完成的 Task 数（按成员类型统计：H 和 A 分开算）

**如何确定 Cycle 长度（实施步骤）：**

1. **首个 Cycle**：Lead 估算 5-8 个 Task 为一个 Cycle，实测耗时，记录为 Baseline
2. **后续 Cycle**：根据 Baseline Velocity 预估。如果上个 Cycle 的 8 个 Task 用了 3 天，下个 Cycle 的 10 个 Task 预估 ~4 天
3. **持续校准**：每 Cycle 结束时更新 Velocity 数据。偏差 > 30% 时触发 retrospective 分析原因
4. **存储**：Velocity 数据持久化到项目信息的 `metrics` 字段（HxA Workspace: `@hxa/tasks` 的 velocity 表）

**为什么不用固定时间窗口：**

- Agent 的 Tick Rate 远高于 Human（一个 Agent 一天可能完成 10+ Tick）
- 不同项目复杂度差异巨大（文档项目 vs 底层架构项目）
- 固定"2 周"会造成简单项目空转、复杂项目赶工
- 基于 Velocity 的 Cycle 自适应团队实际产出能力

---

## 3. H 和 A 的守则

### 3.1 H 守则

| 编号 | 守则 | 保障机制 |
|------|------|---------|
| H-1 | 决策请求必须在 SLA 内回复（见 5.5），超时则 Lead 执行推荐方案 | 超时自动执行 + 通知 |
| H-2 | 不直接修改 Agent 正在工作的分支代码（防冲突） | Git branch protection |
| H-3 | 项目目标变更通过正式渠道通知，不在随意对话中改方向 | Lead 只认正式渠道的指令 |
| H-4 | 信任 Lead 的日常决策。想介入细节时先读 Lead 的最近状态汇报 | 状态汇报自动推送给 H |
| H-5 | Merge PR 前确认代码审查工具 CLEAN + Reviewer Approve | 分支保护强制 |

### 3.2 A 守则

| 编号 | 守则 | 保障机制 |
|------|------|---------|
| A-1 | 遭遇阻塞不沉默超过 1 小时，必须上报 Lead | WorkLoop 超时检测 + 告警 |
| A-2 | 完成任务后立即更新持久化状态 + 通知 Lead（见 3.4） | 自动检测：PR submitted 但状态未更新 -> 告警 |
| A-3 | 会话结束前生成交接包（当前分支、PR 链接、下一步） | 活动监控检测会话结束 -> 检查交接包 |
| A-4 | 不跳过质量门禁（代码审查工具 Review + Reviewer Approve） | CI/CD 强制检查 |
| A-5 | 不自行修改项目优先级或 Roadmap（通过 Lead 路由） | 文件权限 / Review 机制 |
| A-6 | 保持心跳，掉线恢复后主动同步上下文 | 心跳检测 + 恢复协议 |

### 3.3 通用守则（H + A 共同遵守）

| 编号 | 守则 | 保障机制 |
|------|------|---------|
| G-1 | 技术决策必须有记录（Issue / PR 描述 / 决策文档） | Lead 定期检查决策记录完整性 |
| G-2 | 凭据不暴露在群聊、公开文档、Git commit 中 | 代码审查工具安全扫描 |
| G-3 | 一个 PR/MR 只做一件事 | Reviewer 拒绝混合 PR |
| G-4 | 提交 PR 后 30 分钟内必须通知 Reviewer | 自动通知（webhook） |
| G-5 | 任务进展必须及时更新到持久化存储，不仅仅口头汇报 | 状态更新检测 + 告警（见 3.4） |

### 3.4 任务进展的及时更新与持久化

**核心原则：任务状态不只是"汇报"，而是"驱动"。每次状态更新都应持久化到存储，并触发后续动作。**

**持久化存储层级：**

| 层级 | 存储 | 适用阶段 |
|------|------|---------|
| L0 | Markdown 文件（Git 版本控制） | Phase 1（当前） |
| L1 | GitLab Issue 状态 + HxA Connect 消息记录 | Phase 1-2 |
| L2 | `@hxa/tasks` 数据库（SQLite/PostgreSQL） | Phase 2+ |

**实施步骤（Phase 1 / 当前可执行）：**

1. **每个项目维护一个 `status.md` 文件**（或 GitLab Issue Board），包含：
   - 当前 Cycle 的所有 Task 及其状态
   - 每个 Task 的最后更新时间
   - 阻塞项及其依赖
2. **任务状态变更时，执行者必须**：
   - 更新 `status.md`（或对应 Issue 状态）
   - 通过通信频道通知 Lead（见 5.7 多通道通信）
   - 如果是 `done`，附带交付物链接（PR / 文件 / 部署 URL）
3. **Lead 每日检查持久化状态与实际进展是否一致**，不一致则告警
4. **状态更新驱动下一步**：
   - Task A `done` -> 自动检查是否有依赖 Task A 的 Task B -> 通知 Task B 的 Owner 可以开始
   - Task 进入 `blocked` -> 自动通知 Lead + 记录阻塞原因 + 开始计时
   - 所有 Task `done` -> 自动通知 Lead: Cycle 可以结束，触发 retrospective

**Phase 2+ 自动化方案：**

```
[任务状态变更]
    |
    v
@hxa/tasks API: POST /tasks/:id/status
    |
    +--> 持久化到数据库
    +--> 触发依赖检查 -> 通知下游 Task Owner
    +--> 触发 SLA 计时器
    +--> 推送到 HxA Dashboard（H 视角）
    +--> 推送到 HxA Connect（A 视角）
```

---

## 4. 项目信息结构

### 4.1 信息结构（抽象层）

每个项目必须包含以下信息，但**载体不限**：

| 信息 | 必填 | 说明 |
|------|------|------|
| **定位** | 必填 | 20 字以内的一句话 |
| **角色表** | 必填 | PO / Lead / Members，含当前状态 |
| **优先级** | 必填 | P0-P3 |
| **当前 Cycle** | 必填 | 正在执行什么、目标是什么、Velocity 基线 |
| **关键链接** | 必填 | Repo / Issue 追踪 / 测试环境 / 沟通频道 |
| **路线图** | 推荐 | Milestone -> Cycle -> Task 层级 |
| **任务池** | 推荐 | 待分配 + 进行中 + 已完成（持久化，见 3.4） |
| **变更日志** | 推荐 | 重要决策和状态变更的时间线 |
| **Velocity 数据** | 推荐 | 历史 Cycle 的 Tick Rate 和 Throughput |

### 4.2 信息载体选项

| 载体 | 适用场景 | 最佳实践 |
|------|---------|---------|
| **Markdown + Git**（推荐） | 代码项目、需要版本控制的文档 | 放在团队仓库的 `projects/<name>/` 或项目自己的 repo |
| **GitLab Project** | 需要 Issue Board + CI/CD 的项目 | GitLab 的 README + Issue + Milestone 天然满足 4.1 结构 |
| **Notion / Lark Doc** | 非技术项目（品牌、市场、活动） | 一个 Doc 对应 4.1 所有字段，链接记录在团队仓库 |
| **HxA Workspace**（目标） | 统一的结构化存储 | `@hxa/tasks` + `@hxa/id` + `@hxa/config`，见 4.5 |
| **HxA Dashboard**（目标） | 所有项目的汇聚视图 | 自动从各载体拉取数据，统一呈现 |

**核心规则：不管载体是什么，信息结构（4.1）必须完整。Lead 有义务保持信息是最新的。**

### 4.3 项目与 Repo 的关系

| 类型 | 有 Repo？ | 示例 |
|------|----------|------|
| 代码项目 | 有 | 浏览器扩展、通信平台 |
| 文档/调研项目 | 不一定 | 品牌策略、竞品分析 |
| 活动项目 | 不一定 | 直播活动、GTM Launch |
| 综合项目 | 可能有多个 | 平台产品（前端 repo + 后端 repo + 文档） |

**团队仓库的角色**：项目信息的索引和默认存储。不是所有项目信息都在这里——有些在 GitLab，有些在 Notion——但团队仓库至少有每个项目的 README 指向正确的位置。

### 4.4 变更通知机制

| 变更类型 | 通知机制 | 接收方 |
|---------|---------|--------|
| 项目优先级变更 | PO -> 多通道广播（见 5.7） | Lead + 全体 Member |
| 成员变更 | Lead -> 多通道广播 | 全体成员 + 关联项目 Lead |
| Repo/Issue 平台变更 | Lead -> 更新 README + 通知 | 全体成员 |
| 阻塞影响下游 | Lead -> 通知下游 Lead | 被影响项目的 Lead |
| 环境变更（域名/部署/API） | 操作者 -> 全局通知 | 所有依赖方 |
| 规范变更 | 提交 PR -> 全员 review | 全团队 |

**事件驱动 + 定时轮询**：变更通知首选事件驱动（webhook / 多通道消息）。作为兜底，Lead 每个 Cycle 至少轮询一次关联项目的状态。

### 4.5 HxA Workspace 集成

HxA Workspace 是 HxA 的组件套件（monorepo），为协作提供结构化基础设施。本规范与 HxA Workspace 的集成点如下：

| HxA Workspace 组件 | 在协作范式中的角色 | 集成方式 |
|-------------------|-------------------|---------|
| **@hxa/id** | 统一身份：H 和 A 共用 ID 体系 | 角色表中的成员 ID 引用 `@hxa/id` |
| **@hxa/tasks** | 任务持久化存储和状态机 | 替代 Markdown status.md（Phase 2+） |
| **@hxa/config** | 项目配置和规则存储 | Workflow Template 和 Living Rule 的结构化存储 |
| **@hxa/connect** | 多通道通信路由 | 通知、告警、状态广播的统一出口 |
| **@hxa/events** | 事件总线 | Git webhook + 任务状态变更 + SLA 超时 -> 统一事件流 |
| **@hxa/dashboard** | HxA Friendly 可视化 | H 的浏览器视角 + A 的 API 视角 |

**实施策略：**

- **Phase 1（当前）**：用 Markdown + Git + HxA Connect 手动实现上述功能
- **Phase 2**：逐步将数据迁移到 HxA Workspace 组件，每个组件独立上线
- **Phase 3**：所有组件联通，HxA Workspace 成为协作的底层操作系统

**灵活度**：HxA Workspace 是推荐基础设施，不是强制依赖。如果某个项目因特殊原因不使用 HxA Workspace（例如纯文档项目只用 Notion），只要满足 4.1 的信息结构要求即可。

---

## 5. 全局工作原则

### 5.1 融合的管理方法论

| 方法论 | HxA 融入点 | 具体实施 |
|--------|-----------|---------|
| **GTD** | 任务收集 -> 处理 -> 组织 -> 回顾 -> 执行 | WorkLoop 的 "capture everything, close every loop"。每条消息中的任务必须被录入任务池，不允许"口头答应但没记录" |
| **Eisenhower Matrix** | 优先级 P0-P3 映射紧急/重要四象限 | P0=紧急+重要，P1=重要不紧急，P2=紧急不重要（委派），P3=都不（backlog或删除） |
| **RACI** | 每个任务明确角色 | 任务池中每个 Task 必须标注 R（执行者）/ A（审批者）/ C（咨询者）/ I（知会者）。最小配置：R + A 必填 |
| **Kanban** | 任务状态流 + WIP 限制 | 看板列：unassigned -> in_progress -> pending_review -> done。WIP 上限：每人 in_progress <= 2 |
| **PDCA** | 每个 Cycle 闭环 | Plan（Cycle 规划）-> Do（执行）-> Check（Cycle retrospective）-> Act（改进下个 Cycle）|
| **OKR** | 目标与度量 | Milestone = Objective，Velocity + Task 完成率 = Key Results。每 Milestone 结束时评分 |

**如何在实际工作中使用这些方法论（具体操作）：**

1. **每日 GTD 收集**：Lead 在每日检查时扫描所有通信频道，将未录入的任务录入任务池
2. **RACI 标注**：创建 Task 时必须填写 R 和 A 字段。模板：`Task: <描述> | R: <执行者> | A: <审批者>`
3. **Kanban WIP 检查**：Lead 在分配新任务前检查目标成员的 WIP 数。超限则排队或重分配
4. **PDCA Cycle**：每 Cycle 结束时 Lead 发起 5 分钟 retrospective（异步文档即可，不要求会议）
5. **OKR 评分**：每 Milestone 结束时 Lead 生成 OKR 评分报告，包含目标达成率和 Velocity 趋势

### 5.2 自动化优先级

| 层级 | 说明 | 当前状态 |
|------|------|---------|
| L0：全手动 | Lead 手动追踪、手动通知 | 大部分项目 |
| L1：半自动 | Webhook 自动通知、手动追踪 | GitHub/GitLab webhook 已上线 |
| L2：工具辅助 | @hxa/tasks + @hxa/events + Agent Registry | 计划中 |
| L3：自驱动 | AI 任务分解 + 智能分配 + 自动交接 | 远期目标 |

**原则：每一步自动化都必须先在手动模式下跑通，验证流程正确后再自动化。**

### 5.3 Review-Change-Approve Loop（强制）

所有交付物必须经过 Loop：

```
提交（Submit）
    |
    v
审查（Review）-- 代码审查工具自动 + 人/Agent 手动
    |
    v
如果有问题 -> 修改（Change）-> 重新提交 -> 回到 Review
    |
    v
如果通过 -> 批准（Approve）-> 合并/发布
```

**适用范围：**
- 代码：PR/MR（代码审查工具 Review + Reviewer Approve）
- 文档：PR review（至少 1 人 approve）
- 决策：提案 -> PO review -> approve/reject/request changes
- 设计：稿件 -> 团队 feedback -> 修改 -> PO 确认
- 规范：Living Rule 提案 -> 全员 review -> approve/reject（见第 13 节）

**Loop 超时规则：**
- Review 超 1 小时无响应 -> 自动提醒
- Review 超 4 小时无响应 -> 升级到 Lead
- Change requested 超 2 小时未修改 -> Lead 介入

**灵活度机制（Escape Hatch）**：
- **紧急修复（Hotfix）**：P0 级生产事故允许先合并后补 Review（必须在 4 小时内补完 Review Loop）
- **Trivial 变更**：纯格式、typo 修复可由 Lead 单人 approve（不需要代码审查工具 Review，但需要 Lead 在 PR 中标注 `trivial`）

### 5.4 代码质量门禁（强制）

所有 PR/MR 合并前必须通过：

1. **代码审查工具 Review CLEAN**：无 P1、无 P2 问题（P3 可选修）
2. **Reviewer Approve**：至少 1 名团队成员 approve

Review 矩阵（按项目配置，至少需要 1 名 Reviewer）：

| 提交人 | 默认 Reviewer |
|--------|--------------|
| Agent A | Agent B 或 Agent C |
| Agent B | Agent A |
| H 成员 | 指定 Agent 或另一名 H |

> 注：具体 Reviewer 分配由各项目 Lead 在项目信息中维护。上表为示意结构。

**不允许跳过代码审查工具 Review，即使是紧急修复。**（紧急修复按 5.3 Escape Hatch 流程）

### 5.5 响应 SLA（强制）

| 场景 | 响应时间 | 超时处理 |
|------|----------|---------|
| Lead 分配任务 | 30 分钟内确认接收 | 自动告警 -> Lead 重新分配 |
| Lead 状态查询 | 15 分钟内回复 | 自动告警 |
| Reviewer 收到 PR review | 30 分钟内开始 | 自动提醒 -> 1h 升级 Lead |
| Agent 遭遇阻塞 | 1 小时内上报 Lead | WorkLoop 超时检测 |
| PO 决策请求 | 附带选项 + DDL，超时执行推荐 | Lead 自动执行 + 通知 |

**双重保障：**
- **事件驱动**：消息到达即触发响应检查
- **定时轮询**：每 15-30 分钟扫描所有待响应事项，超时则告警

**具体实施（Phase 1）：**
1. Lead 维护一个 `pending-responses.md`，记录所有待响应事项及其 DDL
2. Lead 每 30 分钟扫描该文件，超时项通过通信频道提醒
3. 决策请求格式必须包含 DDL 和推荐方案（见 8.3），超时自动执行推荐

**Phase 2+ 自动化**：`@hxa/events` 的 SLA Timer 模块自动追踪所有待响应事项。

### 5.6 PR/MR 规范（强制）

- **一个 PR 一件事**：不混合不相关改动
- **标题格式**：`<type>: <简短描述>`（feat / fix / docs / refactor / test / chore）
- **描述必填**：变更内容、测试方式、关联 Issue（`Closes #N`）
- **提交后立即通知 Reviewer**：自动（webhook）+ 手动（通信频道）
- **双平台 Issue**：Issues 可能从 GitHub 或 GitLab 任一平台来。Lead 负责确保不遗漏。

### 5.7 通信规范（多通道架构）

**核心设计：通信通道是可插拔的。规范定义消息类型和路由规则，不绑定特定平台。**

**支持的通道（可持续扩展）：**

| 通道 | 类型 | 适用场景 | 不适用场景 |
|------|------|---------|---------|
| **HxA Connect** | A-A / H-A 核心通道 | 任务分配、状态查询、阻塞上报、Agent 协调 | 不用于外部通信 |
| **Slack** | H-H / H-A 协作 | 团队讨论、外部合作伙伴沟通（尤其 US 市场） | 不用于 Agent 间内部通信 |
| **Lark（飞书）** | 正式通信 | 进展汇报、决策请求、中文团队对外通知 | 不用于技术细节讨论 |
| **Discord** | 社区 / 开发者 | 开源社区运营、外部开发者支持 | 不用于内部正式决策 |
| **Telegram** | 快速通信 | 紧急通知、快速确认、个人 DM | 不用于正式决策记录 |
| **WhatsApp** | 快速通信 | 海外团队即时沟通、客户对接 | 不用于技术讨论或正式决策 |
| **Notion** | 异步协作 | 长文档协同编辑、知识库、会议纪要 | 不用于即时沟通 |
| **GitHub PR / GitLab MR** | 技术 Review | 代码 Review、技术决策记录 | 不用于日常协调 |
| **GitHub/GitLab Issue** | 任务追踪 | Bug、需求、技术讨论 | 不用于进展汇报 |

**消息路由规则：**

| 消息类型 | 首选通道 | 备选通道 | 持久化 |
|---------|---------|---------|--------|
| 任务分配 | HxA Connect | Slack / Lark | 必须持久化到任务池 |
| 状态更新 | HxA Connect | 自动同步到 Dashboard | 必须持久化到 status.md |
| 决策请求 | PO 偏好的通道 | Lark / TG | 必须持久化到决策记录 |
| 紧急告警 | TG（即时性最高）| HxA Connect + Slack | 持久化到事件日志 |
| 代码 Review | GitHub/GitLab | -- | 平台自动持久化 |
| 对外沟通 | Slack（US）/ Lark（CN） | -- | 按平台存储 |

**新增通道的步骤：**

1. 在本规范的通道表中添加新条目（类型、适用场景、不适用场景）
2. 配置消息路由规则（哪些消息类型发到新通道）
3. 如果是 Agent 需要使用的通道：开发对应的 Adapter（@hxa/connect 或 `@hxa/events` 的 plugin）
4. 提交 PR -> Review-Change-Approve Loop -> 生效

**灵活度**：每个成员可以设置自己的通知偏好（"我只在 TG 收紧急通知"），Lead 负责记录并尊重这些偏好。但正式沟通（决策、任务分配）必须走上述路由规则。

### 5.8 文档规范（强制）

- **决策记录**：所有影响设计的决策必须有记录（任何载体，但必须可检索）
- **任务状态实时更新**：变更即持久化（见 3.4），不等汇报周期
- **Roadmap 更新**：Cycle 完成后 Lead 更新 Roadmap
- **Velocity 记录**：每 Cycle 结束时记录 Tick Rate 和 Throughput

---

## 6. 角色与职责

### 6.1 角色模型

**核心规则：角色 =/= 身份。H 和 A 都可以担任任何角色。角色可轮换。**

```
PO（项目所有者）
  +-- Lead（项目主责）     <- H 或 A
  |     +-- Member       <- H 或 A
  |     +-- Member       <- H 或 A
  |     +-- Member       <- H 或 A
  +-- Lead（另一个项目）  <- H 或 A
        +-- ...
```

### 6.2 PO

**PO 角色可按项目分配给不同 H。同一个人可以是 A 项目的 PO、B 项目的 Member。**

**职责：**
- 定义项目目标和优先级
- 审批重大决策（见第 8 节）
- Merge 关键 PR/MR
- 对外代表项目

**不做的事：**
- 不追任务进度（Lead 负责）
- 不分配具体任务（Lead 负责）
- 不做日常技术决策（已授权给 Lead）

### 6.3 Lead

**每个项目有且只有一个 Lead（H 或 A）。**

**职责：**
- 维护项目信息（实时更新并持久化，不管载体是什么）
- 任务分解：Cycle 目标 -> 具体 Task（每个 Task 标注 RACI）
- 进展追踪：随时掌握每个 Member 状态，确保持久化存储与实际一致
- 日常决策：技术方案、实现路径、任务调整（不上报 PO）
- 跨项目协调：解决依赖冲突、通知上下游
- 质量把关：确保所有交付物经过 Review-Change-Approve Loop
- 定期汇报：向 PO 发摘要（P0 项目：每 Cycle；P2 项目：每 Milestone）
- Velocity 管理：每 Cycle 记录和校准 Velocity 数据

**Lead 的 HxA Friendly 接口：**
- H Lead：通过 Dashboard / Lark / 浏览器查看和操作
- A Lead：通过 API / CLI / HxA Connect 查看和操作
- 同一个操作（分配任务、查看状态），H 和 A 都有自己舒服的入口

### 6.4 Member

**职责：**
- 执行 Lead 分配的具体 Task
- 30 分钟内响应 Lead 的任务分配或状态查询
- 遭遇阻塞时立即上报 Lead（不沉默超过 1 小时）
- 完成任务后：提交交付物 + 更新持久化状态 + 通知 Lead
- 会话结束时：生成交接包

**Member 的边界：**
- 不直接联系 PO 处理技术事项（通过 Lead 路由）
- 不自行修改项目优先级或 Roadmap

---

## 7. 项目规则

### 7.1 项目管理最佳实践（全局指导）

| 实践 | 说明 | 具体实施 |
|------|------|---------|
| **WIP 限制** | 每个 Member 同时 in_progress 的任务 <= 2。Lead 同时管理的 P0 <= 1 | Lead 在分配前查看成员当前 WIP 数；超限时排队并通知成员原因 |
| **Cycle 回顾** | 每个 Cycle 结束时做 retrospective | Lead 发起异步文档，每人回答：(1) 什么做得好 (2) 什么可改进 (3) 下 Cycle 建议。5 分钟内完成，不开会 |
| **Backlog 整理** | Lead 定期清理 backlog | 每 2 个 Cycle 清理一次：关闭超过 3 个 Cycle 未动的 Task、重新评估优先级、合并重复项 |
| **可视化看板** | 任务按状态列展示 | Phase 1: GitLab Issue Board 或 Markdown 表格。Phase 2+: @hxa/dashboard 看板视图 |
| **任务粒度** | 1 个 Agent 在 1 个 Tick 内可完成 | 如果 Task 预估超过 1 Tick（一个 PR 的工作量），Lead 拆分为子 Task |
| **定义 Done** | 每个任务必须有验收标准 | 创建 Task 时填写 `done_criteria` 字段。"做完了"不算 Done，"验收标准全部通过"才算 |

### 7.2 任务状态流转

```
待分配 (unassigned)
    | Lead 分配（标注 RACI）
    v
进行中 (in_progress)
    | 完成 + 提交交付物 + 持久化更新
    v
等待 Review (pending_review)
    | Review -> Change -> Approve（Loop 可能反复多次）
    v
等待合并 (pending_merge)
    | PO merge
    v
完成 (done) -> 触发依赖检查 -> 通知下游 Task Owner
```

**异常路径：**
```
in_progress -> 阻塞 (blocked) -> Lead 介入 -> unassigned 或 in_progress
in_progress -> 掉线 (offline) -> Lead 触发交接 -> unassigned
```

**每次状态变更必须持久化**（见 3.4）。口头说"我做完了"不算状态变更。

### 7.3 允许各项目自定义的维度

| 维度 | 说明 | 示例 |
|------|------|------|
| **信息载体** | 见 4.2 | 代码项目用 Markdown+Git；品牌项目用 Notion |
| **分支策略** | main-only 或 main+develop | 活跃迭代项目: main+develop；文档: main-only |
| **Issue 平台** | GitHub / GitLab / 两者都有 | Issue 可能从两个平台来，Lead 确保不遗漏 |
| **部署节奏** | 每 PR 或 每 Cycle | 频繁迭代：每 PR；稳定项目：每 Cycle |
| **Reviewer 指定** | 特定成员优先 | 由项目 Lead 在项目信息中维护 Reviewer 矩阵 |
| **进展同步频率** | 按优先级 | P0: 每 Cycle；P2: 每 Milestone |
| **通信通道偏好** | 在全局通道基础上自定义 | 某项目用 Slack 替代 Lark 做正式沟通 |
| **Workflow Template 变体** | 在标准模板基础上调整 | 某项目不需要 staging 环境部署步骤 |

**自定义规则**：各项目可在项目信息的末尾添加 `## 项目规则` 章节，记录与全局规则的差异。差异项必须说明原因。

---

## 8. Lead 决策框架

### 8.1 Lead 可自主决策（不上报 PO）

| 决策类型 | 示例 |
|---------|------|
| 技术方案选型（常规） | 用 SQLite 还是 JSON 存任务池 |
| 任务优先级微调 | 将 P2 任务提前到本 Cycle |
| 任务分配和重新分配 | 将某成员的任务转给另一成员 |
| Bug 修复方案 | 如何修复 403 错误 |
| 代码重构决策 | 拆分过大的模块 |
| PR 合并顺序 | 先合 fix 再合 feat |
| Cycle 内调整 | 将某功能延到下个 Cycle |
| 成员轮换（因掉线）| 触发交接协议 |
| Workflow Template 微调 | 为本项目调整发版流程步骤 |

### 8.2 必须上报 PO 的决策

| 决策类型 | 原因 |
|---------|------|
| 项目优先级变更（P0/P1 调整） | 影响资源分配 |
| Milestone 目标变更 | 影响对外承诺 |
| 引入新外部服务（有成本） | 预算影响 |
| 关闭或暂停项目 | 重大资源决策 |
| 新增团队成员 | 组织层面决策 |
| 架构重大变更 | 风险较高 |
| 对外发布 | 外部影响 |
| 安全漏洞修复方案（P1 级别） | PO 需知晓 |
| Living Rule 修改（全局级别） | 影响全团队 |

### 8.3 上报格式（Review-Change-Approve Loop）

```
[决策请求] <项目名> -- <决策描述>

背景：<2-3 句>

选项：
A. <选项 A>（推荐）-- <优缺点>
B. <选项 B> -- <优缺点>

推荐：A，原因：<简短理由>

等待确认。DDL：<date/time>，超时执行 A。
```

**PO 回复 approve/reject/request changes -> Loop 闭环。**

---

## 9. 成员状态与调换机制

### 9.1 成员状态

| 状态 | 定义 | Lead 的响应 |
|------|------|------------|
| `online` | 正常工作，心跳正常 | 无需干预 |
| `busy` | 正在执行任务 | 监控 ETA |
| `idle` | 在线但无任务 | 分配新任务 |
| `blocked` | 有任务但被阻塞 | 介入解锁 |
| `offline` | 心跳丢失 > 3 分钟 | 触发交接协议 |
| `recovering` | 心跳恢复 | 推送上下文 |

### 9.2 掉线交接协议

1. **Lead 标记任务**：`in_progress` -> `unassigned`（持久化更新）
2. **保存上下文**：分支名、最新 commit、PR 链接、已完成部分、下一步
3. **分配新 Owner**：按空闲度 + 技能匹配选择
4. **新 Owner 准备**：拉取分支，读取上下文，确认接手
5. **防抖**：掉线 < 5 分钟不触发

### 9.3 Lead 轮换

Lead 不可替代性是单点风险。轮换条件：

- Lead 长时间掉线（> 30 分钟）
- Lead 因能力限制无法处理当前任务
- PO 主动指定新 Lead

**Lead 轮换步骤：**

1. PO 或 Lead 发起，在团队频道通告
2. 当前 Lead 生成完整上下文包（项目状态 + open PR + 待决策 + 成员状态）
3. 新 Lead 读取上下文包，在频道宣告接手
4. 更新项目信息中的 Lead 字段（持久化）

---

## 10. Git 协作工作流

### 10.1 平台职责

| 平台 | 主要用途 | 优势 |
|------|---------|------|
| **GitHub** | 公开项目、开源社区、Release | 生态成熟 |
| **GitLab**（自托管） | Agent 密集型工作、内部项目 | Bot 友好、自托管 |

**GitLab 是 Agent 工作的主战场；GitHub 是对外门面。**

### 10.2 双平台 Issue

- Lead 负责监控两个平台的 Issue，确保不遗漏
- 如果同一个问题在两个平台都有 Issue，Lead 在其中一个标注"tracking in <link>"，避免重复工作
- 自动化建议：webhook 将新 Issue 通知到项目通信频道

### 10.3 双平台同步策略

```
[Agent 在 GitLab 工作]
    |
    v
  GitLab MR（Agent-to-Agent review）
    |
    v
  主分支合并 -> 同步到 GitHub
    |
    v
  GitHub PR -> merge
    |
    v
  GitLab 拉取同步
```

### 10.4 分支策略

遵循团队 Git 分支策略文档。

| 项目类型 | 策略 |
|---------|------|
| 活跃迭代项目 | main + develop 双分支 |
| 小型/内部项目 | main-only |
| 文档仓库 | main-only |

### 10.5 Commit 规范

```
<type>: <简短描述>

[可选] 详细说明

Co-Authored-By: <Agent Name> <noreply@anthropic.com>
```

---

## 11. 优先级与资源分配

### 11.1 优先级定义

| 优先级 | 含义 | 资源分配 |
|--------|------|---------|
| **P0** | 本 Cycle 必须完成 | Lead 全力投入 |
| **P1** | 下 1-2 个 Cycle 内完成 | Lead + 指定 Member |
| **P2** | 本 Milestone 内 | Lead 周期参与 |
| **P3** | Backlog | 无持续资源 |

**每个 Agent/Human 同时只能有一个 P0 项目。**

### 11.2 并行规则

| 角色 | 上限 |
|------|------|
| Lead（主责） | 1 P0 + 1 P1 |
| Member | 2 P1 或 1 P0 + 1 P2 |
| 所有 | 不限 P2/P3 参与 |

### 11.3 资源冲突

1. 按优先级：P0 > P1 > P2 > P3
2. 同级：按 PO 最近指令
3. 仍有冲突：Lead 上报 PO + 影响分析 + 推荐方案

---

## 12. 工作流模板（Workflow Templates）

**设计思路**：工作流程本质是"有序步骤 + 角色分工 + 门禁条件"。通过模板化实现复用，通过参数化实现灵活。

### 12.1 Workflow Template 结构

每个 Workflow Template 包含：

```yaml
workflow:
  name: <工作流名称>
  version: <版本号>
  trigger: <触发条件>
  params:            # 可配置参数（灵活度）
    - name: <参数名>
      default: <默认值>
      override: <是否允许项目覆盖>
  steps:
    - name: <步骤名>
      role: <执行角色 R/A/C/I>
      action: <具体动作>
      gate: <进入下一步的条件>
      timeout: <超时时间>
      on_timeout: <超时处理>
  escape_hatch: <紧急情况下的简化路径>
```

### 12.2 标准工作流模板

#### 12.2.1 发版流程（Release Workflow）

| 步骤 | 角色 | 动作 | 门禁 |
|------|------|------|------|
| 1. 版本规划 | Lead | 确定版本号、变更内容、目标日期 | Lead 确认 |
| 2. 代码冻结 | Lead | 创建 release 分支，通知团队停止向 develop 合并新 feat | 分支已创建 |
| 3. 集成测试 | Member (R) + Lead (A) | 按测试清单执行，记录结果 | 所有 P1 测试通过 |
| 4. Bug 修复 | Member (R) | 修复测试中发现的问题，每个 fix 走 Review Loop | 代码审查工具 CLEAN + Reviewer Approve |
| 5. 版本标签 | Lead | 打 tag，更新 CHANGELOG | Tag 已创建 |
| 6. 部署 | Lead / Ops | 部署到目标环境 | 部署成功 + 冒烟测试通过 |
| 7. 发布通知 | Lead | 通知 PO + 全团队 + 相关方 | 通知已发送 |

**可配参数**：是否需要 staging 环境（默认 yes）、是否需要 PO 最终确认（默认 P0 项目 yes）、部署方式（手动/CI）。

**Escape Hatch**：紧急 hotfix 可跳过步骤 2-3，直接从 main 拉分支修复，但必须在 1 Cycle 内补完完整流程。

#### 12.2.2 测试流程（Testing Workflow）

| 步骤 | 角色 | 动作 | 门禁 |
|------|------|------|------|
| 1. 测试范围 | Lead | 根据变更内容确定测试范围（全量/增量） | Lead 确认范围 |
| 2. 自动测试 | CI | 运行自动化测试套件 | 全部通过 |
| 3. 代码审查 | 代码审查工具 (R) | 自动代码审查 | CLEAN（无 P1/P2） |
| 4. 手动验证 | Reviewer (R) | 功能验证 + 边界测试 | Reviewer Approve |
| 5. 回归确认 | Lead | 确认无回归，标记测试通过 | Lead sign-off |

**可配参数**：是否有自动化测试（默认按项目配置）、手动验证深度（P0: 全面；P2: 快速）。

#### 12.2.3 新成员入职流程（Onboarding Workflow）

| 步骤 | 角色 | 动作 | 门禁 |
|------|------|------|------|
| 1. 身份注册 | Admin | 在 @hxa/id 注册成员、分配初始角色 | ID 已创建 |
| 2. 通信接入 | Lead | 将成员加入相关通信频道（HxA Connect + 项目通道） | 成员确认可收发消息 |
| 3. 权限配置 | Admin | Git 平台权限、部署环境访问 | 成员确认可访问 |
| 4. 上下文传递 | Lead | 分享项目信息、规范文档、当前状态 | 成员确认已阅读 |
| 5. 首个任务 | Lead | 分配一个低风险 Task 作为试跑 | Task 完成 + Review 通过 |
| 6. 正式纳入 | Lead | 更新项目角色表，宣告入职完成 | Lead 确认 |

#### 12.2.4 事故响应流程（Incident Response Workflow）

| 步骤 | 角色 | 动作 | 门禁 |
|------|------|------|------|
| 1. 发现与报告 | 发现者 | 通过紧急通道报告，标注影响范围 | 报告已发送 |
| 2. 分级 | Lead | P0: 生产中断 / P1: 功能受损 / P2: 非关键问题 | Lead 确认级别 |
| 3. 指定处理人 | Lead | 按技能和空闲度分配 | 处理人确认接手 |
| 4. 修复 | 处理人 (R) | 修复问题（Hotfix 分支 -> PR -> 部署） | 问题解决 |
| 5. 回顾 | Lead | 记录根因、影响、修复方案、预防措施 | 回顾文档已提交 |
| 6. 预防 | Lead | 创建预防 Task（如加测试、加监控） | Task 已创建 |

### 12.3 自定义 Workflow Template

**项目可以基于标准模板自定义**：

1. 复制标准模板到项目信息中
2. 修改 params 的值（如关闭 staging 步骤）
3. 添加/删除步骤（如增加安全扫描步骤）
4. 在项目规则中记录与标准模板的差异及原因

**新建 Workflow Template**：

1. 任何团队成员都可以提案新模板
2. 提交为 PR -> Review-Change-Approve Loop
3. 通过后纳入标准模板库（`@hxa/config` 或团队仓库 `workflows/` 目录）

---

## 13. Living Rules 机制

**核心理念：规则是活的（Living Rules）。它们有版本、有 Owner、有有效期、可以被提案修改或废弃。规范提供秩序，灵活度提供生命力。**

### 13.1 Living Rule 结构

每条规则包含以下元数据：

| 字段 | 必填 | 说明 |
|------|------|------|
| **Rule ID** | 必填 | 唯一标识（如 `RULE-001`） |
| **标题** | 必填 | 规则名称 |
| **内容** | 必填 | 规则正文 |
| **版本** | 必填 | 语义化版本号（如 1.0、1.1、2.0） |
| **Owner** | 必填 | 维护者（H 或 A） |
| **创建日期** | 必填 | 首次生效日期 |
| **最后审查日期** | 必填 | 上次确认仍然有效的日期 |
| **审查周期** | 必填 | 多久审查一次（如"每 10 Cycle"或"每 Milestone"） |
| **适用范围** | 必填 | 全局 / 特定项目 / 特定角色 |
| **状态** | 必填 | active / deprecated / draft / under_review |
| **Escape Hatch** | 推荐 | 在什么条件下可以不遵守此规则 |

### 13.2 规则生命周期

```
提案（draft）
    | 提交 PR / 在团队频道发起讨论
    v
审查（under_review）
    | Review-Change-Approve Loop（全局规则需全团队 review）
    v
生效（active）
    | 定期审查（按审查周期）
    v
修订（under_review）  或  废弃（deprecated）
    | PR + Review Loop       | 标注废弃原因 + 替代规则
    v                         v
新版本生效（active）        归档
```

### 13.3 规则维护的具体实施

**Phase 1（当前）：**

1. **存储**：所有 Living Rules 记录在团队仓库 `rules/` 目录下，每条规则一个 Markdown 文件
2. **索引**：`rules/INDEX.md` 列出所有活跃规则的标题、Owner、状态、最后审查日期
3. **变更**：修改规则 = 提交 PR -> Review-Change-Approve Loop
4. **审查**：Lead 在每 Milestone 结束时检查所有规则的审查周期，到期的触发 review
5. **应用**：新成员入职时阅读所有 `active` 状态的规则；Agent 在 session start 时加载相关规则

**Phase 2+（自动化）：**

1. **存储**：迁移到 `@hxa/config` 的 rules 表
2. **审查自动化**：`@hxa/events` 定时检查审查周期，到期自动通知 Owner
3. **应用自动化**：CI/CD 门禁自动检查 PR 是否违反活跃规则
4. **变更通知**：规则变更自动广播到所有相关通道

### 13.4 规范与灵活度的平衡

**平衡机制：**

| 机制 | 作用 | 示例 |
|------|------|------|
| **Escape Hatch** | 每条规则都有"紧急例外"条件 | "代码审查工具 Review 必须通过"的 Escape Hatch: P0 生产事故允许先合并后补 |
| **Override 权限** | PO 可以覆盖任何规则（需记录原因） | PO 确认"某项目不需要 staging"-> Lead 记录到项目规则 |
| **试验期** | 新规则可以设置试验期，试验期满评估是否转正 | "Agent 必须每小时汇报"试行 2 Cycle 后发现频率过高，调整为"状态变更时汇报" |
| **审查周期** | 所有规则定期重新审视 | 每 Milestone 检查一次，过时的规则废弃 |
| **团队投票** | 争议性规则通过团队投票决定（H+A 各有投票权） | 有分歧时 Lead 发起异步投票，多数通过 |
| **项目覆盖** | 项目可在全局规则基础上添加/放宽/收紧 | 某开源项目放宽 Review 要求（社区贡献者不走内部 Review） |

**原则总结**：

1. **规范是默认路径**，不是牢笼。遵循规范可以减少沟通成本和犯错概率。
2. **灵活度是显式的**，不是偷懒。偏离规范必须有 Escape Hatch、Override 记录或项目规则声明。
3. **规则演进是常态**，不是例外。每条规则都有生命周期，不合时宜的规则应该被修改或废弃。
4. **HxA 的生命力来自迭代**：规范 -> 执行 -> 发现问题 -> 修订规范 -> 更好的执行。这个循环永不停止。

---

## 14. 分阶段实施

### Phase 1：用现有基础设施跑通（当前）

**目标**：不新建工具，跑完一个完整的多项目并行周期。

**行动项：**

1. 为所有活跃项目创建标准信息结构（4.1），载体暂定 Markdown
2. 每个项目明确 Lead，写入项目信息
3. 为每个 P0/P1 项目创建通信频道（HxA Connect + 项目偏好的通道）
4. Lead 手动维护任务池（Markdown 表格或 GitLab Issue Board），确保持久化（3.4）
5. Lead 定期向 PO 同步（频率按优先级和 Cycle 节奏）
6. 双平台 Git 流程按第 10 节执行
7. 创建团队仓库 `rules/` 目录，将本规范中的强制条款转为 Living Rules
8. 首个 Cycle 实测 Velocity Baseline

**Phase 1 不做：** 不开发新工具、不自动化任务分配、不实现心跳检测。

**成功标准：** 3+ 项目并行 1 Milestone，PO 可随时查询任何项目状态，Velocity 有 Baseline 数据。

### Phase 2：工具增强（Phase 1 稳定后）

**前提**：Phase 1 流程稳定。**先验证需求再动手。**

**候选工具（需逐一论证价值和差异化）：**

| 工具 | 解决的摩擦 | 有没有现成替代？ | 自建差异化 |
|------|-----------|----------------|-----------|
| @hxa/tasks | 手动追踪任务效率低 | GitLab Issue Board / Linear | HxA Connect 深度集成 + Velocity 自动追踪 |
| @hxa/id + 心跳 | 掉线检测靠人盯 | 无直接替代 | Agent 原生能力 |
| @hxa/events | Lead 手动路由通知 | Zapier / n8n | WorkLoop + SLA Timer + 多通道统一事件流 |
| @hxa/config (rules) | 规则散落难维护 | Notion wiki | 版本控制 + 审查自动化 + CI 集成 |
| **@hxa/dashboard** | 信息散落无法一览 | Notion / Linear | **HxA Friendly 双接口** |

**决策规则：只有"无直接替代"或"有明确差异化"的工具才自建。能用现成的先用现成的。**

### Phase 3：全自动化（Phase 2 稳定后）

**目标**：Lead 零摩擦运转，PO 只在决策时介入。

**计划能力：** AI 任务分解 / 智能分配 / 自动交接 / CI/CD 全自动 / PO 决策队列 / 规则自动审查。

**前提**：Phase 2 稳定运行 > 4 Cycle。

### HxA Dashboard（目标形态）

**愿景**：一个统一的 Dashboard，让 H 和 A 各以舒服的方式查看和操作所有项目。

**H 的视角（浏览器）：**
- 所有项目一览（按优先级排序）
- 每个项目的状态、进展、阻塞
- 决策队列（待审批）
- 团队成员状态
- Velocity 趋势图
- Living Rules 索引

**A 的视角（API/CLI）：**
- `GET /projects` -- 所有项目
- `GET /projects/:id/tasks` -- 任务列表
- `POST /tasks/:id/status` -- 更新状态（自动持久化 + 触发通知）
- `GET /rules?scope=global&status=active` -- 当前生效的规则
- HxA Connect 命令：`/task list`、`/task update`、`/team status`、`/rule check`

**实现路径**：Phase 2 优先级项。

---

## 附录 A：快速参考

### A.1 Lead 每 Cycle 检查清单

```
[ ] 任务池状态是否最新？（持久化存储与实际一致？）
[ ] 有没有 in_progress 超预期 Tick？
[ ] 有没有 blocked 超 1 小时？
[ ] 有没有 pending_review 超 30 分钟？
[ ] 本 Cycle 需要向 PO 汇报什么？
[ ] 有没有需要 PO 决策但未提交的事项？
[ ] 上下游项目有没有影响本项目的变更？
[ ] Velocity 数据是否已记录？
[ ] Living Rules 有没有到审查期的？
[ ] pending-responses.md 有没有超时项？
```

### A.2 新项目启动清单

```
[ ] 创建项目信息（按 4.1 结构，选择合适载体）
[ ] 在团队仓库至少有一个指向该项目的 README
[ ] 确认 PO、Lead、Members
[ ] 创建通信频道（HxA Connect + 项目偏好通道）
[ ] 确认 Issue 追踪平台（GitHub / GitLab / 两者）
[ ] 选择适用的 Workflow Templates（发版/测试/入职等）
[ ] 记录项目特有的规则覆盖（如有）
[ ] 设定首个 Cycle 的 Task 列表（5-8 个 Task）
[ ] 通知 PO：项目已启动
```

### A.3 PR 提交检查清单

```
[ ] 标题格式：<type>: <描述>
[ ] 描述：变更 + 测试 + Closes #N
[ ] 代码审查工具 Review CLEAN
[ ] Request reviewer（GitHub/GitLab）
[ ] 通知 Reviewer（通过项目配置的通信通道）
[ ] 一个 PR 只做一件事
```

### A.4 Living Rule 提案清单

```
[ ] 填写 Rule 元数据（ID、标题、Owner、审查周期、Escape Hatch）
[ ] 明确适用范围（全局 / 特定项目 / 特定角色）
[ ] 说明为什么需要这条规则（解决什么问题）
[ ] 提交 PR 到团队仓库 rules/ 目录
[ ] 通知全团队 review
[ ] Review-Change-Approve Loop 完成后标记 active
```
