# SalesPilot Learning Edition — 新能源汽车 AI 销售助手

> **当前唯一执行路线：SalesPilot Learning Edition V1.0**。历史 AutoCoach 学习路线已撤销。目标：**AI Agent 应用开发**，毕业任务优先。
>
> 当前状态：课程设计已更新，**C00–C13 尚未实现及验收**。

## 开始学习

**[查看新版完整开发课程：需求、教材实验、独立设计、测试、Eval、Python/Agent 面试题](docs/salespilot-learning-roadmap.md)**

**[学习资料索引（AI Agent Book + AgentGuide + 小林 coding + SalesPilot）](docs/source-materials.md)**

## 四个资源分别做什么？

| 项目 | 职责 |
|---|---|
| [SalesPilot 原项目](https://github.com/FelixDemon1/SalesPilot) | **业务场景和目标架构**：新能源汽车销售问答、企业 RAG、Web 搜索、深度研究、FastAPI、SSE |
| [李博杰 AI Agent Book](https://github.com/bojieli/ai-agent-book) | **唯一 Agent 理论主教材 + 官方实验**；本仓库 `references/ai-agent-book` 为固定版本 submodule |
| [AgentGuide](https://github.com/adongwanai/AgentGuide) | 工程验收、安全、Trace、项目交付与求职指南 |
| [小林 coding](https://xiaolincoding.com/) | Python、Agent/RAG、API、数据库与系统设计面试训练 |

**注意**：本项目是根据 SalesPilot 的业务规格进行原创学习实现，不是原作者源码的镜像或改名版本。原 SalesPilot 仓库未检测到明确顶层开源许可证，因此目前不复制其代码。

## 产品目标

用户提出新能源车型参数、竞品、政策和销售沟通问题。系统使用企业文档检索（RAG）、实时网页搜索、可控 Agent 深度研究，给出带来源的答复，并通过 FastAPI + SSE 提供服务。真实市场资料注明时间与来源，初期用明确标注的演示车型数据。

## 固定学习闭环

**需求 → 原书正文与实验 → 自己画设计 → 编码 → pytest/Mock → 真实模型与搜索 → Eval/Trace → 小林题库答辩 → Git 提交**。

不做原 AutoCoach 的客户扮演、培训评分、角色模拟。第一阶段专注完成可靠的 Python Agent 应用，后续按岗位需要延展 Go/MySQL/Redis 等。

## 当前进度

- [x] 完成项目切换与课程设计
- [x] 关联原书与实验资料
- [ ] C00 最小模型请求与工程化测试
- [ ] C01–C13 代码、评测和可演示服务
- [ ] 简历 V1 与独立面试答辩

## 获取原书实验

```bash
git clone --recurse-submodules https://github.com/tudousir123/animal_platform_v2.git
# 已克隆用户：
git submodule update --init --recursive
```

先阅读 [C00 任务](docs/salespilot-learning-roadmap.md)，不要提前安装 PostgreSQL、Elasticsearch 与 OCR 全家桶。
