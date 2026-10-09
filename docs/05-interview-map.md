# 小林 coding 真实面试题映射（按里程碑）

**资料核对**：
- [Python 官方118题目录](https://xiaolincoding.com/interview/python.html)，包含 12 类；题号采用站内原编号。
- [Agent 官方24题](https://xiaolinnote.com/ai/agent/)。
- [RAG 官方专题](https://xiaolinnote.com/ai/rag/)（当前目录列至 #21，旧宣传“20题”的数字不完全同步）。
- [LLM Tools 官方专题](https://xiaolinnote.com/ai/tools/)。
- [牛客](https://www.nowcoder.com/) 公开面经可作为二次验证；**无法提供全站真实118题逐题频率排名**，以下是 Agent 应用开发相关性优先，不是伪造的计数统计。

## Python：优先专题与真实原题编号

| 里程碑 | 优先掌握的官方编号和考点 | 项目中的现场验证 |
|---|---|---|
| M0/M1 | 1.2 对象引用；1.3 `is`/`==`；1.4 可变对象；3.1 Dict 底层；4.2 `*args/**kwargs`；4.3 可变默认参数 | 消息与工具参数不共享可变默认值；字典如何查找 schema |
| M1 | 4.6 闭包；4.8 装饰器；6.4 类方法/静态方法；7.1 异常顺序；7.4 不乱捕获 BaseException/Exception；8.7 类型注解；8.8 dict/TypedDict/dataclass/Pydantic | Tool Registry / Adapter 设计、错误分类、输入输出 |
| M2 | 5.1 可迭代与迭代器；5.4 生成器；8.9 大文件读取；8.10 JSON 序列化；3.5 可哈希对象 | 文档分块、索引迭代器、数据结构 |
| M3 | 9.1 进程/线程/协程；9.2 CPU/I/O 并发选择；10.6 await vs Task；10.10 超时/取消 | 本地检索+外部搜索并发；正确终止 |
| M4 | 5.5 yield/return；7.6 with；9.3 GIL；9.5 GIL与线程安全；10.1 协程；10.4 Event Loop；10.5 Task/Future；10.7 gather/TaskGroup；10.8 同步阻塞；10.9 to_thread；10.10 超时取消；10.11 Semaphore；10.12 contextvars | FastAPI 请求并发、SSE 流、取消与 Trace ID 隔离 |
| M5 | 11.3 循环引用；11.7 缓存和后台任务的内存问题；12.5 CPU/I/O 性能定位；12.6 lru_cache | 性能分析、缓存成本、一致性与故障复盘 |

不用先背完118题再动手；项目中核心题至少能闭卷答3/4分，其余完整题库用来冲刺查漏。

## Agent 24题：原题编号与项目场景

| 里程碑 | 官方题号和确切考点 |
|---|---|
| M0 | #1 Agent和LLM区别、#3 Workflow/Agent/Tools职责、#13 为何有时手写 Runtime |
| M1 | #2 架构组件、#5 ReAct、#20 震荡与死循环、#23 任务幻觉 |
| M2 | #17 Context Engineering、#19 Agent指标评价的基本思路 |
| M3 | #6 ReAct/Plan-and-Execute/Reflection、#7 任务拆分、#14 LLM规划、#15 反思 |
| M4 | #8/#9 长短期记忆、#18 多轮状态和恢复、#21 Trace延迟、#24 数据库越权 |
| M5 | #19 评测集和指标、#20 路径稳定、#21 Trace排查、#23 执行状态验证 |

**明确选修**：#10/#11/#16/#22 多 Agent 设计与子 Agent 协作，不作为当前单 Agent 产品的先修门槛。

原文示例（已核实URL）：
- [#5 ReAct 实现](https://xiaolinnote.com/ai/agent/5_react.html)
- [#6 三种范式取舍](https://xiaolinnote.com/ai/agent/6_three_patterns.html)
- [#13 手写还是框架](https://xiaolinnote.com/ai/agent/13_handcode.html)

## RAG 与工具专项（按官方题号；点目录查看原题）

- **M2 RAG**：#1 整体流程、#4 文档 Chunking、#5 语义切割、#6 Embedding 选择、#11 向量与关键词、#13 多路召回、#14 检索优化、#17 幻觉规避、#18 量化指标、#21 资料冲突。
- **M3/M4 Tools**：#1 Function Calling 原理、#4 MCP 基础、#6 MCP 与 Function Calling、#7 何时用 MCP、#14 SSE/WS、#17 Tool Routing、#18 参数/超时/失败容错。特别是 [#7 FC vs MCP](https://xiaolinnote.com/ai/tools/7_fc_vs_mcp_usage.html)。
- **基础工程**：HTTP/SSE 与数据库题仅在实际增加对应服务/存储时纳入；不提前把全部 Go/MySQL/Redis/分布式 PDF 变成当前开发课程。

## 每题验收模板

```text
问题来源（小林完整原文链接）：
我第一次闭卷回答：
具体代码路径 / 自己写的验证实验：
面试官追问 / 异常变体：
0–4分评级与未解决盲点：
下次复核日期：
```

**版权**：仓库只保存自己写的笔记、主题索引与原题链接，不复制整个商业/第三方题库 PDF。
