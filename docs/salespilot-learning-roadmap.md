# SalesPilot Learning Edition · AI Agent 应用开发课程 V1.0

> 当前唯一执行路线（替代 AutoCoach V3/V3.1）。目标：以 [FelixDemon1/SalesPilot](https://github.com/FelixDemon1/SalesPilot) 的新能源汽车销售**辅助工作台**为业务规格，从零独立实现一个可运行、可评测的 Python Agent 应用。不是客户扮演/模拟培训项目。原仓库目前未发现显式顶层 LICENSE，因此仅**参考公开代码与架构**；未经授权不复制原源码、不声称其代码是自己的。历史设计可在 Git 历史中追溯。

## 一、三种教材，一条开发主线

- **工程对标 SalesPilot**：需求、前后端接口、RAG、本地知识+实时网页搜索、Agent 深度研究、SSE、Session、持久化。已核对源码 `backend/app/service/agent/agent.py`、`backend/app/router/ai_serarch_rt.py` 和 README。README 已列出首次工具结果丢失、Reflection 不执行、固定 user_id 等待修复事项；这些是**对标检查案例，不是我们已修复的 Bug**。
- **原理主教材 [AI Agent Book](https://github.com/bojieli/ai-agent-book)**：第1章 Agent；第2章 Context；第3章 Memory/RAG；第4章 Tools/MCP；第7章 Eval；第9章失败驱动优化。原书作为本仓库 submodule 放在 `references/ai-agent-book/`，可离线阅读实验。
- **工程与求职 [AgentGuide](https://github.com/adongwanai/AgentGuide)**：开发岗路线、[Agent Harness](https://github.com/adongwanai/AgentGuide/blob/main/docs/02-tech-stack/27-agent-harness-engineering.md)、[如何落地一个可写进简历的项目](https://github.com/adongwanai/AgentGuide/blob/main/docs/03-practice/05-ship-agent-project.md)、[项目指南](https://github.com/adongwanai/AgentGuide/tree/main/projects)；只用来审查可靠性、证据和边界，不替换主课。
- **面试 [小林 coding](https://xiaolincoding.com/)**：Python 题库、Agent/RAG、MySQL/Redis/系统设计；按每个 Commit 做题、手写代码、结合项目答辩。具体原题必须核对链接；下表“考点”不是冒充题库原文。历史 PDF 暂未进入仓库，避免声称已经上传。

## 二、产品定义与不做事项

面向新能源车**销售人员的知识与研究助手**。输入客户的车型/预算/竞品问题，调用企业车型手册 RAG、必要时调用实时 Web Search，综合证据形成销售可用回答并给出来源；用户可查看历史记录、上传文档。高级模式做 Plan→Tool→Observation→Reflection→Answer；并提供最小 FastAPI HTTP/SSE 服务。初期以标注为模拟的“星辰电动 ES9”及许可明确的示例资料作为演示，避免无来源的价格和政策断言。

**明确不做**：原 AutoCoach 的模拟客户、销售训练评分、训练课程、声音/视频训练；不提前引入多 Agent、RL、复杂权限运营体系。MVP 不用一开始就安装 ES+PostgreSQL+OCR 全套基础设施；先 SQLite/本地检索、Mock Web Search，逐阶段替换。不得强制每个学习 Commit 都从头搭一个大平台。

## 三、系统结构（目标架构）

```text
CLI (早期)  / FastAPI + SSE (中后期) / 轻量前端 (最后)
                     |
                  API / Service
                     |
        +------------+-------------+
        |                          |
   Ordinary RAG Q&A           Deep Research Agent
   local knowledge            Plan→Tool→Observe→Reflect→Answer
   optional fresh Web         iteration budget / error gates
        +------------+-------------+
                     |
    document ingest / retrieval / search adapters
         session store / trace / evaluation
```

Agent Runtime 不能依赖 FastAPI；普通问答走清晰的固定 Workflow，深度研究才需要可迭代 Agent。这是关键设计与面试取舍。

## 四、每个 Commit 的开发闭环

每次只进行当前编号：**业务需求 → 阅读原书指定章节和一个最小官方实验 → 自主设计接口与异常 → 你写关键代码 → pytest+Mock → 小样本真实模型/搜索验证 → 记录 Trace/Eval → 小林 Python+Agent 口头答辩 → Git 提交**。

必要工件：`docs/commits/Cxx.md`（需求、独立设计、技术取舍、测试命令、实际结果、失败样例和 SHA）、`docs/interview/Cxx.md`（核对过的原题链接、自答、追问）、`tests/`、`eval/cases/` 和 `eval/results/`（按需逐步创建）。禁止在未执行测试时写“已通过”。

## 五、Commit 地图（按顺序验收）

| Commit | 需求和你要独立设计什么 | 原书/官方实验及 AgentGuide | 通过什么证明完成 | Python/Agent 面试考点 |
|---|---|---|---|---|
| **C00 项目与调用** | Python 3.11+、uv/venv、配置、ModelAdapter、最简 CLI；画 CLI→Service→Provider 边界 | Book ch1、`chapter1/web-search-agent` 离线轨迹；Guide 求职项目 Spec | Mock 正常/无 Key/超时测试；1次去敏真实 API 调用；配置不泄露；首批3个 Golden Cases | 对象引用、dict、异常、LLM 消息结构、Mock 与集成测 |
| **C01 汽车问答业务** | 用模拟车型数据做咨询回答；定义问题类型、资料版本、可答/不能答 | Book ch1、`chapter1/context` | 5条业务查询正确路由到对应静态答案/拒答；记录模型+提示词版本 | 函数/参数/可变默认值；chatbot vs agent |
| **C02 资料与引用** | Document/Chunk/source id，读取 Markdown/文本，最小规则分块及引用标记 | Book ch3、`chapter3/sparse-embedding`，Guide RAG 项目规范 | 3份示例资料可导入与重建；文档缺失/无证据时不捏造；原文来源可追溯 | 文件 I/O、生成器、列表与哈希；RAG 为什么要可追溯 |
| **C03 基线检索** | BM25/关键词检索+答案；定义 top-k、数据与评测标签 | Book ch3、`chapter3/sparse-embedding` | 10题人工核验 Recall@5 基线；无匹配拒答；正常/边界 pytest | BM25、分词、chunk、倒排索引、dict |
| **C04 Dense + Hybrid RAG** | Embedding、向量相似度、BM25+Dense 融合、按失败决定是否 Rerank | Book ch3、`chapter3/retrieval-pipeline` | 同一测试集 dense / sparse / hybrid 对照；指标、耗时、引用支持率与失败分析 | Embedding、ANN、RRF、Rerank、评测污染 |
| **C05 Web Search** | SearchAdapter 与 Mock / 实时来源，时间戳、域名、失败回退、注入防护 | Book ch4（工具）、ch2（注入），Guide Tool/Harness | 正常、无搜索结果、限流、超时4个场景；企业资料与网页来源分开标记 | HTTP/JSON、请求超时、重试、信息可信度 |
| **C06 自研工具与 Loop** | ToolSpec/ToolResult/Planner、Tool Registry、Plan→Action→Observation→Answer，有限轮数与 ToolCall ID | Book ch1/ch4、`chapter4/execution-tools`、Guide Harness | Mock 2次工具调用稳定退出；未注册工具/非法参数/循环上限测试；Trace 能复现 | ReAct、Function Calling、JSON Schema、装饰器 |
| **C07 Reflection 深度研究** | 判断证据是否充分，最多追加有限次检索；区分补查与重复死循环 | Book ch2/ch9、`chapter9/trajectory-verifier` | 3条可判断需不需要补查的案例；补查最终能停止；重复查询被阻止 | Agent vs Workflow、状态机、上下文压缩 |
| **C08 会话与存储** | SessionService、消息持久化、知识文档元数据；先 SQLite，分离 Memory 与会话 | Book ch3、`chapter3/user-memory` | 重启恢复会话，隔离不同用户；数据修改与删除有测试；不能硬编码固定 user_id | OOP、数据库事务、Python context manager、Session vs Memory |
| **C09 FastAPI API** | /health、/sessions、/chat、/research；Pydantic 请求响应模型，异常转换和依赖注入 | Book ch4 工具接口；FastAPI 官方；Guide 应用交付 | TestClient 无模型也能测接口；4xx/5xx/超时语义合理；API 与核心业务解耦 | ASGI vs WSGI、FastAPI、依赖注入、asyncio/GIL |
| **C10 流式 SSE** | 普通问答和深度研究分阶段事件，协议、结束/异常/客户端断开 | Book ch2/ch7、Guide Trace/成本；SalesPilot SSE 设计参考 | SSE 模拟客户端收有序事件与 [DONE]；中断不泄露资源；记录首 token 与总耗时 | yield/async generator、Event Loop、SSE vs WS、取消 |
| **C11 可靠性 + MCP选修** | 工具超时/取消、有限重试、权限白名单、注入/输出截断；有余力加一个最小 MCP client | Book ch4、`chapter4/execution-tools`、Guide Harness 安全 | 注入不越过程序级权限，重试不重复有副作用操作；MCP 未做不得写已做 | GIL、协程、超时取消、MCP vs FC、幂等 |
| **C12 Eval / Trace** | 贯穿 C00 的 Golden Case 汇总；组件/轨迹/端到端指标，模型成本、错误归因、改善前后比较 | Book ch7、`chapter7/tau2-bench-eval`；Guide 项目验收 | 最低20条人工核对端到端/组件组合案例；至少1个明确失败归因+修复对照；报告配置和分母 | RAG Eval、LLM Judge、Recall@K、Trace、A/B |
| **C13 可展示作品与求职** | 选做轻前端/Swagger Demo、Docker Compose（可控）、文档、录屏脚本与简历 V1 | Book ch9、`chapter9/prompt-auto-optimization`；Guide 求职指南 | 无外部 API 时 Mock 可启动；带凭据真实演示；3/10分钟可讲清架构、性能与失败；20道关键题闭卷模拟 | Python 高频、Agent/RAG、API、项目设计和 trade-offs |

## 六、测试与学习数据

**每 Commit 的硬门禁**：正常+异常测试可复现、代码可解释、文档与 SHA 对应、不含密钥，至少2道 Python/Agent 面试题能在代码中找到依据。即使 Mock 通过也不能冒充真实 API / 线上成功。

**建议样本（均是尚未执行的目标）**：C03 先10条人工标注 RAG 问题；C12 拓展≥20条业务/工具端到端用例、至少10条注入/故障用例；再视资源扩充至50+条知识问答，保存测试集版本。对于小样本只报告精确分子/分母与置信边界，不虚构“95%生产成功率”。

**推荐报告字段**：模型/版本、温度/参数、提示词版本、数据集版本、调用次数、工具选择正确率、检索 Recall@5、引用支持率、未支持声明数、任务完成数/总数、延迟分布、Token/成本、失败类别。业务指标定义先于实验，不成功也保留轨迹。

**面试能力评分**：0不会、1复述、2能解释原理、3能结合自己的代码讲取舍、4能回答故障与变体；每 Commit 题目目标≥3，C13 随机20题至少16题≥3。小林公开题库是题源，具体原题链接需逐个核对，不制造“牛客全站频次”。

**投入规划**：每周15–20小时，可用8–10周作规划参考；毕业论文优先；进度按完成测试推进而非按日期追工期。

## 七、版本管理与资料

- 原 AutoCoach 客户模拟与培训评分**已撤销**，其旧文档从当前分支删除；Git 历史仍可找回。
- `references/ai-agent-book` 继续保留为已关联的完整官方教材。
- SalesPilot 官方源码可通过链接学习，但未经许可不复制；AgentGuide 以官方链接和学习笔记方式使用。
- 所有个人上传的面试 PDF 在仓库确认 Private、并具备相应使用权限之前不上传。仓库目前 GitHub API 仍返回公开。
- 下一步从 **C00** 开始，不提前搭完整 ES / PG / OCR / React 全栈。
