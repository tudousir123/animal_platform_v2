# AutoCoach AI 原始资料库

这里将**原著和自己的项目实现分离**：`references/ai-agent-book` 是李博杰原仓库的 Git submodule（固定源码版本），包括原书中文 Markdown、附图、109 个配套实验与许可证；本仓库 `src/`、`tests/` 是自己完成的作业，不能复制粘贴冒充个人实现。

## 官方材料

- [已关联：完整 AI Agent Book 原始仓库](https://github.com/bojieli/ai-agent-book)；源码固定 SHA：`dbc046eb896ac4e39aa19c7774c8bf49583b89a6`；Apache-2.0
- [原著官方 PDF（Release 下载）](https://github.com/bojieli/ai-agent-book/releases/download/latest/AI-Agents-in-Depth-zh-CN.pdf)；PDF 目前采用官方链接，未上传到本仓库
- [原著官方 EPUB](https://github.com/bojieli/ai-agent-book/releases/download/latest/AI-Agents-in-Depth-zh-CN.epub)
- [官方在线阅读](https://bojieli.github.io/ai-agent-book/astro/)
- [官方学习建议](https://github.com/bojieli/ai-agent-book/blob/main/docs/zh-CN/LEARNING.md)
- [作者配套实验总目录](https://github.com/bojieli/ai-agent-book#-运行配套实验)

## 如何下载完整源材料

```bash
git clone --recurse-submodules https://github.com/tudousir123/animal_platform_v2.git
# 已经克隆过本仓库：
git submodule update --init --recursive
```

克隆后按 `docs/autocoach-commit-playbook-v3.md` 的 C00–C14 顺序阅读 `references/ai-agent-book/book/chapter*.md` 与 `references/ai-agent-book/chapter*/` 的实验 README。

## 小林 coding / 用户上传的 PDF

- 原题浏览入口：[小林 coding 官网](https://www.xiaolincoding.com/)
- 《Golang面试题 _ 小林coding.pdf》（用户提供，89页）：**暂未上传**；这是后续 Go 阶段的资料。
- Agent/RAG/LLM 工具调用专题：逐 Commit 建面试题索引和自己的复盘笔记；当前不复制站点受版权保护的全文。
- **原因：仓库 API 返回 `private:false`（公开）**。必须先确认已改成 GitHub Private，再讨论把个人下载的第三方 PDF 放到仓库，且避免把非授权的副本公开分发。

## 注意
- Git submodule 是链接并固定另一个仓库的原始 Git 历史，不是把上游源码逐个文件改写为你自己的 Commit；克隆时需 `--recurse-submodules`。
- 官方 Release PDF/EPUB 采用链接，不要把它们误称为“已经上传”。
- 未开始 C00，资料导入与学习任务完成是两回事。
