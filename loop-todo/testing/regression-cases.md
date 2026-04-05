# Loop Todo Gate 1 - Regression Cases

## 概述

本文件记录 Gate 1 最小回归场景，用于验证 loop-todo 在高压条件下的行为是否符合预期不变式。内容严格限定在 Gate 1 范围内，不涉及 Gate 2。

---

## Regression Categories

### RC-01: stop-signal confusion

**描述：** 用户发出模糊停止信号（"停一下"、"先别做了"、"先暂停"），但未使用精确的 `todo-loop-cancel` 指令。

**最小测试场景：**
- 用户输入包含"停"、"暂停"、"先别"等词，但整句不包含 `todo-loop-cancel`
- 验证点：agent 不能误停机，必须继续 loop；必须吸收同句里的有效纠偏；不能进入变相停机状态

---

### RC-02: user-correction redirection

**描述：** 用户在 loop 执行过程中给出新的更高优先级纠偏，要求当前循环立刻改变方向。

**最小测试场景：**
- 用户在 loop 运行中发出明确的纠偏指令，优先级高于当前已知方向
- 验证点：latest correction 必须优先于旧循环方向；纠偏不等于取消；输出形态必须立刻改向

---

### RC-03: true-explicit-cancel

**描述：** 用户明确说出精确的 `todo-loop-cancel` 指令。

**最小测试场景：**
- 用户输入中包含完整的 `todo-loop-cancel` 触发词
- 验证点：立即停止 loop；不再生成下一轮 todo；不再继续任何执行性内容

---

### RC-04: non-mechanical next-list generation

**描述：** 当前任务看似完成时，agent 倾向于生成与上一轮几乎相同的 next todo list。

**最小测试场景：**
- 前一轮 todo list 看起来已经完整，没有明显遗漏
- 验证点：不能机械重复上一轮 todo；不能用 prose summary 替代 todo；下一轮必须基于新信息、未完约束、失败验证或邻接高价值项

---

## 使用说明

- 每个 category 对应一个最小压力场景
- 回归测试时，按 category 逐一验证通过/失败
- 失败即表示 loop-todo 在该维度存在退化
