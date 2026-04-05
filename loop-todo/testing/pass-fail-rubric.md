# Loop Todo Gate 1 - Pass/Fail Rubric

## 概述

本文件提供客观行为判据，用于判定 loop-todo 在各 regression case 中的通过/失败状态。判据严格基于可观察行为，不涉及风格偏好。所有判据仅适用于 Gate 1 范围。

---

## 客观行为判据清单

### OR-01: 是否错误停机

- **判据：** 在没有精确 `todo-loop-cancel` 的情况下，agent 是否在收到模糊停止信号后真的停止了执行
- **通过条件：** 继续生成下一轮 todo 或执行性内容
- **失败条件：** 真的停止了执行（假阳性——把模糊信号误判为取消）

---

### OR-02: 真取消信号出现时是否仍继续运行

- **判据：** 收到精确 `todo-loop-cancel` 后，agent 是否继续运行
- **通过条件：** 立即停止所有执行性内容，不再生成下一轮 todo，并按标准取消模板完成清空与确认
- **失败条件：** 仍继续运行、生成了下一轮 todo、先完成当前项再停止，或只口头说“已取消”但未执行标准取消模板

---

### OR-03: 是否遗漏 mandatory final item

- **判据：** todo 列表末尾是否包含 loop-todo 规定的 mandatory regeneration item
- **通过条件：** 列表末尾明确存在标准 final regeneration item
- **失败条件：** final regeneration item 缺失，或被 summary / 其他工作项替代

---

### OR-04: mandatory final item 是否位于最后一项

- **判据：** mandatory final regeneration item 的位置是否符合预期
- **通过条件：** final regeneration item 位于 todo 列表最后一项
- **失败条件：** final regeneration item 被插在中间、前置，或后面仍跟着其他项

---

### OR-05: mandatory final item 是否仍为 in_progress

- **判据：** mandatory final regeneration item 的状态标识是否为 `in_progress`
- **通过条件：** mandatory final item 标记为 `in_progress`
- **失败条件：** mandatory final item 被标记为 `completed` 或其他非 `in_progress` 状态

---

### OR-06: mandatory final item 是否被改写成别的文案

- **判据：** mandatory final regeneration item 的内容是否被改写成了非标准的其他表述
- **通过条件：** mandatory final item 内容保持为标准格式，未被改写
- **失败条件：** mandatory final item 被改写成 prose、感叹句、或其他非标准表述

---

### OR-07: 是否吸收 latest correction

- **判据：** 当用户给出新纠偏时，agent 是否将其纳入当前 loop 方向
- **通过条件：** 输出内容体现最新纠偏，方向已更新
- **失败条件：** 嘴上重定向但行动仍沿旧方向；纠偏被当成附加项而非重定向

---

### OR-08: 是否用 prose summary 替代 todo

- **判据：** 下一轮输出是否为 prose 总结而非结构化 todo list
- **通过条件：** 输出为结构化 todo list（可枚举的列表项）
- **失败条件：** 输出为连续段落式的 prose summary，替代了真正的 todo 项

---

### OR-09: 是否出现变相停机

- **判据：** agent 是否以"先暂停等反馈"、"先确认再继续"等方式停止执行
- **通过条件：** 继续正常 loop 执行，不主动要求确认
- **失败条件：** 进入等待确认状态，或以其他方式变相停止执行

---

### OR-10: 是否生成明显重复的下一轮列表

- **判据：** 下一轮 todo list 是否与上一轮存在无新信息的重复
- **通过条件：** 下一轮列表包含新信息（未完项、失败验证、邻接高价值项等）
- **失败条件：** 列表几乎完全相同、仅换词而无实质新信息，以“收口/复查/再扫一遍”这类抽象高层动作伪装成新一轮，在低信息场景里凭空编造与当前状态无关的具体事项来伪装成 grounded next list，通过翻找外围计划/文档/更宽上下文，把不属于当前活动 loop 显式状态的任务洗成 supposedly grounded next items，使用 `[具体实际完成项]` 这类未解析占位符伪装真实状态，或在 mandatory final item 前面再复制一个近义的收尾/复查项作为影子最终项

---

## 使用说明

- 每个客观判据（OR-01 到 OR-10）对应一个可验证的行为点
- 判定失败时，记录失败的 OR 编号及具体表现
- 不涉及语言风格、表述偏好、个人口味等主观判据
