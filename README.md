# Animal Platform V2 — Agent Runtime

> 状态：**学习与开发准备就绪；首个 Go 实现尚未开始**（更新于 2026-10-09）。
> 下一次开发：**2026-10-10，从基础契约与 Golden Case 开始**。

这是以真实动物行为分析问题为业务背景的求职导向工程项目，目标是理解、设计、实现和验证 Agent 系统，而不只是调用现成框架。

## 当前执行路线（唯一入口）

- **[最小可就业能力路线](docs/roadmap-minimum-employment.md)**：目标岗位、能力门槛、阶段与停止条件；任务/Commit **不固定数量**。
- **[学习—设计—开发—验收工作流与资源索引](docs/learning-playbook.md)**：每张任务卡该看什么、写什么、怎么证明会了。
- **[2026-10-10 开发任务书](docs/first-session-2026-10-10.md)**：明天打开即可执行。
- **[面试最小知识集](docs/interview-core.md)**：Agent / RAG / Go / MySQL / Redis / Python。
- **[首批 Golden Case 草案](eval/golden_cases/README.md)**：评测从第一阶段启动，不等全部开发完才评测。

原来的 **[V2.2 / C00–C23 Commit Map](docs/commit-map-v2.2.md)** 完整保留为历史参考；**不再以完成 24 个固定 Commit 作为就业前置条件**。

## 核心技术取舍

**必学 Go**：主项目使用 Go 独立实现 Agent Runtime（Model、Tool、Loop、Context、测试、后端工程）。  
**必学 Python Agent Runtime**：单独做有验收的 asyncio / typing / pytest / LangGraph 小型实现，与 Go **共用业务场景和评测案例**；不维护两个同等规模的平台。

核心能力：**Agent Loop + Tool Calling + Agent Evaluation + RAG/RAG Evaluation**。  
就业基础：**Go/Python、HTTP、MySQL、Redis、Git/Linux、必要的测试与调试**。  
进阶按真实需求：MCP、Session/Memory、权限、可靠性、Trace、服务化、异步任务；不为了凑技术栈提前堆组件。

**暂不做**：独立 LeetCode 刷题计划、复杂 Multi-Agent、Kubernetes/Kafka 集群、不需要的视觉算法重训、没有真实数据的性能指标。

## 业务场景与真实性

1. 按实验/视频 ID 查询**已有**小鼠水迷宫指标，并注明来源。
2. 比较 AD / sham / 药物处理组的**已有**行为指标，不伪造统计显著性。
3. 根据实验范式手册和已有记录回答问题，证据不足时明确说明。

视觉推理服务作为外部业务工具；初期 Mock 明确标记，后续有可用接口才接真实数据。

## 开始开发

阅读 [明天的任务书](docs/first-session-2026-10-10.md)。每个任务执行同一闭环：**需求 → 指定资料 → 独立设计 → 编码 → 测试/评测 → 面试复盘 → Git 提交**。

不承诺当前有可编译的 Go 项目，不宣称已经完成 Eval、RAG 或任何业务 API。毕业论文时间为优先边界。
