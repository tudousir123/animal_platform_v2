# SalesPilot Learning Edition · 新能源汽车销售 AI 助手

**当前唯一课程基线：六个能力里程碑（M0–M5），不规定 Commit 数量。** 原 AutoCoach、固定 C00–C13 课程已从当前分支撤下。目标：**Python AI Agent 应用开发求职**；毕业论文优先。

## 从哪里开始

1. [需求和产品规格](docs/01-product-spec.md)：究竟做什么、什么不做、具体成功案例。
2. [四套资料的源码审计与选材结论](docs/02-source-audit.md)：原项目已核实问题与学习边界。
3. [六个里程碑学习路线](docs/03-learning-roadmap.md)：每一阶段的阅读、原版实验、自主设计、实现与数据验收。
4. [执行方法与证据标准](docs/04-execution-playbook.md)：如何拆真实 Commit、如何避免 AI 代写、如何验收。
5. [小林 coding 原题映射](docs/05-interview-map.md)：Python 118 题、Agent 24 题及 RAG/Tools 精选，真实原题链接。
6. [学习进度](docs/progress.md)：目前代码与真实测量均尚未开始。

**从 M0 的业务案例与 M1 的最小 Agent Tool Loop 开始；不要先搭复杂 RAG、Elasticsearch 或 React。**

## 四种资源，四种分工

| 资源 | 作用 |
|---|---|
| [FelixDemon1/SalesPilot](https://github.com/FelixDemon1/SalesPilot) | **业务参照**：新能源车销售辅助、RAG、实时 Web Search、深度研究、FastAPI/SSE；不直接移植原代码 |
| [AI Agent Book](https://github.com/bojieli/ai-agent-book) | **唯一 Agent 理论主教材和官方可运行实验**，已作为 `references/ai-agent-book` 子模块引用 |
| [AgentGuide](https://github.com/adongwanai/AgentGuide) | **工程审查与求职指南**：Harness、权限、Trace、Eval、故障归因、作品集 |
| [小林 coding](https://xiaolincoding.com/) | **闭卷面试训练**：Python、Agent、RAG、Tools、系统设计及需要时的数据库基础 |

## 目标产品

销售人员询问车型参数、竞品、政策等问题；系统在注明来源与资料时间的前提下，调用**企业知识检索和实时 Web Search**，必要时由 Agent 规划多步检索，形成有出处的回答。使用 FastAPI 提供 HTTP，后续加入会话、SSE、可靠性和可复现实验。

**本项目不再做销售员扮演、培训评分或模拟销售对话训练。** 初期使用明确标为「演示数据」的虚构车型；不捏造真实汽车价格或政策。

## Git 与源码约束

- 每个 Commit 代表一个**有独立验收证据的改动**，数量随真实开发自然增长。
- 原 SalesPilot 仓库没有检测到顶层许可证；仅阅读和对照设计，**不复制代码**。
- API Key、个人资料、受限制的面试题 PDF 不提交到公开仓库。
- 当前 GitHub API 显示此仓库仍为 **Public**；私人面经 PDF 尚未上传。
- 书籍子模块：`git submodule update --init --recursive`。

本次仓库更新是**教学文档与资料审查**，不是已经写出或验收了 AI Agent 系统。
