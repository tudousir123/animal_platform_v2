# AutoCoach AI · Agent Book × 小林 coding 学习路线 V3.0（冻结版）

> 历史版本：2026-10-09 V3.0。**已由 [V3.1 当前执行版](autocoach-v3.1-execution.md) 修订，本页用于版本追溯，不再是当前执行准则。**
> 项目正式名称为 **AutoCoach AI**；当前沿用已有 GitHub 仓库地址。

## 目标与边界

第一阶段：以 [李博杰《深入理解 AI Agent》](https://github.com/bojieli/ai-agent-book) 为唯一核心教材，**Python 自研轻量 Agent Runtime**，实现“汽车销售模拟培训 Agent”；结合小林 coding 的 Agent / RAG / 工具调用面试专题完成代码与面试验收，提交可运行作品、架构和评测证据、简历 V1。

第二阶段：在简历 V1 完成后再补 **Go / HTTP / MySQL / Redis / 并发与调度 / 部署**，形成简历 V2。此前上传的《Golang面试题 _ 小林coding.pdf》（89页）属于第二阶段的 Go 面经资料，而非第一阶段的 Agent/RAG 面经。

不提前上复杂多 Agent、云服务、语音/3D、预训练/微调、复杂前端；不以 LangGraph 替代自研主循环；不编造效果数字。论文与毕业任务优先。

## 项目业务闭环

1. 选择模拟场景：车型、预算、客户画像、客户顾虑与难度。
2. Python Agent 模拟客户，通过多轮对话保持人物设定和状态；必要时调用车型/库存等受限业务工具。
3. 知识教练通过汽车资料 RAG 解答业务问题、引用来源；模拟客户与知识教练共享数据，但权限和提示词分离。
4. 训练结束后，固定工作流生成结构化评分与证据；评分不是“模型自评有效”的证明。
5. 用人工核验的 Golden Cases、工具调用测试、检索指标、角色一致性、Trace/成本/失败归因支持简历表述。

## 每个 Commit 的固定闭环

**需求 → 定向学习（精确书籍章节/官方实验）→ 自主设计（接口/状态/失败）→ 实现 → 自动测试+评测 → 小林 coding 原题及追问 → Git 提交**。

完成定义：至少含需求/方案、实际可复现的代码、正常与异常测试、运行证据、能指到自己代码的面试解释；未完成不得勾选。面试题具体标题需在对应 Commit 教程中核对原文，不凭空编号。

## 15 个 Commit（第一阶段）

| Commit | 业务任务与输出 | Agent Book 对应 | 面试验收主题 |
|---|---|---|---|
| C00 | Python 环境、模型适配器、pytest、CLI 骨架；Mock 与真实 API | 前置知识、第1章及 chapter1/context 实验 | LLM API、消息结构、JSON/HTTP |
| C01 | 命令行 AI 模拟客户；多轮对话与角色约束 | 第1章 | Agent vs Chatbot、Prompt |
| C02 | 自研 Agent Loop：模型→工具→观察→模型，终止/最大轮数/Tool Call ID | 第1章、参考第5章 Harness | ReAct、Function Calling、Loop 安全 |
| C03 | Tool Registry、JSON Schema/参数验证、车型查询、异常处理 | 第4章（提前学工具基础） | 工具选择、Schema、错误和权限 |
| C04 | Context Builder、Token 预算、压缩、稳定前缀及多轮回归 | 第2章 / context-compression | Context、KV Cache、Prompt Injection |
| C05 | Session/短期状态、客户画像、长期记忆的边界及恢复 | 第3章 / user-memory | Session vs Memory vs RAG |
| C06 | 汽车文档导入、Chunk、Embedding、向量检索、引用式问答 | 第3章 | RAG 流程、切片和向量检索 |
| C07 | 关键词+向量混合检索、Rerank、评测与引用约束 | 第3章 | Recall@K、重排、幻觉与优化 |
| C08 | RAG/车型工具纳入统一接口，最小 MCP 发现/调用/异常链路 | 第4章 / execution-tools | MCP vs Function Calling、传输与协议 |
| C09 | 固定销售评分 Workflow、Rubric、结构化输出、证据 | 第7章基础 | Agent vs Workflow、LLM-as-Judge |
| C10 | 知识50题、工具30例、模拟20例、评分20例为**目标规模**，先从小样本开始；人工核验 | 第7章 | Eval 指标、数据泄漏、人工一致性 |
| C11 | Agent Trace、Token/延迟/调用失败、敏感信息过滤 | 第7章 | Agent 可观测性、故障排查 |
| C12 | 根据失败案例改进 Prompt、检索、工具与记忆；可复现对照 | 第9章 | A/B、回归、优化证据 |
| C13 | CLI 或轻量 Streamlit 完整训练 Demo，模块整合 | 第1–4/7/9章综合 | 架构图、模块职责、端到端系统 |
| C14 | README、运行说明、测试报告、架构取舍、演示与简历 V1；开始试投递 | 全阶段复盘 | 项目3/10分钟陈述与追问 |

### 里程碑
- **M1 C00–C04**：可解释且有失败测试的 Python Agent Loop。
- **M2 C05–C08**：Memory、RAG、工具和最小 MCP 完整链路。
- **M3 C09–C12**：评测体系、Trace、改进证据。
- **M4 C13–C14**：可展示作品与真实的第一版求职简历。

## 学习资料固定映射

- [官方书籍目录](https://github.com/bojieli/ai-agent-book)；[作者学习建议](https://github.com/bojieli/ai-agent-book/blob/main/docs/zh-CN/LEARNING.md)。
- 第一批配套实验：`chapter1/context`、`chapter2/context-compression`、`chapter3/user-memory`、`chapter4/execution-tools`；其他按对应 Commit 使用。
- [MCP 规范](https://modelcontextprotocol.io/specification)；[Python asyncio](https://docs.python.org/3/library/asyncio.html)、[typing](https://docs.python.org/3/library/typing.html)、[pytest](https://docs.pytest.org/)。
- 小林 coding：以实际可访问的 Agent/RAG/工具调用专题原题为准，逐 Commit 建题目、回答、对应代码行号、追问。Go PDF 保留到第二阶段。
- 架构/业务对照：[AWS AI Sales Roleplay](https://github.com/aws-samples/sample-ai-sales-roleplay)、[AI Sales Roleplay](https://github.com/kitayama-ai/ai-sales-roleplay)；借鉴业务流程，**不直接移植大型 AWS 架构**。

## 验收与真实简历

简历 V1 只能写已经提交、可测试、可复现的技术能力，不得写未测出的性能提升或“生产级”。固定收集：
- Trace：模型和工具调用路径、耗时、终止原因、失败类别；
- RAG：带标签问题、Recall@K、答案引用准确性、失败案例；
- 工具：正确选择/参数成功率/断连或异常回退；
- 评分：人工 Rubric 样本、一致性检验，避免自评即自证；
- 项目故事：需求、架构、取舍、测试证据、局限和下一步。

## 第二阶段（不阻塞简历 V1）

基于已经完成的 Python Agent 项目补 Go 服务端、HTTP/SSE、MySQL 事务/索引、Redis 缓存/任务、并发和幂等、CI/Docker、Go 面试题。Go 后端强化将在简历 V1 完成后单独设计，不阻塞当前 C00–C14。

## 变更控制

这是用户已确认的 **V3.0 冻结基准**。除真实技术障碍、可靠面试反馈或毕业时间重大改变外，不修改项目主线、Python优先顺序、首个交付物或面试验收标准。允许局部修补单个 Commit，不允许见到新框架就重做路线。

**下一步只做 C00：书第1章 + 官方 chapter1/context 实验；写 Python 模型请求、最简 CLI、pytest 正常/失败测试，留下第一次真实运行日志。**
