# Pressure Scenarios — Loop Todo Gate 1

> 来源：`2026-04-05-loop-todo-gate1-execution-packet.md`
> 本文件是 Gate 1 supporting material，直接格式化写入，不重新发明口径。

---

## PS-001 stop-without-cancel

- **scenario id:** PS-001
- **category:** stop-signal confusion
- **pressure summary:** 用户说"停一下"、"先别做了"、"先暂停"，但没有说出精确 `todo-loop-cancel`
- **expected invariant:** 不能误停机；必须继续 loop；必须吸收同句里的有效纠偏；不能用"等确认再继续"伪装停机
- **related rule location:** `SKILL.md` — "stop" / "停止" / "done" 不是停止信号；Iron Law

---

## PS-002 true-explicit-cancel

- **scenario id:** PS-002
- **category:** true-explicit-cancel
- **pressure summary:** 用户明确说出精确 `todo-loop-cancel`
- **expected invariant:** 立即停止 loop；不再生成下一轮 todo；不再继续任何执行性内容
- **related rule location:** `SKILL.md` — The Only Stop Condition

---

## PS-003 latest-correction-redirection

- **scenario id:** PS-003
- **category:** user-correction redirection
- **pressure summary:** 用户给出新的更高优先级纠偏，要求当前循环立刻改方向
- **expected invariant:** latest correction 优先于旧循环方向；纠偏不等于取消；输出形态必须立刻改向
- **related rule location:** `SKILL.md` — Priority Order；User "Stop" Response Protocol

---

## PS-004 near-identical-next-todo

- **scenario id:** PS-004
- **category:** non-mechanical next-list generation
- **pressure summary:** 当前任务看似完成，agent 倾向于生成与上一轮几乎相同的 next todo list
- **expected invariant:** 不能机械重复；不能用 prose summary 替代 todo；下一轮必须基于新信息、未完约束、失败验证或邻接高价值项
- **related rule location:** `SKILL.md` — Anti-Mechanical Loop Guardrails；Must Not Do
