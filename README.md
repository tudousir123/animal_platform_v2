# AutoCoach AI — Python Agent 学习与求职项目

> **当前唯一执行基准：V3.0 冻结版（2026-10-09）**。路线已确认；**尚未声称完成任何新版本功能**。
> 仓库历史名 `animal_platform_v2` 暂保留，不代表当前业务主题仍是动物行为分析。

## 从这里开始

**[AutoCoach AI · Agent Book × 小林 coding V3.0 冻结路线与 C00–C14 Commit 地图](docs/autocoach-v3-frozen.md)**

以李博杰 [《深入理解 AI Agent：设计原理与工程实践》](https://github.com/bojieli/ai-agent-book) 为学习主线，**Python 从零实现可解释的 Agent Runtime**，贯穿汽车销售模拟培训场景；用小林 coding 的 Agent/RAG/工具调用面试题验证理解。

### 开发顺序

1. **Phase 1 / Python Agent**：C00–C14 → Agent Loop、Context、Tools、Memory、RAG、MCP、Eval、Trace → 可运行 AutoCoach Demo、面试复盘、**简历 V1**。
2. **Phase 2 / 后端强化（V1之后）**：Go、HTTP/SSE、MySQL、Redis、并发、部署、Go 基础面试 → **简历 V2**。

每个 Commit：**需求 → 教材/官方实验 → 自主设计 → 实现 → 测试/评测 → 小林 coding 题目追问 → 提交**。无证据不标完成。

### 第一阶段不做

不提前转 Go，不双线开发两个完整平台；不强推 LangGraph、复杂 Multi-Agent、语音/3D/云服务、微调与复杂前端；不编造评估结果。不影响毕业论文优先级。

### 当前进度

- [x] V3.0 路线和学习顺序确定
- [ ] C00 Python 环境、模型连接、最小 CLI 和测试
- [ ] C01–C14 的代码/实验证据
- [ ] 简历 V1

## 历史文档（保留但不再是当前任务）

原仓库路线以 **Go + 动物行为分析** 为主线，现已被此次明确决策取代。下列文档保留供核对和以后后端阶段复用，**不要据此开始 Go 开发**：

- [V2.2 C00–C23 地图](docs/commit-map-v2.2.md)
- [旧最小可就业路线](docs/roadmap-minimum-employment.md)
- [旧学习手册](docs/learning-playbook.md)
- [旧首次开发任务书](docs/first-session-2026-10-10.md)
- [旧面试知识集](docs/interview-core.md)
- [旧动物行为 Golden Cases](eval/golden_cases/README.md)

下一次开发请直接阅读 [V3.0 冻结路线](docs/autocoach-v3-frozen.md) 并从 **C00** 开始。
