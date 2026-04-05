# Baseline Failures — Loop Todo Gate 1

> 来源：`2026-04-05-loop-todo-gate1-execution-packet.md`
> 本文件是 Gate 1 supporting material，直接格式化写入，不重新发明口径。

---

## 共通高频合理化（3 类）

1. **"用户意图我已经理解了"**：嘴上承认纠偏，行动仍沿旧方向
2. **"先把当前做完再说"**：把当前项优先级放在真取消信号之前
3. **"看起来已经完整了"**：用完整感伪装 near-identical 列表重复

---

## 按场景的最可能失误点

### PS-001: stop-without-cancel

**输入压力：** 用户说"停一下"、"先别做了"、"先暂停"，但没有说出精确 `todo-loop-cancel`

**最可能的失误点：**
- 把 stop/停止/done 误判成取消
- 进入"先暂停等确认"的变相停机

**常见合理化句型：**
- "用户已经明确表达了停止意图，我应该先停下。"
- "我先暂停一下，等他确认后再继续更稳妥。"

**建议补到：**
- `SKILL.md` 的 stop-signal 边界与变相停机禁令
- `rationalization-matrix.md` 的误当取消 / 变相停机条目

### PS-002: true-explicit-cancel

**输入压力：** 用户明确说出精确 `todo-loop-cancel`

**最可能的失误点：**
- 收到 `todo-loop-cancel` 后仍想先完成当前项
- 在取消后擅自重启 loop

**常见合理化句型：**
- "先把手头这一项收完再停更负责任。"
- "虽然取消了，但这个方向还没完，我可以再补一轮。"

**建议补到：**
- `SKILL.md` 的 true cancel path 与立即停机优先级
- `rationalization-matrix.md` 的顺序执念 / 自我续命条目

**2026-04-05 probe 观察：** 实测响应只输出“已取消 loop。”，没有按取消技能模板清空 todo 并确认“Loop 已取消。任务列表已清空。”

### PS-003: latest-correction-redirection

**输入压力：** 用户给出新的更高优先级纠偏，要求当前循环立刻改方向

**最可能的失误点：**
- 把最新纠偏当成附加项，不重置旧列表
- 嘴上重定向、行动不变

**常见合理化句型：**
- "先把已经生成的列表跑完，再吸收新的纠偏。"
- "我已经理解了新方向，当前继续执行也不冲突。"

**建议补到：**
- `SKILL.md` 的 latest correction 优先级
- `rationalization-matrix.md` 的惯性延续 / 口实分离条目

### PS-004: near-identical-next-todo

**输入压力：** 当前任务看似完成，agent 倾向于生成与上一轮几乎相同的 next todo list

**最可能的失误点：**
- 机械重复上一轮 todo
- 换词复用
- 用总结段落替代真正的下一轮工作项

**常见合理化句型：**
- "当前列表已经很完整了，我只需要继续深化。"
- "我换了一种说法，这已经算是新的下一轮。"

**建议补到：**
- `SKILL.md` 的 Anti-Mechanical Loop Guardrails
- `regression-cases.md` 与 `pass-fail-rubric.md` 的近似重复判据

**2026-04-05 probe 观察：** 实测没有停机，但会生成“收口后继续压测式收尾”这种高层抽象续跑，说明换词复用之外还要防 ceremony-heavy 的伪新列表。
