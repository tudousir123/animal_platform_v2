# 六个能力里程碑：边学边造 SalesPilot

> **没有固定 Commit 数量；M0–M5 是验收节点，不是每阶段一个提交。** 如需拆解，按最小可测试能力自然做小提交。M0–M5 的顺序是依赖路径，不是死板日历。

## 通用学习方法

每个小任务执行：需求 → 看 AI Agent Book 指定章节与官方实验 → 先画你的设计（接口/状态/正常+失败）→ **自己实现核心代码** → Mock/pytest → 实际服务验证（如果有条件）→ 记录指标与失败 → 根据[小林原题映射](05-interview-map.md)闭卷答辩 → Git 提交。

我负责提出约束、给原文入口、评审设计、设计测试、解释报错、进行面试追问；不要在用户尚未设计时一次性生成完整核心实现。需要示范时给局部、最小且解释充分的代码。

## M0 · 明确需求、环境与基线（不是搭架构）

**任务**：阅读 [产品规格](01-product-spec.md) 与 [SalesPilot 源码审计](02-source-audit.md)，画出普通问答与研究 Agent 两条业务路径；挑选3条常规、2条失败的案例写 Golden Case，选 Python3.11+、venv/uv、pytest 与一个模型提供商。设计 `ModelClient` `ToolSpec` `ToolResult` 的最小形态，不创建空的大量目录。

