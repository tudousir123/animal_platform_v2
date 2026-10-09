# 项目协作规则

- 当前唯一目标：从零构建 SalesPilot Learning Edition 新能源汽车销售 AI 助手，求职目标 Python AI Agent 应用开发。
- 课程[六个能力里程碑](docs/03-learning-roadmap.md)，**不得依据固定 Commit 数量推动用户学习**。
- **学习先于代码**：每个新任务先给需求、最小原文/官方实验、自己设计题、验收测试与对应小林原题。用户先画设计；未经明确要求不要一次性代写完整核心模块。
- 尽早纵向跑通 Model → ToolCall → ToolResult → Model。普通问答使用 Workflow，深度研究按需 Agent。
- 每次新增功能有 Mock 测试、边界条件、可复现结果、脱敏 Trace；没有运行的指标不填写“已通过”。
- 不复制没有明确许可的 SalesPilot 源码；只对照业务和代码问题。官方 AI Agent Book 固定为子模块。
- 保持依赖克制。先 Python + pytest + 轻量数据与 FastAPI；ES/PG/Redis/React/MCP/Go 等根据实验、JD和需求择机引入。
- 不上传密钥、真实客户数据、未经许可的面经 PDF。
- 毕业论文优先于新增功能；不要频繁修改已经经过验证的主线。
