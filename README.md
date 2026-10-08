# Animal Platform V2 — Agent Runtime

> **项目状态：规划阶段 / C00 尚未开始。**

本仓库用于以真实动物行为分析场景为基础，从零构建并理解一个可验证的 **Go Agent Runtime**。开发路线采用经过 JD 与牛客面经定性调研后的 **V2.2 冻结执行版**。

## 项目目标

- **主线**：Go + Pi-inspired Agent Runtime；先贯通 Model → Tool → Agent Loop，再扩展 Context、Session/Memory、RAG/MCP、可靠性及后端工程。
- **业务场景**：接入已有动物行为分析工具与研究结果；真实接口未就绪时明确使用 Mock，不编造数据。
- **面试能力**：每个 Commit 具备需求、定向学习、设计取舍、测试验收、面经复盘和可讲解证据。
- **边界**：暂不做完整 Multi-Agent、第二套 Python 平台、Kubernetes/Kafka、无依据的性能宣传。优先保证毕业任务进度。

## 路线图

**[查看完整 C00–C23 Commit Map（V2.2）](docs/commit-map-v2.2.md)**

| 里程碑 | 内容 | 交付 |
|---|---|---|
| **C04 / M1** | Agent 最小纵向链路 | 可复现 CLI Demo + Trace + Golden Cases |
| **C13 / M2** | Context、工具、可靠性与权限 | 具备初步投递基础，可解释故障与设计取舍 |
| **C23 / M3** | 服务化、评测、性能与工程交付 | CI + Docker + Eval + Benchmark + 项目作品集 |

> 默认执行 V2.2，不因为一篇新 JD、框架趋势或单条面经而推倒重来；确有证据的改动先记录决策，再修改。

## 下一步

从 **C00 项目契约与工程骨架** 开始。当前仓库仅初始化路线文档，尚不宣称实现任何功能或通过任何测试。
