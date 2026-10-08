# Agent Runtime 工程化求职路线图 V2.2（冻结执行版）

> 基线日期：2026-10-08｜状态：**冻结执行**（允许根据项目证据做小幅补丁，不因单篇帖子重构）  
> 项目主线：**Go + Pi-inspired Agent Runtime + 真实动物行为分析业务**  
> 面向岗位：Agent Runtime / Agent Infra、AI Agent 研发、AI 应用工程师；不以预训练/SFT/RL 算法研究岗为主。  
> 路线编号：**C00–C23，共 24 个 Commit**；C04 能演示；C13 初步可投递；C23 完整作品集。  
> 学习顺序：**先 Agent 纵向链路，后工程基础设施**。开发不能挤占毕业论文底线。

## 0. 为什么冻结这版

这次调研参照了用户提供的 AI Agent 研发 JD，以及已讨论的牛客网 Agent Infra、Agent 应用、服务端开发等 JD/候选人面经。归纳结论仅作**定性指导，非招聘市场统计**：

| 从 JD / 面经提取的能力 | 我们的应对 | 边界 |
|---|---|---|
| Agent Loop、Harness、ReAct / Planning、状态和工具调用 | C01–C04 是项目主心骨 | 不以调用 LangChain API 代替独立实现 |
| Context、Session、Memory、Compaction | C05–C06、C12 | 区分短期状态、长期记忆、RAG 文档检索 |
| Tool、MCP、权限与异常处理 | C02、C07、C09、C13 | MCP 先做一条协议完整链路 |
| RAG 评测、测试集可信度与设计取舍 | C08、C20 | 不追求漂亮但不可信的数字 |
| Debug、Trace、Eval、可靠性 | C00 定案例；C04 存轨迹；C10–C13 注入故障；C19–C20 系统化 | 不等最后才开始评测 |
| MySQL / Redis / 异步 / 并发 / 幂等 | C14–C18 | 不急着上 Kubernetes、Kafka、微服务 |
| Python 岗位门槛、手撕代码、基础八股 | C04-P + 并行训练线 | **不维护第二套完整 Python 平台** |
| 多 Agent、规划式复杂流程 | 先理解设计取舍，C23 后按实际需求扩展 | 不阻碍主线验收 |

## 1. 项目描述与系统边界

**目标交付**：一个 Go 实现的、可调试/可复现/可恢复的 Agent Runtime。用户以自然语言提出动物行为分析需求，Agent 能按 Schema 选择并调用受限业务工具、维护上下文与 Session、必要时查询知识库，然后生成有来源的结构化结果；工程化阶段支持异步任务、日志追踪、权限控制及测试评估。

**从已有研究业务抽出 3 个端到端任务**（测试用例内容应以真实可用的数据/接口修订）：

1. 给定实验/视频 ID，调用现有分析结果服务，返回指定行为指标及解释。
2. 查询 AD / sham / 药物处理组的已存在结果，执行合规的组间比较并附数据来源（不虚构统计显著性）。
3. 根据本地实验手册、范式说明和真实分析输出，回答解释性问题；证据不足时明确拒答或提示缺失。

**不重造视觉模型**：现有视觉推理能力作为被调用工具/服务，先用 Mock，后接实际接口。不要把 Agent 项目变成新的视频算法研发项目。

### 1.1 逻辑架构

```text
CLI 交互（C00–C13）                    HTTP/SSE API（C14 起）
              │                              │
              └──────────┬───────────────────┘
                         ▼
                 Agent Runtime (Go)
              ┌──────┼──────────┐
          Model Adapter  Context / Session   Tool Registry
          (LLM / Mock)   Memory / Compaction   ├─动物行为业务工具
                                             ├─RAG 工具
                                             └─MCP Client
                   │
          Trace / Eval / Permissions
                   │
            Job & Worker (C14–C18)
          MySQL / Redis Streams / Outbox
                   │
             Docker / CI / Benchmark
```

