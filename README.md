# Agent 工程化学习路线｜Lab Agent → SalesPilot

> **唯一有效学习路线（2026-10-11 更新）**：先完整学习 **Lab Agent 1.0.0** 的配套视频与源码，再将掌握的技术迁移到 **SalesPilot**，形成 Python Agent/AI 应用工程求职作品。旧的“立即从零开发 SalesPilot / 自研 Runtime”执行路线已停止作为当前主任务。

## 当前在哪里

**阶段 A（现在）：Lab Agent 1.0.0。** 观看配套视频、跟随项目运行与阅读源码；围绕 Python、FastAPI、Agent/工具调用、LangGraph、RAG、MySQL、SSE 等**项目实际涉及**的模块开展学习，不提前堆栈。

**阶段 B（以后）：SalesPilot。** 完成 Lab Agent 的学习、代码理解和笔记后，再独立设计与开发新能源汽车销售场景 Agent。现在不并行开发 SalesPilot 或额外的 Agent Runtime。原有 SalesPilot 需求与 M0–M5 是**第二阶段的历史候选规格/能力验收参考**，不是当前的日程或已通过的门禁；业务范围以后以迁移时的需求审查为准。

**求职目标**：能够解释代码、理解架构和取舍、定位故障、修改功能、展示可复现项目，并完成 Python / Agent / RAG 等面试。

## 只保留三份核心学习资料

| 资料 | 定位 | 用法 |
|---|---|---|
| **Lab Agent 1.0.0（用户已选的课程包及视频）** | 唯一当前工程实践主线 | 看视频 → 读源码 → 跟随复现 → 解释与改动 |
| [AI Agent Book](https://github.com/bojieli/ai-agent-book) | Agent 原理 | 只学习与当前 Lab Agent 模块相关的章节/机制；本仓库已有 `references/ai-agent-book` 子模块 |
| [小林 Coding](https://xiaolincoding.com/) | 面试与个人掌握验收 | 当前模块结束后匹配 Python、Agent、RAG、工具调用题；先独立回答 |

Pi、AgentGuide、《软件设计的哲学》/CS190 仅在遇到具体设计疑问时按需参考；ZCode/Matt Skills 是开发辅助工具，不属于第四份主教材。**Python 优先，Go 后期按需补。**

## 每个模块的六步闭环

1. **看视频**：理解作者解决的具体问题、输入输出与执行效果。
2. **读源码**：梳理文件职责、核心调用链、状态/数据流与关键失败路径。
3. **补原理**：ChatGPT 使用 All-in-One 方式结合 AI Agent Book，直接讲解必要理论、关键代码、图解和练习。
4. **实践输出**：跟随复现并解释，逐步进行小改动、变式测试或定位故障；ZCode 可以协助重复代码，但关键判断必须由本人完成。
5. **小林面经**：结合真实对应题独立解释原理、实现、边界与取舍；原题和项目改编题分开标注。
6. **Notion 笔记**：自由记录理解、思考、疑问和验证证据；不设僵硬日程，不以观看时长或代码提交次数冒充掌握。

**阶段 A 退出条件**：已看完选定课程并整理笔记；能讲清主要架构与关键调用链；能独立解释和调整至少一个有代表性的模块、复现基本运行/测试证据，答出关联面试题。具体范围结合真实 Lab Agent 课程核对，不预先声称已完成。

## 文档入口与维护约定

- [唯一学习与 AI 协作协议](docs/07-design-guided-learning.md)
- [当前阶段与证据进度](docs/progress.md)
- [阶段 A/B 的路线及 M0–M5 归属](docs/03-learning-roadmap.md)
- [小林 Coding 现有面经索引](docs/05-interview-map.md)
- [第二阶段 SalesPilot 候选业务规格](docs/01-product-spec.md)
- [此前参考源码审计（历史材料）](docs/02-source-audit.md)

**单源维护**：只在这些现有文件里更新，不新建重复路线 V2/V3。GitHub 存代码、技术协议与验证证据；Notion 存学习笔记、进度与个人反思。用户提供的 Lab Agent 课程 ZIP 不代表已提交到 GitHub；未经核查版权/许可不将课程源码或视频上传公开仓库。仓库当前为 Public；不要提交密钥、私人 PDF 或第三方未授权源码。

**状态说明**：本次仅同步学习计划，并未宣称 Lab Agent 运行、SalesPilot 开发或任何自动化测试已完成。
