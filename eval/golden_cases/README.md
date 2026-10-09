# Golden Cases（草案，尚未执行）

三个 JSON 是初始**业务规格**，不是已建立的评测集、不是跑通的案例，也不包含真实实验结果。所有预期调用仅供 A0 设计讨论；当实际业务工具名称和接口落地后再修订。

- metric-query：按实验/视频 ID 查询已有行为指标，不能编造值；
- group-compare：对已有 AD / sham / 药物组结果进行合规比较，没有统计证据不能声称显著；
- protocol-qa：依实验手册解释范式，证据不全要说明。

后续 A3 将它们扩展为有 mock fixture、明确 expected tool calls、可运行确定性 grader、trace 和 badcase 的首版 Agent Eval。R0 会另设带相关证据标注的 RAG 检索评测集。

评测记录应明确 **case id / fixture 版本 / mock 或 real / 实际工具调用 / 是否执行成功 / 判定规则 / 失败原因**。不可凭最终自然语言中出现了正确数字就宣称任务完成。