**技术栈约束**：核心语言 Go；Python 用于一个轻量异步 Agent 练习（C04-P）和已经存在的视觉服务，不改成双技术栈全面开发。MySQL/Redis 在出现真实状态与排队需求后引入。对 LangGraph 等框架采取“学其设计取舍，不替换内核”的策略。

## 2. 如何做每个 Commit：强制闭环

每个 Commit 必须同时有工程资产和面试证据；不接受“代码是 AI 生成的，但自己讲不清楚”。

1. **需求**：谁需要这项能力？当前系统为何不能解决？写出输入、输出、边界、失败情况。
2. **先独立思考**：不参考完整实现，写最小方案与状态/接口草图；明确自以为是的未知。
3. **定向学习**：只看本 Commit 相关的官方文档/源码与 1–2 篇针对性文章。
4. **设计**：记录 1–2 个可替代方案、所选方案及取舍，形成短 ADR。
5. **编码**：优先最小实现与 Mock，真实依赖随后替换；代码需要自己过一遍。
6. **验收**：至少一个正常、一个边界/失败的自动化测试；保留运行命令与复现材料。
7. **面经复盘**：记录 2–3 道面试题，并写自己的答案和相关代码位置。
8. **3 分钟表达**：脱稿回答“解决什么问题—为什么这样设计—怎么验证—不足是什么”。

**完成定义 DoD**：设计文档、可运行代码、可自动运行测试、复现说明、面经问答五者齐备；否则不进入下一 Commit。难度大时可切分实现任务，但不增加新的主线编号。

建议仓库维护：`README.md`、`docs/roadmap.md`、`docs/adr/`、`docs/commits/Cxx.md`、`eval/golden_cases/`、`benchmarks/`、`docs/interviews/`、`CHANGELOG.md`。

## 3. 冻结 Commit 地图（需求｜学习｜验收｜面经）

### 阶段 A · Agent 纵向链路（C00–C04，P0）

| 编号 | 需求与实现 | 定向学习 | 验收标准 / 证据 | 面试复盘 |
|---|---|---|---|---|
| **C00 基础契约** | 建项目、领域模型 `Message/ToolCall/ToolResult/RunState`，明确 3 个业务任务和数据来源；写核心接口与运行协议 | Go modules / testing；阅读 pi-mono 项目组织 | `go test ./...` 可执行；Fake Agent 状态可序列化；3 份 Golden Case 草案 | 什么是 Agent Runtime/Harness？为何不直接用 LangGraph？ |
| **C01 Model Adapter + CLI** | LLM Client 抽象、Mock/真实模型切换、简单 CLI、超时/错误结构 | Go interface、JSON、HTTP、Go `context` | 能进行至少一轮模型交互；断网/超时/无效响应测试通过 | Provider Adapter 应如何设计？超时如何传递？ |
| **C02 Tool Registry** | 工具 Schema、注册、参数校验、未知工具/异常处理、Tool Result 标准格式 | JSON Schema、Function Calling、错误封装 | 合法工具执行，错误参数明确返回，不执行未注册工具；单测齐全 | 工具调用为何需要 Schema？如何处理幻觉工具名？ |
| **C03 Pi-style Agent Loop** | `model→tool→observation→model`；多轮工具调用、Tool Call ID 对齐、最大步数/终止原因、事件流 | pi-mono 的 agent loop 设计；ReAct 原理；Go 状态机 | Mock 固定 2–3 次工具调用可稳定退出；错误分支和循环上限有测试；画状态转移图 | ReAct 与 Plan-and-Execute 的取舍？为何 Loop 不能无限运行？ |
| **C04 首个业务 Demo** | 接一条动物行为分析工具链；真实结果或清晰标记的 Mock；记录每步执行轨迹 | 旧业务接口、领域指标定义、结构化输出 | 完成至少 3 个固定端到端任务并复现；保存 Trace + Golden Cases；可录制 CLI 演示 | 项目解决了什么真实问题？哪部分代码是你自己写的？ |

**里程碑 M1（C04）**：一个能演示、能解释每个状态转换、失败可复现的 CLI Agent。若还不具备，不准先引入 Redis/复杂服务化。

