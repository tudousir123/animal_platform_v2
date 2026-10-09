# 面试最小知识集：按实践证据验收，不靠背题数

> 参考目标：初级 Agent / AI 应用及部分 Runtime 岗位；题目来自小林 Coding 专题与既有面经定性整理。**这不是统计意义上的「最常考 Top N」排名。**
> 暂无独立 LeetCode 训练线；招聘明确要求时单独处理。

| 主题 | 首轮必须能回答的核心问题 | 验收证据/对应任务 |
|---|---|---|
| **Go** | Slice/Map、Interface 与 nil、错误、Context 取消、goroutine/channel/Mutex、GMP 基础 | A0–A2 + 后续并发小实验 |
| **Agent** | Agent vs Workflow；Loop/Harness；Tool Schema 与 ID；ReAct；循环与失败；Context/Memory/Session；工具执行证据；Eval/Trace | A0–A3、X0、X1 |
| **Agent Eval** | Tool selection vs argument correctness vs task success；golden cases；Badcase；回归测试与 mock/真实区分 | A0 定规则；A3 Eval v1 |
| **Python Agent** | async/await、Task、取消、typing、pytest；LangGraph State/Checkpoint；手搓与框架取舍 | P0、P1 |
| **RAG** | Chunk、Embedding、BM25/Vector、Hybrid/RRF/Rerank、Hit@K/MRR、证据引用、检索错误 vs 生成幻觉 | R0/R1 |
| **MySQL** | SELECT/JOIN/GROUP BY；B+ 树、联合/覆盖索引、EXPLAIN；ACID/MVCC/隔离级别/锁；唯一约束 | D0 |
| **Redis** | 五大类型；过期/淘汰；缓存三大问题；Cache Aside；RDB/AOF；分布式锁风险；Streams/ACK/PEL | D1 |
| **MCP/HTTP** | MCP vs Function Calling；协议、工具权限边界；HTTP 和超时/错误 | X1、I0 |

**分级验收：** L1 用自己的话解释；L2 面对业务问题做技术选择；L3 展示代码/测试/数据。简历写进的核心技能尽量达到 L3；DB 基础先达到 L2 + 小实验。

**小林资料入口**（网页内容会更新，实际学习以届时原文为准）：

- [Agent 24 题目录](https://xiaolinnote.com/ai/agent/agent_info.html)：优先 Q1、Q2、Q5、Q13、Q17–Q21、Q23、Q24。
- [RAG 21 题目录](https://xiaolinnote.com/ai/rag/rag_info.html)：优先 Q1、Q4、Q6、Q10、Q11、Q13、Q17、Q18、Q21。
- 你上传/打印的 89 页 Go、111 页 MySQL、94 页 Redis 及 MQ/分布式等 PDF：优先按学习卡的题目与页码定位，而不是从第 1 页刷到最后一页。

**停止学习条件**：能 L2 作答且亲手跑通实验的主题，先停止继续阅读同类材料；工程深度优先投入 Agent Loop、Eval、RAG。一次模拟面试暴露漏洞后再对症补足。