**阅读实验**：`book/chapter1.md`、`chapter1/web-search-agent/README.md` 的免 Key 离线执行轨迹、AgentGuide [Harness](https://github.com/adongwanai/AgentGuide/blob/main/docs/02-tech-stack/27-agent-harness-engineering.md) 的 L1–L3。

**独立设计**：为什么普通问答是 Workflow 而复杂研究是 Agent？怎样区分模型声明的动作与真实执行的工具结果？

**验收**：需求例 B01/B03/B07/B08/B09 有输入+期望动作与证据来源；初始代码环境的 Mock 测试可独立运行；至少一个失败路径；密钥不入仓库。没测过不标通过。

**闭卷面试**：Agent #1、#3、#13；Python #8.6、#8.8、#7.4。

## M1 · 最小 Agent 横切纵向完整链路（第一可展示节点）

**业务**：用户问演示车规格 → 模型返回结构化 tool_call → 宿主注册/校验/执行 `lookup_demo_vehicle` → tool result 带 id/来源回到消息历史 → 模型终答。CLI 必须运行；FastAPI `GET /health` 与 `POST /chat` 可此阶段顺手接上，不能阻塞 CLI。

**学习**：`book/chapter1.md`、`chapter1/context/README.md`（消融：没有工具输出会怎样）、`book/chapter4.md`、`chapter4/execution-tools/README.md`；AgentGuide L1–L3 实现示意。Python：函数参数、dict/dataclass/Pydantic、装饰器、异常、mock。

**自己设计**：`ModelClient`、`ToolRegistry`、`RunState`、`ToolCall`、`ToolResult`；状态图要含无工具、正常执行、未知工具、非法参数、超步数与重复循环。

**交付与验收**：固定 Mock 至少覆盖 6 类状态分支；每次 ToolCall ID 对应原 ToolResult；模型最多调用设定次数；失败不伪装成功；Trace 最少含 run_id/step/tool/result/terminal_reason；真实模型端到端进行1次去敏冒烟验证，没真实 Key 则写“待测”。**能从零解释并重写最简 Agent Loop 才过关。**

**闭卷面试**：Agent #2、#5、#13、#20；Tools #1；Python #4.3、#4.8、#7.1、#8.8。

## M2 · 企业资料 RAG，替换模拟车型工具数据源

**业务**：将自制新能源汽车产品 Markdown/PDF 文本样例导入、切分，构建文档版本、source_id、chunk位置；实现最小关键词/稀疏检索与有引用问答，然后根据基线测得的问题选 Dense/Hybrid/Rerank。第一版可使用纯文本或可解析 PDF，不强制 DeepDoc OCR。

**阅读实验**：`book/chapter3.md`，`chapter3/sparse-embedding/README.md`，`chapter3/retrieval-pipeline/README.md`（先 `python evaluate.py --no-dense --no-rerank`；后再尝试带模型的对照）。AgentGuide 项目 Spec/Eval。

**独立设计**：Document/Chunk/SourceCitation、Query/RetrievalResult、索引重建协议、文档新旧版本冲突策略、无证据与有证据的回答分界。

**验收**：三份有明确“演示资料”标记的文档可建索引；初始至少10条人工核对的查询集，包含精确车型代码与语义改写；输出 Recall@5 的实际分子/分母、检索耗时和答案引用支持情况；缺资料时不得编造。复杂检索组件只有对照能解释收益/代价才引入。

**闭卷面试**：RAG #1、#4、#6、#11、#13、#17、#18、#21；Python #5.1、#5.4、#8.9。

## M3 · 真实 Web Search 与证据驱动深度研究

**业务**：统一 `search_local_docs` 与 `search_web` 工具契约；对比竞品及公开政策时获取可追溯网页结果；Agent 根据证据缺口有界补查，区分确定事实、出处不一致、无依据。普通问答走轻量 Workflow，复杂问题走研究 Agent，不能为了使用 Agent 每题多调模型。

**阅读**：`book/chapter4.md`、`chapter2/context-compression/README.md`、`chapter9/trajectory-verifier/README.md`。AgentGuide [项目落地流程](https://github.com/adongwanai/AgentGuide/blob/main/docs/03-practice/05-ship-agent-project.md)。

**独立设计**：SearchResult(title,url,snippet,observed_at,source_type) 与 ToolError；如何判断新增查询 vs 重复查询；外部网页内容为什么不可信；输出如何引用日期。

**验收**：正常 Web、缺 Key、HTTP 429、超时、空结果、检索重复、来源冲突和注入至少8个可复现路径；可核实真实搜索时间与URL；最大步数/工具调用预算和独立停止条件；记录每条答复引用来自内网还是公开网页。**明确测试原项目“首条结果丢失”和“反思动作未执行”两种失败模式，证明自己的实现不会重犯。**

**闭卷面试**：Agent #6、#14、#15、#20、#23；Tools #6、#17、#18；Python #7.4、#9.2、#10.10。

## M4 · FastAPI 服务化、会话、流式与可靠性

**业务**：CLI 仍保留；FastAPI / Pydantic 暴露 chat、research、session；引入 SQLite 或合适的轻量存储，严格用户/会话隔离；普通回答/研究阶段事件可以通过 SSE 输出；工具取消和外部超时有界执行。

**阅读**：FastAPI 官方 https://fastapi.tiangolo.com/tutorial/ ；Python asyncio https://docs.python.org/3/library/asyncio.html ；AI Agent Book `book/chapter2.md`、`chapter2/context-compression`、`chapter3/user-memory`；AgentGuide Harness 的权限与上下文段落；原 SalesPilot SSE/API 设计**只作阅读参考**。

**独立设计**：业务 core 与 HTTP 的依赖方向；事件 `start/tool/answer/error/done` 的 Schema；Session 的归属、清理和恢复；异步中同步 SDK 阻塞如何处理；哪些工具允许重试，哪些不行。

**验收**：TestClient 无付费凭据能验证 API 输入/输出、400/404/超时、会话隔离与恢复；SSE 事件顺序可解析、失败有终止事件、断连能取消并释放资源；日志不写密钥或原始隐私；定义并量测少量场景延迟与调用成本。**不要求上来就 PostgreSQL/Redis/ES**。

**闭卷面试**：Python #10.1、#10.4、#10.6、#10.7、#10.8、#10.10、#10.11、#9.3、#9.5；Tools #14；Agent #18、#21、#24。

## M5 · Eval、优化证据、部署演示与简历 V1

**业务**：评估组件检索、工具轨迹与端到端任务；挑出真实失败，针对最小改动进行对照；提供可运行 README、架构图、演示脚本、API 示例与3/10分钟项目介绍。

**阅读**：`book/chapter7.md`、`chapter7/tau2-bench-eval` 的验证思想（注意外部环境依赖）、`book/chapter9.md`、`chapter9/trajectory-verifier`；AgentGuide [简历级项目标准](https://github.com/adongwanai/AgentGuide/blob/main/docs/03-practice/05-ship-agent-project.md)。

**独立设计**：人工标注的预期轨迹、失败类型/等级、评测数据锁定策略、Baseline/Change 的版本号和配置，成功是否等于真实工具执行完毕，引用是否支持主张。

**验收**：至少20条跨组件/端到端的标注案例（建议知识10、工具与研究各5，实际按业务风险增加），10条可靠性/安全用例；**全部报告实际分子/分母，不以20题的百分比代表真实泛化性能**；至少一次真正的失败归因和修复/消融对照，保存真实 Trace/模型及数据版本。Mock 模式独立运行，真实模型服务演示若未完成必须标待测。20道跨专题面试题中至少16道达到3/4，关键模块不能失语。

**闭卷面试**：Agent #19、#20、#21、#23；RAG #18、#20；Python #11.7、#12.5；以自己的代码解释架构、错误、优化和所有未做功能。

## 选做与第二阶段：按证据决定，不强制

- **MCP 小实验**：本地 `search_local_docs` 已稳定，且要对外复用工具时做一次 tools/list 与 tools/call；不能把“接过函数”写成“已接入 MCP”。
- **React**：有可用 Swagger/CLI 后再决定是否增加轻 UI，复杂前端会分散时间。
- **PostgreSQL/Elasticsearch/Redis**：当持久化、全文索引规模、队列/缓存等现实需求出现，写 ADR 再替换；若目标 JD 明确要求，可作求职补强。
- **Go**：简历 V1 和毕业任务不受其阻塞；Go/Redis/MySQL/分布式资料保留第二阶段或专项面试训练。

所有里程碑以功能验收为门禁，而不是用“本周必须完成三次提交”来制造进度。每周建议只给 Agent 项目**约15–20小时上限**；遇论文截止期限缩短，不以熬夜赶人工日期。