**C04-P（附加练习，不计入 24 个主 Commit）**：用 Python `asyncio`、类型注解和 `pytest` 写一个极简 `model→tool→model` Loop，包含至少一个并发/超时测试。只为覆盖 Python 岗位的基础能力，不复刻整个平台。

### 阶段 B · Context、知识与协议（C05–C09，P0/P1）

| 编号 | 需求与实现 | 定向学习 | 验收标准 / 证据 | 面试复盘 |
|---|---|---|---|---|
| **C05 Context Builder** | Prompt 组装、Token Budget、保留规则、稳定前缀；优先保住系统约束与工具关联 | Context Engineering、模型上下文窗口、Token 预算 | 构造超窗用例；限制长度且不破坏工具消息配对；策略决策可记录 | Context 截断会导致什么故障？稳定前缀为何有价值？ |
| **C06 Session / Checkpoint** | Session ID、消息持久化接口、重启恢复、会话隔离；定义 Session 与长期 Memory 的边界 | LangGraph Checkpointer vs Store、持久化与状态快照 | 两个 Session 不串话，进程退出后能恢复已完成状态；损坏记录能处理 | Session Memory 与长期 Memory、RAG 有何不同？ |
| **C07 真实业务 Tool** | 将 Mock 工具换成至少一个真实动物行为服务适配器；定义鉴权、超时、可重试性和副作用标记 | REST/OpenAPI、错误码、实验统计接口 | Mock/Real 以配置切换，断连和数据缺失结果明确；不伪造业务数据 | 外部工具失败后怎样告诉 Agent？哪些工具不能自动重试？ |
| **C08 RAG 最小闭环** | 领域文档导入、Chunk、基础关键词/向量检索、带引用答案；有限对照（BM25/混合/Rerank 按必要性） | 检索基础、Recall@K/MRR、RAG 忠实度 | 事先确定至少 20 个可核对的问题；记录 Recall@5 基线；至少一轮可复现对照及错误分析 | Chunk Size 怎样选？检索评测集为何会失真？ |
| **C09 MCP Client** | 接一个 MCP Server，完成发现/调用/失败处理；明确协议版本与所选传输实现 | MCP 官方 Spec、JSON-RPC、stdio / Streamable HTTP | 真实 MCP 工具发现和调用成功；未知工具、断连、协议不匹配有测试 | MCP 与普通 API、HTTP+SSE、stdio 的区别？ |

### 阶段 C · 可靠性、安全与 Context Compaction（C10–C13，P0）

| 编号 | 需求与实现 | 定向学习 | 验收标准 / 证据 | 面试复盘 |
|---|---|---|---|---|
| **C10 Timeout / Cancellation** | 上下文取消贯穿模型与工具调用；运行中断与最终状态定义 | Go `context`、goroutine cancellation | 主动取消后不继续工具操作；资源释放；超时传播端到端测试 | Agent 卡住 20 分钟如何定位？取消为什么会失效？ |
| **C11 Retry / Recovery** | 错误分级、有限重试、指数退避、Jitter、副作用防重复 | Retry / Backoff、幂等原则、错误分级 | 限流/临时 5xx 按策略重试；确定性错误不重试；副作用调用默认不得盲重试 | At-least-once 与 Exactly-once 差异？ |
| **C12 Context Compaction + 最小长期 Memory** | 长对话压缩与事实保留策略；Memory 写入、检索和更新的简化生命周期 | Summarization、Checkpoint/Store、上下文压缩失真 | 固定长对话测试比较压缩前后：约束、实体、未完成任务保持情况；记录失败样例 | 为什么摘要可能丢失关键信息？如何检测压缩质量？ |
| **C13 Permissions + HITL** | 工具允许列表、人工审批暂停/恢复、危险工具默认拒绝、工具输出注入的信任边界；界定 Sandbox 能力 | 最小权限、Prompt Injection 基础、HITL 状态机 | 未授权工具无法执行；审批拒绝后无副作用；暂停恢复与工具恶意返回文本用例通过 | 权限控制在哪里做？模型说“可以调用”就可信吗？ |

