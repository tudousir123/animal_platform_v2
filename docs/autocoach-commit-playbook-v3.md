# AutoCoach AI · C00–C14 逐 Commit 执行教程（V3.0）

> 当前采用 [V3.1 修订执行版](autocoach-v3.1-execution.md)，本文保留 15 张 Commit 详细任务卡。涉及 Python 面试、FastAPI、Eval/Trace 前置与答辩门禁时以 V3.1 为准；[原 V3.0](autocoach-v3-frozen.md) 仅为历史基线。此文是**计划/任务书**，不是完工证明。开始于 2026-10-09，15 个 Commit 均待验收。每个编号是一个阶段性提交目标，不意味着只能创建一次 Git 提交；开发中可拆小提交，但验收编号不变。
>
> **执行顺序**：需求 → 阅读书籍正文/实验 README → 不看实现先画设计 → 自己写核心逻辑 → Mock 单元测试 → 真实环境验证 → 面试闭卷复盘 → 提交；不为赶进度跳过设计。
>
> **关于小林 coding**：下列面试栏目使用[小林 AI 面试题目录](https://www.xiaolincoding.com/project/xiaolinnote.html)定位；每卡最后列出的都是**对应考点和本项目模拟追问**，不是声称逐字摘录的“小林原题”，实际刷题时从题库点开原题并把链接及自己的答案填进 `docs/interview/Cxx.md`。用户之前上传的89页《Golang面试题》属于第二阶段 Go 后端资料，本阶段不冒充 Agent PDF。小林公开题库为 Agent/RAG/LLM 工具调用/大模型工程等专题。

## 统一交付目录（按需创建，不要求 C00 一次写全）

```text
src/autocoach/         # 第一阶段 Python 包；runtime/ tools/ rag/ memory/ eval/
tests/                 # pytest
data/mock/             # 合成的车型与模拟库存，不能伪装为真实库存
data/knowledge/        # 许可明确、注明来源和时间的汽车资料
eval/cases/            # Golden Cases 与人工基准
eval/results/          # 可复现实验记录（敏感信息排除）
docs/adr/              # 方案取舍
docs/commits/          # 每次提交任务完成证据
docs/interview/        # 小林实际原题链接、自己的回答与追问
```

全阶段共享的完成定义（DoD）：(1) 有输入/输出/边界和失败条件；(2) 可复现的正常与异常测试；(3) 有运行命令和结果/日志；(4) 独立讲清模块及技术取舍；(5) 面试题答案指向代码；(6) 新资料、指标与库存均不造假。开源仓库不得提交 API key、客户隐私或真实未经授权资料。

## 快速导航

| 里程碑 | Commit | 必须获得的能力 |
|---|---|---|
| M1 | C00–C04 | 从模型 API 到 Agent Loop、Tools、Context |
| M2 | C05–C08 | Memory、RAG、检索优化和 MCP |
| M3 | C09–C12 | 销售评分、评测、Trace、迭代优化 |
| M4 | C13–C14 | 完整可展示 Demo 与简历 V1 |

---

## C00 · 可复现开发环境与模型调用

**业务需求 / 本次新增：** 构建 Python 3.11+ 工程，CLI 接收一句话，ModelClient 适配 Mock 与真实模型；配置从环境变量读取，不上传密钥。

**应该看什么：** book/chapter1.md；chapter1/web-search-agent/README.md（先观察离线轨迹）；chapter1/context/README.md（先读说明，能连 API 时运行）。Python：venv/uv、typing、dataclass、pytest、HTTP+JSON。

**自己先设计（不要直接复制答案）：** 先画从 CLI→ModelClient→Provider→Reply 的数据流；设计 Message(role,content) 与 ModelClient 接口，标注异常边界。

**验收门槛：** 运行 `python -m pytest`；Mock 用例无需网络可过；真模型完成1次请求并去敏保存输出；无 Key 有明确错误；检查 git diff 不含密钥。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** LLM API 中 system/user/assistant 的区别？一次请求包含什么消息？Mock 测试与真实模型验收各证明什么？

**本次提交产物：** `docs/commits/C00.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C00.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C01 · AI 客户模拟器：5轮真实对话

**业务需求 / 本次新增：** 设计预算20万、重视安全的模拟客户，与学员完成≥5轮对话；角色信息作为系统约束，客户不主动做销售教练。

**应该看什么：** book/chapter1.md；chapter1/context/README.md：在同一场景对照有/无 persona 上下文。

**自己先设计（不要直接复制答案）：** 设计 Scenario、Persona、Conversation 三个对象；区分客户公开信息和隐藏信息；列出3条不能透露的约束。

**验收门槛：** 5轮历史顺序正确；一条多轮脚本、两条角色越界测试；日志标记模型版本/提示词版本；不承诺概率性测试必过。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** Agent 与聊天机器人有什么区别？为何仅多轮对话不能证明 Agent 自主使用工具？如何保持角色稳定？

**本次提交产物：** `docs/commits/C01.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C01.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C02 · 手写最小 Agent Loop

**业务需求 / 本次新增：** 实现 model→tool calls→execute→observations→model；对多次工具调用、终止、最大步数和 ToolCall ID 关联提供确定行为。

**应该看什么：** book/chapter1.md 中 Agent/Harness/ReAct；chapter1/web-search-agent/README.md 的轨迹；选读 book/chapter5.md Agent Harness。

**自己先设计（不要直接复制答案）：** 纸上定义消息类型与 run_state；画成功、无工具、错误、超步数4条状态转移；先写 Mock 模型剧本。

**验收门槛：** Mock 固定2次工具调用能退出；未知/错误工具不会导致死循环；超过轮数返回终止原因；tool_call_id 完整匹配；pytest 覆盖4分支。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 什么是 ReAct？Function Calling 是谁实际执行函数？为什么要设置 max_steps？Workflow 与 Agent 的控制权区别？

**本次提交产物：** `docs/commits/C02.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C02.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C03 · 受控 Tool Registry 与车型工具

**业务需求 / 本次新增：** 注册 `get_car_specs`、`check_mock_inventory`；Schema 验参、工具白名单、调用超时、错误分类；库存为显式模拟数据。

**应该看什么：** book/chapter4.md 工具分类与设计；chapter4/execution-tools/README.md 离线 demo、schema/风险标识。

**自己先设计（不要直接复制答案）：** 设计 ToolSpec(name,description,input_schema)、Executor、ToolResult；哪些是只读操作？如何处理无法识别的车型？

**验收门槛：** 合法 JSON 正确执行；必填缺失、类型错误、未注册工具、超时均有自动测试；非法工具无副作用；输出标注 mock。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** Function Calling 与普通 API 有何关系？为何 JSON Schema 不足以构成安全边界？失败时该不该重试？

**本次提交产物：** `docs/commits/C03.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C03.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C04 · 上下文预算与压缩

**业务需求 / 本次新增：** 实现 ContextBuilder：system 保留、消息配对、token预算、必要时摘要/裁剪；区分模型窗口与实际计费和服务缓存。

**应该看什么：** book/chapter2.md；chapter2/context-compression/README.md（smoke）；chapter2/kv-cache/README.md（前缀机制）；chapter2/prompt-injection/README.md。

**自己先设计（不要直接复制答案）：** 设计优先保留规则；说明压缩可能丢失哪些客户约束；哪些文本不可信？写1份 ADR。

**验收门槛：** 构造长对话；压缩前后客户预算、隐藏意图与工具配对不丢；注入样例不能突破程序级工具白名单；记录 token 估算方法。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 上下文窗口与 KV Cache 是一回事吗？提示注入怎么发生？如何降低 compaction 信息丢失？

**本次提交产物：** `docs/commits/C04.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C04.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C05 · 会话与客户记忆

**业务需求 / 本次新增：** 分离会话消息、客户画像、长期训练记录；采用本地 JSON/SQLite 存储接口（暂不引入 MySQL），支持恢复和隔离。

**应该看什么：** book/chapter3.md 记忆；chapter3/user-memory/README.md；chapter3/user-memory-evaluation/README.md 选读。

**自己先设计（不要直接复制答案）：** 设计 SessionStore 与 MemoryStore、记忆写入条件、更新/删除规则、隐私字段；区分短期状态和知识库。

**验收门槛：** 重启恢复会话；两名学员绝不串记忆；客户预算改动后只保留最新标注值；损坏记录显式报错；pytest 正常/异常。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** Session、Memory、RAG 区别？何时写入记忆？如何抑制错误记忆污染？

**本次提交产物：** `docs/commits/C05.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C05.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C06 · 汽车知识库最小 RAG

**业务需求 / 本次新增：** 收集/自制许可清晰的车型参数文档，切分、embedding、索引、TopK 检索、带来源回答；与客户模拟角色隔离。

**应该看什么：** book/chapter3.md；chapter3/dense-embedding/README.md；chapter3/sparse-embedding/README.md；chapter3/retrieval-pipeline/README.md 前半。

**自己先设计（不要直接复制答案）：** 设计 Document/Chunk(source_id,version,offset) 与检索接口；列出版本、价格有效期、单位和引用策略。

**验收门槛：** 不少于10个自己核对答案的初始问题；正常能召回引用；缺失资料时不编造；索引可重建；保存小样本 Recall@K 基线。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** RAG完整流程？为什么 Chunk 不是越小越好？Embedding 与向量数据库分别做什么？

**本次提交产物：** `docs/commits/C06.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C06.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C07 · 检索优化与错误分析

**业务需求 / 本次新增：** 加 BM25/关键词、融合、Rerank 与 Query Rewrite（后者视基线决定），同一评测集对比，不为复杂而复杂。

**应该看什么：** book/chapter3.md；chapter3/retrieval-pipeline/README.md（fusion/rerank）；chapter3/agentic-rag/README.md 选读。

**自己先设计（不要直接复制答案）：** 写明需要优化的失败案例、候选方案及评测指标；区分召回错误与生成事实错误。

**验收门槛：** 固定测试集比较 dense、BM25、fusion、rerank；记录 Recall@5/TopK、引用正确率、响应成本；无提升如实写出并保留基线。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 混合检索为何可能改善召回？Rerank 排在什么位置？怎样判断幻觉来自检索还是生成？

**本次提交产物：** `docs/commits/C07.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C07.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C08 · 统一工具与最小 MCP 链路

**业务需求 / 本次新增：** 将知识检索与车型工具统一接口；单独起最小 MCP Server，客户端能发现、调用，并处理断连，不引入业务无关云服务。

**应该看什么：** book/chapter4.md；chapter4/perception-tools/README.md；chapter4/execution-tools/README.md；MCP specification。

**自己先设计（不要直接复制答案）：** 设计普通本地工具与 MCP 工具的适配层；选择 stdio 或 Streamable HTTP 并写取舍；定义权限边界。

**验收门槛：** 客户端能列出工具并调用1个本地 MCP 工具；未知名称、参数错误、进程退出至少3种失败有测试；普通 Tool 行为不回归。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** MCP 与 Function Calling、REST 区别？MCP Client/Server 如何协作？传输协议为何影响生命周期？

**本次提交产物：** `docs/commits/C08.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C08.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C09 · 销售训练评分 Workflow

**业务需求 / 本次新增：** 训练结束才触发评分；按需求挖掘/产品事实/异议处理/沟通策略定义 Rubric，结构化返回逐项证据和建议。

**应该看什么：** book/chapter7.md 中指标与 LLM Judge；chapter7/user-memory-system-evaluation/README.md；chapter7/tau2-bench-eval/README.md 的 verifier。

**自己先设计（不要直接复制答案）：** 设计 4 个评价维度、分值锚点、证据引用与无法评分状态；说明为何固定评分流水线不采用自主 Agent。

**验收门槛：** 正确格式与分值范围单测；缺对话/事实依据时无法评分；每项引用实际发言；构造好/坏两个对话的基准样例并人工复核。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** LLM-as-a-Judge 优点与局限？结构化输出如何验证？Agent vs Workflow 何时选？

**本次提交产物：** `docs/commits/C09.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C09.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C10 · 离线评测框架与基准集

**业务需求 / 本次新增：** 建立可复现 eval runner；知识/工具/角色/评分四类测试，目标50/30/20/20条，先小样本，再逐步扩充。

**应该看什么：** book/chapter7.md；chapter7/tau2-bench-eval/README.md；chapter7/agent-cost-analysis/README.md。

**自己先设计（不要直接复制答案）：** 设计 dataset schema、正确答案/工具调用轨迹、评判器、人工标注与版本锁定；避免把生成器当唯一判官。

**验收门槛：** 能 reset→run→verify→record；固定 seed/模型配置与结果文件；至少一次两版对照；报告样本量、错误分布、未完成指标。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** Agent 如何评价？任务完成率 vs 工具调用准确率？怎样避免数据污染和评测集过拟合？

**本次提交产物：** `docs/commits/C10.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C10.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C11 · Agent Trace、Token、延迟与故障注入

**业务需求 / 本次新增：** 记录每步 trace_id、模型请求/响应摘要、工具名/耗时/错误、终止原因、token及模型成本；脱敏。

**应该看什么：** book/chapter7.md 可观测性/成本；chapter7/agent-cost-analysis/README.md；chapter7/android-world/failure-attribution/README.md 选读。

**自己先设计（不要直接复制答案）：** 设计 Span/RunEvent schema；哪些字段需要掩码？区分 TTFT、总时长和每阶段耗时。

**验收门槛：** 模拟模型超时、工具404、检索空结果，可定位第一错误点；敏感信息不入日志；同次 run trace_id 串联全过程。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 如何定位多轮 Agent 长延迟？LLM token 使用量与实际账单为何可能不同？Trace 与普通日志区别？

**本次提交产物：** `docs/commits/C11.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C11.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C12 · 评测驱动的工程优化

**业务需求 / 本次新增：** 从失败记录选1–2个真实问题，修改 Prompt/检索/工具；比较基线和新版本，用回归集防止牺牲其他任务。

**应该看什么：** book/chapter9.md；chapter9/trajectory-verifier/README.md；chapter9/prompt-auto-optimization/README.md。

**自己先设计（不要直接复制答案）：** 记录失败→根因→最小变更→对照实验→是否接受；预设接受门槛，考虑回滚。

**验收门槛：** 固定输入和模型条件，对照前后指标+成本；保留失败案例和拒绝的方案；自动回归不过不合入主线。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 怎么证明修改有效？Prompt 调优为何可能过拟合？线上退化如何回滚？

**本次提交产物：** `docs/commits/C12.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C12.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C13 · 端到端可演示 AutoCoach

**业务需求 / 本次新增：** CLI 或轻量 Streamlit 展示场景选择→客户训练→知识查询→结束评分→报告；不开发复杂前端/账号体系。

**应该看什么：** 回顾 book/chapter1.md、3/4/7章；查看 AWS sales roleplay README 仅参考交互与模块，不照搬 AWS 依赖。

**自己先设计（不要直接复制答案）：** 画模块依赖图、业务时序图、隐私边界；明确客户模拟与评分分离，哪些部分是 Workflow。

**验收门槛：** 陌生人按 README 可在 Mock 模式运行；3条Golden Case端到端通过；断网/缺 key 提示清晰；提供3分钟演示脚本。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 3分钟讲项目如何展开？为什么不用 LangGraph？为什么选单 Agent+Workflow？最难的 Bug 是什么？

**本次提交产物：** `docs/commits/C13.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C13.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。


## C14 · 项目作品集与简历 V1

**业务需求 / 本次新增：** 补 README、架构 ADR、实验报告、录屏脚本、面试自问自答，形成真实可投递的 Python Agent 项目表述。

**应该看什么：** 复习书第1–4、7、9章章节总结；复习小林 Agent/RAG/工具调用对应题；阅读自己所有测试与 Trace。

**自己先设计（不要直接复制答案）：** 写清个人贡献、架构取舍、局限；准备3分钟和10分钟项目陈述与技术白板；只写已验证功能。

**验收门槛：** 一键运行/测试可复现；20个常见追问脱稿回答并指出代码；简历无未经测量数字；完成一次模拟面试后冻结V1。

**小林 coding 对应专题 / 面试复盘题（按题库目录找原题，下列是练习要点）：** 系统设计：失败恢复/成本/性能瓶颈/业务边界；为何不是套壳 Agent？如果流量10倍怎样演进？

**本次提交产物：** `docs/commits/C14.md`（需求、ADR链接、运行命令、通过与失败测试、证据）；`docs/interview/C14.md`（实际原题链接、自己的答案、对应代码路径）；对应源代码与 pytest 测试。**状态：[ ] 未开始 / [ ] 已通过**。

---

## 阶段验收与停止条件

- M1：Mock 固定工具循环能稳定结束，ToolCall ID/异常/上下文限制可验证；能独立重写最简 Loop。
- M2：模拟客户与知识教练分离；有合法知识来源、会话隔离、检索引用、最小 MCP 调用；有真实检索基线。
- M3：记录失败轨迹、评测版本与人工评分依据；至少一个改进有对照，失败改进不粉饰。
- M4：他人按文档复现，项目能演示；简历只写已经合并的功能、真实指标与本人贡献。

**C14 结束先制作并投递简历 V1，再评估是否开启 Go/MySQL/Redis 后端阶段；不要把第二阶段技术当成当前第一阶段门槛。**
