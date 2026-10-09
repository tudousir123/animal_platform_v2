# 定向学习与任务卡执行手册

**固定工作流：需求 5 分钟 → 精确学习（够用即停）→ 自己画接口/状态 → 实现 → 自动化验收 → 面试问答 → 提交。**

你不需要事先知道怎么设计；但学习完必须能解释自己的设计。AI 可解释原理和 Review，不默认代写核心实现。

## A0 — Go 基础契约（2026-10-10 执行）

**学习（只看以下内容）**

1. [Go 官方：Create a Go module](https://go.dev/doc/tutorial/create-module) — module 与 package。
2. [Go 官方：Add a test](https://go.dev/doc/tutorial/add-a-test) — testing.T、运行测试。
3. [Learn Go with Tests](https://quii.gitbook.io/learn-go-with-tests/) — 优先看 **Hello, world / Arrays and slices / Structs, methods & interfaces** 中与结构体、Slice、测试相关的部分；不要求看完整本。
4. 已上传的《小林 Coding Golang 面试题.pdf》：**PDF 第 3 页 make/new；第 3–4 页数组与 Slice；第 9 页 struct tag**（这里是你打印的 PDF 页码，非网站分页）。
5. [小林 Agent 24 题目录](https://xiaolinnote.com/ai/agent/agent_info.html)：**Q1 Agent 与 LLM；Q2 Agent 基本架构**。Q13 手搓 Agent 的工程取舍作为补充，今天来不及可后看。
6. [Pi Agent Core: types.ts](https://github.com/badlogic/pi-mono/blob/main/packages/agent/src/types.ts) — **看类型之间的关系，不照抄全部框架设计**。

**学习后的设计/开发**：Message、ToolCall、ToolResult、RunState 的字段与边界；JSON round-trip、非法状态测试；三项业务 Golden Case 保持 Mock 草案身份。

**验收命令**：go fmt ./...；go vet ./...；go test ./...（以实现后实际执行为准）。

**面试反问**：数组与 Slice 差异？make/new？JSON tag？Agent 与 LLM Wrapper 的边界？

详见 [明天的独立任务书](first-session-2026-10-10.md)。

## A1 — Model Adapter + Tool Registry（A0 验收后）

- Go：[接口方法](https://go.dev/tour/methods/9)、[encoding/json](https://pkg.go.dev/encoding/json)、[net/http](https://pkg.go.dev/net/http)、[context](https://pkg.go.dev/context)。
- 小林 Go PDF：**Interface 章节（约 49–53 页）与 Context 章节（约 45–48 页）**；以章节标题核对后使用。
- 小林 Agent：**Q2 架构、Q13 手搓与框架**；Tool Schema 看 [JSON Schema 入门](https://json-schema.org/learn)。
- 设计：Model 接口如何隔离 Provider；工具注册与 Schema 校验顺序；错误结构；超时由谁负责。
- 验收：替换 Mock 不改上层业务；未知工具不得执行；无效参数、Provider 异常可自动复现。
- 面试：为什么不用任意 map 取代接口？typed nil 是什么？模型产生不存在的工具名怎么处理？

## A2 — Agent Loop（A1 验收后）

- [Pi Agent Core: agent-loop.ts](https://github.com/badlogic/pi-mono/blob/main/packages/agent/src/agent-loop.ts)：重点读 outer/inner loop、tool result 关联、事件产生与退出。
- 小林 Agent **Q5 ReAct、Q6 ReAct/Plan-and-Execute/Reflection、Q20 重复调用/死循环、Q23 任务幻觉**。
- Go 基础：for/switch、errors、defer、slice 追加；需要时复习 Context。
- 设计：状态机、max steps、终止原因、每次调用 ID、工具未实际执行的证据边界。
- 验收：2–3 次 Mock ToolCall 正常退出；循环超限有终止原因；不得无工具结果却报告执行成功。
- 面试：Agent Loop 与 Workflow 区别？为什么工具是否执行必须由 Runtime 记录？

## A3 — Demo + Agent Eval v1（A2 验收后）

- 小林 Agent **Q19 Agent Evaluation、Q21 Trace、Q23 任务幻觉**；[Go testing](https://pkg.go.dev/testing)。
- 从 eval/golden_cases/ 三个草案增补成真实可运行案例，先实现确定性 grader（工具名、参数、执行痕迹、终止状态），再讨论 LLM Judge。
- 设计：案例定义、Mock/真实标志、评分字段、坏案例保存格式。
- 验收：报告标注样本总数、通过数、失败 ID 和原因；至少一轮设计变更前后对照；不可把 Mock 结果当真实科研指标。
- 面试：工具选择正确率与任务完成率差异？如何诊断错误工具、错误参数与任务幻觉？

## P0 / P1 — Python Agent Runtime（A3 前后可穿插）

- [Python asyncio 官方文档](https://docs.python.org/3/library/asyncio.html)、[Python typing](https://docs.python.org/3/library/typing.html)、[pytest](https://docs.pytest.org/)；LangGraph [官方文档](https://docs.langchain.com/oss/python/langgraph/overview)。
- P0：用类型注解 + asyncio + pytest 实现相同 Mock Loop 的超时、取消、异常测试。
- P1：用 LangGraph State、Node、Edge 和 Checkpoint 重做相同业务调用链并解释取舍。
- **不复制** Go 的数据库、Redis、HTTP 全套系统；优先共用 Golden Case。
- 面试：asyncio 与 Go goroutine 的差异？State/Checkpoint 用途？何时选 LangGraph？

## R0 / R1 — RAG 与 Evaluation

- [小林 RAG 21 题目录](https://xiaolinnote.com/ai/rag/rag_info.html)：首轮 **Q1、Q4、Q6、Q10、Q11、Q13、Q17、Q18、Q21**。
- R0：先定领域文档和带 gold evidence 的测试集，再分别实现 BM25 和向量检索基线；指标 Hit@K、MRR；生成引用的准确性另测。
- R1：仅对已发现失败尝试 RRF / Hybrid / Rerank，保留同样评测集和延迟记录。
- 面试：查询和索引链路？RAG 与微调？为什么 Hit@K 不等于 Recall@K？幻觉是哪一步导致的？

## D0 — MySQL 最小就业能力（A 阶段提前穿插 SQL 基础）

- 已选课程：[Code with Mosh SQL 教程（B 站）](https://www.bilibili.com/video/BV1UE41147KC/)：**SELECT/WHERE → JOIN → GROUP BY → INSERT/UPDATE/DELETE → 表设计**。
- 《小林 Coding MySQL 面试题.pdf》：索引/B+ 树、联合索引与失效、EXPLAIN、ACID、MVCC、锁；已核对 **PDF 第 9 页唯一约束、约第 52 页索引失效**。
- 实验：agent_runs / jobs 表，索引前后 EXPLAIN，并发 UPDATE 与 UNIQUE 约束。
- **未核实该 B 站转载版每一 P 的标题/时间戳，暂不冒充精准 P 数**；开学对应卡片时补核验，不让用户自己满网找。

## D1 — Redis 最小就业能力（A 阶段先认识数据类型）

- [Redis 命令与数据类型](https://redis.io/docs/latest/develop/data-types/)；[Streams](https://redis.io/docs/latest/develop/data-types/streams/)。
- 《小林 Coding Redis 面试题.pdf》：**第 2–4 页数据类型/Streams**；后续按主题学习持久化、缓存三大问题、分布式锁、PEL/ACK。
- 实验：TTL 与缓存失效；Consumer Group、ACK、XPENDING、XAUTOCLAIM / 重投；记录重复处理时业务如何保证幂等。
- 面试：为何 Redis 快？缓存击穿/雪崩/穿透？RDB/AOF？Streams 与 List、Pub/Sub 区别？

## 每张任务卡的完成定义

1. 有简短需求和可核对输入/输出；
2. 有独立设计（可更改），标出一个备选方案；
3. 代码 + 自动化测试可以运行；
4. 有 Golden Case/故障实验/指标中至少一种证据；
5. 能 3 分钟解释「解决问题—取舍—证据—不足」；
6. 提交到 GitHub，记录下一张任务卡前必须修的阻塞项。

本页是**章节级精确执行索引**；缺少外部视频可靠 P 数时明确标注待核，不造时间戳与章节顺序。