**里程碑 M2（C13）**：核心机制可运行、能恢复、能演示失败与防护；具备开始**制作简历和试投递**的基础。必须拿得出 Demo、可复现测试、工程设计说明、能回答取舍问题；不代表可投所有高级岗位。

### 阶段 D · API、异步调度与一致性（C14–C18，P1）

| 编号 | 需求与实现 | 定向学习 | 验收标准 / 证据 | 面试复盘 |
|---|---|---|---|---|
| **C14 HTTP + SSE** | HTTP 接入 Agent、异步 Job API、事件流与断连 | Go net/http、SSE、HTTP 状态与错误处理 | CLI/HTTP 复用同一 Runtime；流式可观察；取消/客户端断连处理明确 | SSE 与 WebSocket 何时选？断线后如何恢复？ |
| **C15 MySQL 持久化** | Job/Run/Session 表、索引、事务与状态变更；基础数据隔离 | MySQL 事务、索引、EXPLAIN、锁 | 初始化迁移脚本；并发状态变更不丢数据；关键查询有 EXPLAIN 证据 | 联合索引、MVCC、隔离级别、乐观锁怎样用？ |
| **C16 并发调度** | Worker Pool、队列背压、并发上限、公平调度的最小实现 | Goroutine、Channel、Semaphore、Backpressure | 可控并发测试；队列满时有确定行为；无 goroutine 泄漏 | Worker Pool 和无限 goroutine 有什么区别？ |
| **C17 Redis Streams** | Consumer Group、ACK、PEL、失败重投、Worker 崩溃恢复 | Redis Streams、XREADGROUP、XPENDING、XAUTOCLAIM | 杀死 Worker 后任务可重新认领；ACK 前后状态可解释；有重复投递测试 | Redis Streams 与 List、Pub/Sub 比较？ |
| **C18 幂等与 Outbox** | 为重要写操作加 idempotency key；对数据库事件引入事务 Outbox 或等效可靠方案 | 事务边界、Outbox、去重与最终一致性 | 人为制造重投不产生重复副作用；DB 提交/消息发送之间崩溃场景有证明 | MQ 为什么通常保证不了业务 Exactly-once？ |

阶段 D **不是 C13 投递的前置条件**。只有当 C13 已经完成，再依序扩展基础设施；如时间吃紧，先完成 C14–C16 和目标岗位高频基础。

### 阶段 E · Trace、评测、缓存、性能与作品集（C19–C23，P1）

| 编号 | 需求与实现 | 定向学习 | 验收标准 / 证据 | 面试复盘 |
|---|---|---|---|---|
| **C19 Trace / Observability** | Trace ID 关联请求、LLM、Tool、Job；阶段耗时与失败原因可定位 | OpenTelemetry GenAI 语义约定、结构化日志 | 能定位 Loop、模型、工具、排队哪一环耗时；无敏感数据泄露 | Agent 5 分钟变 20 分钟，应怎样从 Trace 查起？ |
| **C20 Agent Evaluation** | Golden Cases 扩为固定任务集；分开评工具选择、任务完成、RAG、失败与成本 | Offline Eval、数据泄漏、回归测试、误差分析 | 固定不少于约 30 个有标签案例为目标（量力可调）；至少一次 A/B 改动对照；失败案例分类 | 怎么证明优化不是评测集过拟合？哪些指标不能混用？ |
| **C21 Prompt / Cache-aware** | Stable Prefix、Provider Prompt Cache 的可观测实验；区分响应缓存、工具缓存、Prompt Cache、推理侧 KV Cache | Prefix Cache 原理、模型服务相关文档 | 同一批样本比较延迟/Token 账单/缓存相关指标；无法获取真实缓存数据则如实标注 | Redis 缓存与 KV Cache 是一回事吗？缓存为何可能不命中？ |
| **C22 Benchmark** | 测 p50/p95、TTFT、吞吐、Token 开销，找到真正瓶颈再优化 | Benchmark 设计、Go pprof、负载测试 | 基线与改进在相同环境下复现；标记样本量和噪声，不编造提升百分比 | 性能瓶颈先优化哪里？为什么只看平均延迟不够？ |
| **C23 Docker / CI / 作品集** | Docker Compose 可复现部署、单测/集成测试 CI、架构与设计说明、演示与面试项目故事 | Docker/Compose、GitHub Actions、技术文档 | 按 README 从零启动；测试全绿；演示、架构图、取舍与性能报告完整 | 用 3 分钟和 10 分钟讲项目；被质疑时能指出代码与证据 |

