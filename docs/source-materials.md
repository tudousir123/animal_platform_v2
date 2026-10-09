# 资料入口与使用许可

| 资料 | 官方入口 | 使用方式 |
|---|---|---|
| 原业务项目 SalesPilot | https://github.com/FelixDemon1/SalesPilot | 查业务需求与现有问题；许可不明确，不拷贝源码 |
| 李博杰 AI Agent Book | https://github.com/bojieli/ai-agent-book | 主教材、官方源码实验；已通过 Git submodule 固定到 `dbc046e` |
| 官方图书 PDF | https://github.com/bojieli/ai-agent-book/releases/download/latest/AI-Agents-in-Depth-zh-CN.pdf | **直接链接，未上传 PDF 文件** |
| AgentGuide | https://github.com/adongwanai/AgentGuide | 工程、安全、评测、交付指南 |
| AgentGuide Harness | https://github.com/adongwanai/AgentGuide/blob/main/docs/02-tech-stack/27-agent-harness-engineering.md | 第一次只学 Model+Loop+Tools |
| AgentGuide 项目交付 | https://github.com/adongwanai/AgentGuide/blob/main/docs/03-practice/05-ship-agent-project.md | Spec、Eval、失败归因、可复现 |
| 小林 Python 118 | https://xiaolincoding.com/interview/python.html | 按里程碑选取原题，不平推118题 |
| 小林 Agent 24 | https://xiaolinnote.com/ai/agent/ | 真实题目目录 |
| 小林 RAG | https://xiaolinnote.com/ai/rag/ | 真实题目目录 |
| 小林工具/MCP | https://xiaolinnote.com/ai/tools/ | 真实题目目录 |
| FastAPI 官方 | https://fastapi.tiangolo.com/tutorial/ | M1 极简 API / M4 服务化 |
| Python asyncio | https://docs.python.org/3/library/asyncio.html | M3/M4 I/O与取消 |

## 仓库里的书籍

```bash
git clone --recurse-submodules https://github.com/tudousir123/animal_platform_v2.git
# 已 clone：
git submodule update --init --recursive
```

书籍实验实际路径位于 `references/ai-agent-book/`。因锁定版本不同，上游最新 README 中的特定模型名可能更新；执行时优先核对本地固定版。

## 用户已有的面经 PDF（定位了文件，未上传）

此前上传的资料：小林Coding Agent面试题官方索引_V2.2、Golang、MySQL、Redis、系统设计、消息队列、分布式 PDF。Agent索引的2页已读，Go PDF首页已读；其他大型文件尚未全部逐页审核。Python118、Agent24和RAG/Tools专题已经从官方线上目录核查。

GitHub API 当前显示仓库仍是 **Public**，第三方面试 PDF 不向该仓库公开分发；版权使用边界独立于仓库是否 Private。
