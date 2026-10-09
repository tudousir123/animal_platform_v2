# 学习资料与实际源码审查记录

> 审查范围：2026-10-10，源码/官方 README 与用户文件库中可读取的 PDF/官方网页。此审查描述**可观察源码与文档内容**，没有声称已部署原项目、跑通真实依赖或遍历每份 PDF 的全部页数。

## 1. SalesPilot 原仓库：业务核心，但不是直接复刻模板

仓库：https://github.com/FelixDemon1/SalesPilot

已核对文件：
- `README.md`：销售工作台定位、RAG、Web 搜索、Plan/Tools/Memory/Reflection、SSE、React、PG/ES/Chroma。
- `backend/app/service/agent/agent.py`：Plan → actions → memory → Reflection → final answer。
- `backend/app/router/ai_serarch_rt.py`：`/ai_search/`、`/deep_research/`、`/upload_files/`、`/create_session`。
- `backend/app/service/core/retrieval.py`：依赖 ES/Dealer 检索。
- `backend/app/service/web_search/web_search.py` 与 `procss_web_search.py`：Serper 调用与搜索结果筛选。
- `backend/app/service/core/chat.py`、`backend/app/service/session_service.py`：流式回复、历史与数据库。
- `backend/app/requirements.txt`：除 FastAPI 外含 Elasticsearch、Chroma、OCR/OpenCV/ONNX、SQLAlchemy 等依赖；直接搭建全套对新手代价高。

源码中的**具体可复核问题**（不宣称已在真实服务复现）：
1. `final_answer` 中 `memory_global.extend(list(memory_new)[1:])` 会无条件跳过第一条结果；只有一条结果时全部被丢弃。
2. `action_reflect=reflection(...)` 返回补查动作，但分支中再次执行 `process_actions(actions)`，未执行 `action_reflect`；应通过带 Mock 的追踪测试查证行为。
3. 部分路由把 `user_id` 写死为 `"1"`，不能作为用户隔离的有效实现。
4. 若干裸 `except:` / 仅 `print` 后吞错分支，会使错误归因困难；应通过异常类型、Trace 和显式返回状态改进。
5. README 自述 JWT、前端 Session ID、资料删除一致性与自动化测试仍需完善；这些是**作者自述未完事项**。

许可审查：GitHub 仓库元数据 `license=null`，目录树无顶层 LICENSE。**公开可阅读不等于可以复制和重新分发**；本项目只引用链接和设计思路，核心实现从零编写。

## 2. AI Agent Book：先实验观察，再独立实现

仓库：https://github.com/bojieli/ai-agent-book ；Apache-2.0。
当前 Git submodule 固定到 `dbc046eb896ac4e39aa19c7774c8bf49583b89a6`，不是自动追随上游最新 main。

| 学习主题 | 官方原书/实验目录 | 关键实验说明 |
|---|---|---|
| Agent Loop 与工具轨迹 | `book/chapter1.md`；`chapter1/web-search-agent` | **先看无 Key 离线轨迹**，拆解模型、宿主、工具与观察 |
| Context 的作用 | `chapter1/context`；`book/chapter2.md` | 消融 history/tools/tool results，不把模型解释当实际工具结果 |
| Context 压缩 | `chapter2/context-compression` | 比较不同压缩保留关键证据的能力 |
| RAG | `book/chapter3.md`；`chapter3/sparse-embedding`、`chapter3/retrieval-pipeline` | 前者稀疏基线，后者 dense/BM25/fusion/rerank 对照；可先跑 `python evaluate.py --no-dense --no-rerank` |
| Memory | `chapter3/user-memory` | 会话短期状态和跨会话长期记忆有不同写入/纠错要求 |
| Tool | `book/chapter4.md`；`chapter4/execution-tools` | schema、白名单、执行风险与程序级异常边界 |
| Eval | `book/chapter7.md`；`chapter7/tau2-bench-eval` | 结果、动作轨迹、过程规则要分开核查；该实验含外部环境依赖 |
| 改进 | `book/chapter9.md`；`chapter9/trajectory-verifier` | 可从 `python demo.py`、`python -m unittest -v test_verifier.py` 的无 Key 校验开始 |

**不能误导**：实验的部分在线命令需 API Key/下载模型，有的依赖较重。离线可演示并不意味着全部实验无需安装环境；需看对应 README。

## 3. AgentGuide：工程规范而非强制跟全路线

仓库：https://github.com/adongwanai/AgentGuide

重点已阅读：
- `docs/02-tech-stack/27-agent-harness-engineering.md`：明确**初期只要 L1 Model + L2 Loop + L3 Tools**，不必先搭七层；工具注册/状态/权限/日志逐步增加。
- `docs/03-practice/05-ship-agent-project.md`：先理解系统、写 Spec、提供可运行入口与测试；10–20条用例起步，记录 trajectory 和失败类别。
- `docs/05-roadmaps/learning-roadmap-development.md`：是全职8周目标，涉及 LangChain/Milvus/Docker 等，**不是适合正在写毕业论文的个人硬进度和强制技术栈**。
- `projects/README.md`：Paper Agent / Travel Agent / Web Agent 是项目蓝图；不为了完成多个项目而偏离 SalesPilot 单一主项目。

## 4. 小林 coding：真实题库，而非“自己概括的原题”

- [Python 官方118题目录](https://xiaolincoding.com/interview/python.html)：核对到12大类，含对象、容器、装饰器、OOP、异步、GIL、内存管理、CPython。
- [Agent 官方24题目录](https://xiaolinnote.com/ai/agent/)：含架构、ReAct、Plan-and-Execute、Reflection、上下文、评测、工具死循环等。用户上传的《小林Coding_Agent面试题官方索引_V2.2.pdf》**已成功读取两页**，内容为24题链接索引而非题目解析全文；其中老编号 C00–C23 不再用于当前课程。
- [RAG 官方目录](https://xiaolinnote.com/ai/rag/)：在线目录目前列至第21题；第三方汇总页的“20题”口径可能滞后，以当前目录实际条目为准。
- [LLM Tools 官方目录](https://xiaolinnote.com/ai/tools/)：Function Calling、MCP、SSE、工具失败、Tool Routing 等实际题目。
- 用户之前上传的 Go、MySQL、Redis、消息队列、分布式、系统设计 PDF 已定位文件名；**并未完成全部逐页审读**。Go PDF 核对了开头内容与89页页数，原理题重要但此阶段不应挡住 Python Agent 的首个运行链路。
- 此前从牛客筛出 Python 约25个优先主题，属公开面经+岗位匹配的**经验优先级**，不是可验证的“牛客全站逐题出现次数排名”。

## 5. 源码审计得到的课程顺序结论

正确顺序是 **模型/工具主循环尽快可运行 → 内部 RAG → 外部 Search/研究 → 会话/流式/API 工程 → 评测归因与交付**，Eval/Trace 则从第一天滚动累计。

原 14-Commit 路线先做完混合检索再到 Agent Loop，违反“先纵向再横向”的学习原则。分解单位改为里程碑和有独立测试的代码变更，Commit 数量自然形成。

## 6. 未完成的核验 / 不可声称

- 尚未在此环境执行原 SalesPilot 依赖，也未进行浏览器端到端验收。
- 尚未独立测出项目任务完成率、真实检索 Recall、系统延迟、成本及 QPS。
- 个人文件库的全部大型面试 PDF 未逐页全部审阅；不能宣传“已把所有题全部映射到课堂”。
- 项目 GitHub 仓库仍为 Public；面试原文 PDF 不能因个人拥有下载文件就直接公开上传。