**里程碑 M3（C23）**：形成完整且可复现的工程作品集。目标不是堆技术名词，而是技术决策和真实运行数据经得住追问。

## 4. 面经对应的贯穿训练线（不增加主线 Commit）

### 4.1 算法：约 25–30 分钟 / 学习日

顺序：哈希/数组 → 双指针/滑窗 → 链表/栈队列 → 二叉树/堆 → 二分 → 基础 DP。优先牛客面经出现过的 LRU、链表反转、LIS 等代表性题型。使用投递岗位需要的 Go 或 Python 语言；每题记录解法、复杂度、边界测试。不能只记答案。

### 4.2 计算机基础：约 20 分钟 / 学习日

坚持“与当前 Commit 对齐”：C01–C04 学 Go 接口/HTTP/JSON；C05–C06 学存储和序列化；C09 学 HTTP、SSE、协议；C10–C13 学并发、取消、重试与安全；C15 学索引/事务；C17–C18 学 Redis/MQ/幂等；C19–C22 学性能与系统设计。Python 语法、类型系统、asyncio 在 C04-P 集中补。

### 4.3 开发晚间模板：19:00–22:00（不强制每天都要执行）

- **120 分钟**：当前 Commit 的需求→独立设计→学习→编码→验证（时间灵活分配）。
- **30 分钟**：手撕算法。
- **20 分钟**：与 Commit 相关的基础/面试题。
- **10 分钟**：记录问题、测试证据、三分钟讲解要点。

若毕业任务有风险，优先保护论文时间；不以补课或盲目赶进度导致毕业主线再次延迟。

## 5. 各阶段标准与投递策略

| 节点 | 可证实成果 | 求职动作 |
|---|---|---|
| **C00** | 架构和接口草图、三类业务任务、初始评测案例 | 搜集目标 JD，但不改路线 |
| **C04** | 可运行 CLI Agent + Trace + 固定业务演示 | 开始制作项目表达，练讲项目核心实现 |
| **C13** | 可靠性、安全与 Context 测试；失败可复现 | **开始试投简历和面试**；用真实反馈补短板 |
| **C18** | 可服务化并处理并发、持久化与重投 | 重点覆盖 Agent Infra / 后端研发岗位 |
| **C23** | CI、Docker、Eval、Benchmark、技术文档与 Demo | 完整作品集用于校招或社招初级研发岗位 |

**不能直接写在简历里的内容**：没有真实测量的“99% 成功率”、“高并发生产级”、“Exactly Once”、“GPU KV Cache 优化”、“自研多智能体框架”。按已完成的代码与证据逐步更新。

## 6. 优先学习资源（按用到的 Commit 打开，不需要从头刷完）

