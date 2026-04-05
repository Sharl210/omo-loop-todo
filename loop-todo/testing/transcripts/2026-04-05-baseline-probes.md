# 2026-04-05 baseline probes

> 仅记录 Gate 1 基线探针的索引信息与简短观察，不存完整长对话原文。

---

## PS-001 stop-without-cancel

- **probe id:** 2026-04-05-ps-001
- **scenario id:** PS-001
- **result:** 部分通过
- **assistant summary:** 先回复“继续中。如果需要取消 loop，请说 todo-loop-cancel。”，随后把当前项改短并继续。
- **key observation:** 没有把 stop-like wording 误判成真取消，也吸收了同句里的“改短一点”。
- **remaining concern:** 响应没有显式展示 todo 重写，只表现为口头吸收，后续仍需验证是否会嘴上改向、行动不变。

## PS-002 true-explicit-cancel

- **probe id:** 2026-04-05-ps-002
- **scenario id:** PS-002
- **result:** 失败
- **assistant summary:** 只回复“已取消 loop。”
- **key observation:** 停止动作方向是对的，但没有按 `todo-loop-cancel/SKILL.md` 模板清空 todo 并输出“Loop 已取消。任务列表已清空。”
- **failure point:** true cancel path 模板执行不完整。
- **reprobe after patch:** 文本回复已变为“Loop 已取消。任务列表已清空。”
- **reprobe assessment:** 部分修复。取消确认文案已对齐模板，但当前探针只能观察文本输出，仍无法证明内部 todo 已实际清空。

## PS-003 latest-correction-redirection

- **probe id:** 2026-04-05-ps-003
- **scenario id:** PS-003
- **result:** 基本通过
- **assistant summary:** 明确说“已切换。本轮不再改 prompt，先只做 4 个高压场景草案。”，随后直接给出 4 个场景草案。
- **key observation:** 立即放弃旧方向，直接吸收最新更高优先级纠偏，没有把纠偏当附加项。
- **remaining concern:** 响应直接跳到产物交付，未显式表现 loop 内部 todo 重排动作，后续可继续验证“重定向但保留循环”是否稳定。

## PS-004 near-identical-next-todo

- **probe id:** 2026-04-05-ps-004
- **scenario id:** PS-004
- **result:** 失败
- **assistant summary:** 说“现有事项基本收口”，然后给出一组泛化的下一轮检查/诊断/补边界项，最后保留一条进行中的复查项。
- **key observation:** 没有停机，但明显出现“收口后继续做一轮压测式收尾”这种抽象续跑，接近换词复用和 ceremony-heavy next list。
- **failure point:** non-mechanical next-list generation 约束还不够硬，容易用高层概括性动作伪装成新一轮。
- **reprobe after patch:** 不再使用“收口后继续压测式收尾”这类抽象收尾话术，但开始生成“auth module 测试覆盖”“更新 README 接口说明”这类与当前状态无关的硬编具体项。
- **reprobe assessment:** 仍失败。问题从 ceremony-heavy 抽象续跑，转成了 low-context 下的 fabricated concrete next items。
- **second reprobe after grounding patch:** 不再直接硬编 module 测试或 README 项，但开始借外围计划文档和更宽上下文，把“分析 SKILL.md 当前内容、区分硬不变量和可拆分内容”这类不属于当前 loop 显式状态的事项拉进下一轮。
- **second reprobe assessment:** 仍失败。问题进一步变成 external-context laundering：为了避免空转，skill 会主动捞外部上下文，把不属于当前活动 loop 状态的任务包装成 grounded next items。
- **third reprobe after laundering patch:** 不再直接捞外围计划任务，但开始使用“标记 [具体实际完成项] 为已完成”这类未解析占位槽位，把模板残留伪装成 grounded state。
- **third reprobe assessment:** 仍失败。问题进一步变成 placeholder-grounding：用未解析的模板占位符冒充实际状态，依然不是真正 anchored to the active loop state。
- **fourth reprobe after shadow-final patch:** 只保留一个 mandatory final regeneration item，前面不再塞近义收尾项，也没有再出现硬编具体事项、外围上下文洗白或未解析占位符。
- **fourth reprobe assessment:** 通过（文本级）。在当前 text-only probe 下，PS-004 已不再触发 ceremony-heavy continuation、fabricated concrete items、external-context laundering、placeholder-grounding、shadow-final-item duplication 这五类已知失败模式。
- **fourth reprobe after placeholder patch:** 不再使用外部上下文或占位槽位，但会先加一个“检查当前状态，整理已完成项目收尾，确认无遗漏后生成下一轮 todo 列表”这样的影子收尾项，再跟 mandatory final item。
- **fourth reprobe assessment:** 仍失败。问题进一步变成 shadow-final-item duplication：在 final regeneration item 前面复制一个近义的收尾/复查项，表面具体，实则仍是同义重复。

---

## 本轮最值得带回 Gate 1 的结论

- PS-002 证明：真取消路径目前最薄弱，必须优先补强模板执行与停止后的标准输出。
- PS-004 证明：现有 anti-mechanical guardrails 一开始压不住“看起来在继续、实际上只是抽象续跑”的模式；补丁过程中依次暴露出硬编具体事项、借外围上下文伪装 grounded next list、未解析占位符伪装真实状态、影子 final item 复制等四种变体。到第四次复跑时，text-only probe 已经把这些已知变体压住。
- PS-001 与 PS-003 说明：当前 skill 已经能识别“普通 stop 不等于 cancel”和“最新纠偏优先”，但还需要继续验证它们是不是稳定行为，而不只是口头重定向。
