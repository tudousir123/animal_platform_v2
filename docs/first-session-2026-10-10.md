# 开工任务书｜2026-10-10｜A0: Go 基础契约

> **这是一张可直接执行的任务卡，不是“读完 Go 以后再开发”。**  
> 目标：亲手写出第一版 Go 数据模型与测试，理解 Agent 消息、工具调用、结果和运行状态之间的关系。  
> 今天**不接 LLM、不接 Redis/MySQL、不接视觉算法、不要求写 Agent Loop**。  
> 建议预算：**约 3 小时**（如果沿用此前晚间 19:00–22:00 学习时段）；不足则优先完成 Go 编译和一个 round-trip 测试，第二天继续 A0。

## 1. 先读需求（10 分钟）

业务输入示例：「请查询 EXP001 水迷宫实验的已保存指标，并注明来源。」

只需能描述数据在这一条链路里的形态：

User Message → (将来) Model ToolCall → ToolResult → RunState → (将来) Final Answer

四个核心概念：

- Message：对话消息，谁发的、内容类型是什么；
- ToolCall：模型请求什么工具、传入什么参数、如何唯一标识这次调用；
- ToolResult：哪一次 ToolCall 的结果、成功还是失败、来源或错误信息；
- RunState：一次运行的 ID、状态、必要消息与终止原因。

**设计提示不是标准答案**：不要为了“可扩展”一口气塞 30 个字段。先画最小必需字段，解释理由即可。

## 2. 定向学习（40–50 分钟，够用即停）

按顺序打开：

1. [Go 官方 Module 教程](https://go.dev/doc/tutorial/create-module)：只看 go mod init、包、导入；
2. [Go 官方单元测试教程](https://go.dev/doc/tutorial/add-a-test)：只看 func TestXxx(t *testing.T)、go test；
3. [Learn Go with Tests](https://quii.gitbook.io/learn-go-with-tests/)：Hello, world、Arrays and slices、Structs, methods & interfaces 中的类型/测试段落；跳过暂时无关的扩展。
4. 你的《小林 Golang 面试题》纸质/PDF：**第 3 页 make/new、第 3–4 页数组与 Slice、第 9 页 struct tag**；
5. [小林 Agent Q1/Q2](https://xiaolinnote.com/ai/agent/agent_info.html)：理解 Agent 与模型区别、核心组件；不要求背完整答案。
6. 如有余力，浏览 [Pi Agent Core types.ts](https://github.com/badlogic/pi-mono/blob/main/packages/agent/src/types.ts)；只观察关系，不直接复制类型。

**自测再动手**：能说出 struct 与 map 的差别、为什么 ToolCall 要有 ID、什么情况下 ToolResult 是失败。

## 3. 在 Ubuntu 上启动项目（10–15 分钟）

~~~bash
git clone https://github.com/tudousir123/animal_platform_v2.git
cd animal_platform_v2
go version
git status
~~~

仓库此时**尚无 go.mod 和 Go 源码**。首次运行：

~~~bash
go mod init github.com/tudousir123/animal_platform_v2
mkdir -p internal/runtime docs/decisions docs/commits
~~~

如果 go.mod 已存在就跳过 go mod init。不要为了赶进度写假测试。

## 4. 独立设计（25–35 分钟）

在 docs/decisions/000-runtime-contract.md 写：

- 四种类型各自职责、字段、约束；
- ToolCall ID 与 ToolResult ID 如何保证关联；
- 对空内容、未知消息角色、失败状态如何处理；
- 为什么选择 struct 而不是所有字段都放 map；
- 一个你考虑过但没有采用的备选方案。

先画字段表或者简短伪代码，再开始写代码。

## 5. 自己编码（60–75 分钟）

目标文件：

~~~text
internal/runtime/types.go
internal/runtime/types_test.go
docs/decisions/000-runtime-contract.md
docs/commits/A0.md
~~~

至少实现两组测试：

- **JSON round-trip**：消息和一次含工具结果的运行状态经过 Marshal/Unmarshal，关键信息不丢失；
- **无效输入/状态**：ToolResult 缺关联 ID、状态非法或必要字段缺失时，由显式的验证逻辑返回可解释错误（注意：encoding/json 本身并不会自动验证业务约束）。

选做：为状态转换写一条允许路径和一条拒绝路径，不要提前实现 Agent Loop。

执行：

~~~bash
go fmt ./...
go vet ./...
go test ./...
~~~

没有通过测试不要宣称完成。测试报错可贴给我做 Review，但先自己尝试定位。

## 6. Golden Case 草案与评测意识（10–15 分钟）

检查 eval/golden_cases/ 中的三个 JSON：它们是**评测需求草案，不是已通过的评测结果**。

每个案例至少确认：

- 用户请求是什么；
- 应该查什么既存数据或资料；
- 哪些结果绝对不能编造；
- 如何从工具记录判断「确实执行」而非「模型口头宣称」。

今天不需要实现完整 Eval Runner；只要把判断标准说清楚。接下来 A3 才实现可运行 Agent 评测。

## 7. 提交与面试复盘（10 分钟）

~~~bash
git add go.mod internal/runtime docs/decisions docs/commits
git commit -m "feat: define minimal Go agent runtime contract and tests"
git push origin main
~~~

这组命令只适用于**你确实创建并检查了对应文件**的情况；不要提交密钥、真实患者数据或未脱敏科研原始数据。

在 docs/commits/A0.md 回答：

1. Go 数组与 Slice 有什么不同？
2. make/new 分别做什么？
3. ToolCall ID 为什么重要？
4. Agent Runtime 与一次普通 LLM HTTP 调用有什么区别？
5. 我为什么这样拆 Message / ToolCall / ToolResult / RunState？最大的未知是什么？

**A0 完成条件**：设计可讲清楚、测试全部通过、存在至少一个失败分支、提交记录可见。达不到就继续 A0，不因日期跳任务。

## 8. 明天结束后发给我

直接发送：**你的设计文档、types.go、types_test.go、go test 输出和最不确定的一个设计问题。**

我会先做真实代码 Review 和针对性追问，确认通过后再发 A1 的具体学习卡片，不一次性塞完所有复杂模块。