| 对应 | 推荐材料 | 使用方式 |
|---|---|---|
| C00–C04 | [pi-mono 源码](https://github.com/badlogic/pi-mono)；[Go Effective Go](https://go.dev/doc/effective_go)；[Go Testing](https://go.dev/doc/tutorial/add-a-test) | 只读与 Agent Loop、消息和工具调用有关的实现；自己重写最小核心 |
| C04-P | [Python asyncio](https://docs.python.org/3/library/asyncio.html)；[typing](https://docs.python.org/3/library/typing.html) | 用 Python 写一个小型 Loop 与超时测试，不要开发第二套系统 |
| C05–C06、C12 | [LangGraph Persistence](https://docs.langchain.com/oss/python/langgraph/persistence) | 理解 Checkpointer 与 Store 的区别，学习原理不迁移框架 |
| C08 | [Elasticsearch BM25](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-similarity.html)；[RAG Evaluation Guide](https://docs.ragas.io/) | 以你的领域数据做小规模真实评测 |
| C09 | [MCP Specification](https://modelcontextprotocol.io/specification) | 锁定一个协议版本；先完成 stdio 或 Streamable HTTP |
| C10–C11、C16 | [Go Context](https://go.dev/blog/context)；[Go Pipelines](https://go.dev/blog/pipelines) | 学取消传播、并发上限和资源释放 |
| C15 | [MySQL 参考手册](https://dev.mysql.com/doc/refman/8.4/en/) | 围绕真实 SQL 看 EXPLAIN 与事务，而非刷完所有章节 |
| C17–C18 | [Redis Streams 文档](https://redis.io/docs/latest/develop/data-types/streams/) | 亲手做 ACK、PEL、消费者崩溃重投实验 |
| C19–C22 | [OpenTelemetry GenAI Semconv](https://opentelemetry.io/docs/specs/semconv/gen-ai/)；[Go pprof](https://go.dev/blog/pprof) | 保留基线、测量条件、优化前后对照 |
| C23 | [Docker Compose](https://docs.docker.com/compose/)；[GitHub Actions](https://docs.github.com/en/actions) | 让别人按 README 复现你的项目 |

可用于观察 JD 与具体面经问题的牛客网资料（此前已讨论；帖子是个案，不代表统计频率）：[字节 Agent Infra](https://www.nowcoder.com/discuss/930155293864914944)、[字节 Agent 调试](https://www.nowcoder.com/discuss/925342611194286080)、[小红书 Agent 服务端](https://www.nowcoder.com/discuss/926917172448759808)、[阿里 Agent 开发](https://www.nowcoder.com/discuss/925162488314765312)、[快手 AI 应用](https://www.nowcoder.com/discuss/925163549431742464)。

## 7. 防止再次“推倒重来”的变更规则

1. **版本冻结**：默认只执行 V2.2 C00–C23，不因一份 JD、热搜框架、博主观点或导师临时建议整体改线。
2. **只允许有证据的修改**：①至少多份目标岗位反馈呈现同一真实短板；②真实面试反馈证实关键缺口；③代码开发遇到无法绕开的技术阻碍；④毕业/求职时间发生实质变化。
3. **先补丁，再重构**：能改测试或验收就不改 Commit 顺序；能增加一段小练习就不新增大模块；必要变更写在 `CHANGELOG.md`，包含证据、影响、成本和决策。
4. **评审节点**：主要只在 C04、C13、C20 或真实面试反馈后回顾，不日更架构。
5. **Stop Doing**：不提前做完整 Multi-Agent 平台、两套语言完整 Runtime、Kubernetes/Kafka/微服务堆叠，也不为简历虚构吞吐或商业场景。
6. **成果优先**：未来讨论某项技术时，默认回答“它应该放在哪个现有 Commit、学到什么程度、怎么验收”，而不是重新出新路线图。

## 8. 下一次直接开始 C00：具体行动

**第一个晚上建议交付：**

1. `README.md`：业务目标、项目边界、不做事项、技术栈。
2. `docs/adr/000-runtime-boundaries.md`：Message / Model / Tool / RunState 接口与职责草图；写出为什么是 Go + Pi-inspired。
3. `eval/golden_cases/`：三个真实需求的输入、期望工具链和判定条件；无真实数据的标记 Mock。
4. 项目骨架与 `go test ./...`；先确保能测试，不要求接入所有服务。
5. `docs/commits/C00.md`：需求、资料、设计选择、验收结果、至少 2 道面试题、3 分钟讲述稿。

**C00 完成才推进 C01；C04 前不再发明新的基础设施需求。**

---

**最终一句话：先证明你能独立设计并运行 Agent，再证明它可靠，再证明它能服务化和可量化优化；与此同时，守住毕业与算法/基础面试能力。**
