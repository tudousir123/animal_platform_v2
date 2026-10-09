# 最小可就业能力路线（2026-10-09）

> 用于下一阶段执行；从 V2.2/C00–C23 改为**能力门槛驱动的可变任务卡**。  
> 面向：麒盛科技、天谷信息科技、思必驰等类型企业的初级 Agent / AI 应用与部分 Agent Runtime 岗位；JD 是定性样本，**不等于市场频率统计**。

## 0. 决策与不可变约束

- **Go 必学、主项目必用 Go**：既是就业技能也是个人长期技术选择。
- **Python 的 Agent Runtime 能力必学**：asyncio、typing、测试、LangGraph 等；用同一 Golden Cases 验证，不另造完整后端平台。
- **评测前置**：从第一批 Golden Cases、Mock 与断言开始，第一版 Agent Demo 就提供 Eval Runner 和失败案例；RAG 上线前建立检索基线。高级评测后续迭代。
- **不安排专项算法刷题**：工程内必要的 Map、Slice、复杂度、并发与代码能力照学；若目标职位要求现场算法，再针对性补足。
- **毕业优先**：每周约 12–15 小时为规划参考，不因开发挤占论文底线。
- **Commit 只是变更粒度，不是目标数量**：可分、可合并、可增加或删除，每次变更要说明招聘价值、验收证据和机会成本。

## 1. 最小可就业验收门槛（MJE）

| 能力 | 最低可展示证据 | 通过前不能怎么说 |
|---|---|---|
| Go Runtime | 独立实现可测试的 Model → Tool → Model Loop、异常与终止处理 | 不声称自研生产级框架 |
| Agent Eval | 固定案例、工具选择/参数/执行完成断言、可复现 Badcase；比较至少一次改动 | 不凭主观感受声称准确率提升 |
| Python Agent | asyncio 实现极简 Loop，能用 LangGraph 复现同类链路；pytest 覆盖超时与工具失败 | 不宣称 Python 熟练却无法解释 async |
| RAG + Eval | 领域资料索引、带来源回答、约 20 个带标签检索样例；BM25 与向量检索可比较（按资料规模调整） | 不编造 Recall@K、MRR 等实验数字 |
| Agent 核心原理 | 能解释 ReAct、Tool Schema、Context/Session/Memory、常见失败路径、MCP 基本流程 | 不只背范式名称 |
| MySQL | 能写 SQL、设计索引，用 EXPLAIN / 事务并发小实验解释 B+ 树、MVCC 与索引取舍 | 不以看视频替代动手操作 |
| Redis | 能执行数据类型/TTL/缓存与 Streams 基本命令；理解持久化、重复投递与 ACK | 不把 Redis 当成绝对不丢的队列 |
| 工程表达 | README 复现流程、设计取舍、测试日志，5 分钟能讲清项目与不足 | 不写未经验证的性能、规模、统计结论 |

达到以上条件即可**针对匹配岗位试投递**，不等所有进阶任务完成；不代表保证录用。

## 2. 任务卡队列（不是固定 Commit 数）

执行顺序按依赖和弱项调整；一行可能拆为多个实际 Commit。优先完成 **A0 → A1 → A2 → A3**。

| 卡片 | 能力产出 | 不可跳过的验收 |
|---|---|---|
| **A0 Go 基础契约 + 评测草案** | Go module、Message/ToolCall/ToolResult/RunState、三项 Golden Case | JSON round-trip、状态/参数边界测试；对应面试题 |
| **A1 Model Adapter + Tool Registry** | Mock / 真实 Provider 可替换；工具 Schema、参数校验与分发 | 超时、未知工具、畸形参数、调用失败 |
| **A2 Agent Loop** | 多轮 Tool Calling、关联 ID、最大步数、终止原因和事件日志 | Mock 连续调用、工具失败、无限循环测试 |
| **A3 动物业务 Demo + Eval v1** | 至少一条可演示业务链；案例集扩展到约 15–20 个，简单 Runner、Badcase | 可复现执行报告；不能只比字符串 |
| **P0 Python Runtime Lab** | asyncio + typing + pytest 的相同业务 Loop | 取消、超时、工具错误；与 Go 相同的场景 |
| **P1 LangGraph 对照** | 用 LangGraph State/Node/Edge 实现同一场景 | 能说明何时用框架、何时手写；有对照文档 |
| **R0 RAG + Retrieval Eval** | 基于实验手册的 BM25 / 向量基础检索，来源引用 | 至少约 20 条人工核对样例；记录 Hit@K、MRR 和坏案例 |
| **R1 按缺陷优化** | 仅在 R0 出现明确问题时引入 Hybrid/RRF/Rerank | 基线 vs 改动的同集对比，真实指标 |
| **X0 Context + Session** | 预算、截断不破坏工具配对、会话隔离和必要持久化 | 长上下文、会话串扰、中断恢复测试 |
| **X1 MCP + 安全最小链路** | 真实 MCP 发现/调用、工具权限与审计基础 | 未授权不能执行；失败有明确返回 |
| **D0 MySQL 小实验** | Mosh SQL 实操、索引/事务/MVCC/锁原理 | EXPLAIN + 并发状态更新可复现 |
| **D1 Redis 小实验** | TTL/Cache Aside、Streams 消费组/ACK/重投 | TTL 和 Worker 崩溃的可验证测试 |
| **I0 HTTP / Trace / Deployment** | 最小 HTTP API、可关联日志、可复现启动 | 端到端错误定位 + 重启复现；深化 Outbox/CI 按需 |

**原则**：D0/D1 的面试知识可在 A 阶段以短时练习提前学习；大型数据库接入不必抢在 Agent MVP 前。R0 可以紧随 A3，视 Python 入门进度与文档准备情况穿插。

## 3. 停止条件与路线变更

- 某主题能回答常见问题、完成对应小实验或测试，就停止刷更多同类文章。
- 新文章、新 JD 原则上**仅用于更新对应任务卡的资源/验收**，不新开技术主线。
- 每到 A3、R0、首次模拟面试三个节点复盘一次；修改计划时记录：证据、收益、成本、决定。
- 未达到 MJE 就继续补**最弱的一项**，而不是为「完整架构」抢跑复杂分布式、Multi-Agent 或数据库集群。
