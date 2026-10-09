# SalesPilot Learning Edition 学习资料索引

## 四个主入口

1. **SalesPilot 业务与代码参考（不复制源码）**：https://github.com/FelixDemon1/SalesPilot
   - 后端入口：`backend/app/app_main.py`
   - 业务 API：`backend/app/router/ai_serarch_rt.py`
   - Agent 逻辑：`backend/app/service/agent/agent.py`
   - RAG：`backend/app/service/core/rag/`；Web：`backend/app/service/web_search/`
   - README 的待改进问题可作为工程验收反例，未独立验证不能声称已经修复
2. **李博杰 AI Agent Book**：https://github.com/bojieli/ai-agent-book
   - 本仓库已有 `references/ai-agent-book` Git submodule，含原书、章节和官方实验源码；需 `git submodule update --init --recursive`
   - 官方 PDF：https://github.com/bojieli/ai-agent-book/releases/download/latest/AI-Agents-in-Depth-zh-CN.pdf （链接，**没有上传 PDF 文件**）
   - 官方学习指导：https://github.com/bojieli/ai-agent-book/blob/main/docs/zh-CN/LEARNING.md
3. **AgentGuide**：https://github.com/adongwanai/AgentGuide
   - 求职项目交付：https://github.com/adongwanai/AgentGuide/blob/main/docs/03-practice/05-ship-agent-project.md
   - Harness 工程：https://github.com/adongwanai/AgentGuide/blob/main/docs/02-tech-stack/27-agent-harness-engineering.md
   - 项目总览：https://github.com/adongwanai/AgentGuide/tree/main/projects
4. **小林 coding**：https://xiaolincoding.com/
   - Python 面试：https://xiaolincoding.com/interview/python.html
   - Agent/RAG/大模型工程面试内容：按各 Commit 现场核对题目来源和原题链接。
   - 之前定位的用户资料包括 Python/Agent 索引，以及 Golang、MySQL、Redis、分布式、消息队列和系统设计 PDF。**尚未上传这些 PDF**。

## 版权、隐私与复现

SalesPilot 当前 GitHub 仓库未提供明确的顶层 License；仅供阅读、对照与独立实现，不将源码直接复制到本学习项目。AI Agent Book 原仓库以 Apache-2.0 提供，在本仓库以 submodule 引用。面试题 PDF 暂不重新公开分发。任何供应商 API Key 均不能放进 Git。

**仓库目前仍是 Public（按 GitHub API 实际查询）**，改为 Private 后才讨论个人 PDF 的仓库内备份，并仍需遵守文件权利人的使用许可。

学习资料以链接为主，真正完成的代码与评测结果另放 `src/`、`tests/`、`eval/`。
