# omo-loop-todo

> Stop asking "should I continue?" — the agent already knows.

`loop-todo` 是一个 OpenCode 技能，把一次委托变成真正的自主多阶段任务执行。Agent 创建结构化待办列表、执行、再生成下一个列表、继续执行——直到你明确说停止，或者任务真正耗尽。无需确认、无需汇报、无需问"要继续吗"。

---

## 它解决了什么问题

没有这个 skill，Agent 哪怕出于好意也会：

- 一个阶段完成后停下来"汇报一下进度"
- 问"要继续吗？"——答案明显是 yes
- 用 phase 总结替代实际工作
- 因为"看起来差不多了"或"不确定还有没有更多"而停
- 用项目经理腔：*"当前阶段目标已阶段性达成……"*

`loop-todo` 让这些行为变成违规。Agent 没有权利决定何时停止。

---

## 工作流程

```
你: loop 一下这个重构
Agent: [创建 todo 列表，开始执行第一个 item]
Agent: [完成第一个，继续下一个]
Agent: [到达最终 item] → [从新状态生成下一个 todo 列表，继续]
Agent: [phase 边界] → [一句话总结，立即转入下一阶段]
Agent: ... 直到你说停止或没有什么可做了
```

**你不需要说"继续"。Skill 本身就是继续信号。**

---

## 快速上手

### 安装

将 `SKILL.md` 复制到 OpenCode 的 skills 目录：

```bash
mkdir -p ~/.config/opencode/skills/loop-todo
# 把本仓库的 SKILL.md 复制到上述目录
```

### 使用

```
你: 帮我把这个模块重构一下，loop 一下
```

就这样。Agent 会一直 loop，直到你说 `stop` 或 `停止`。

---

## v2.0 Extreme Pressure Edition 更新了什么

| 新增内容 | 目的 |
|---------|------|
| **Iron Law**（3 行） | 清晰定义什么才能停止 loop |
| **Rationalization Table**（13 种借口） | 对每一种看似合理的逃脱话术给出明确反驳 |
| **Red Flags List**（10 条） | 自我检测触发器，在违规发生前捕捉信号 |
| **Hard Blocks**（10 条禁止项） | 零例外规则——没有"但是这种情况……" |
| **Anti-Mechanical Guardrails** | 防止假循环：看起来在干活但实际没有推进 |
| **Pressure Response Rules** | 在时间压力、上下文过载下依然保持 loop |
| **"Nothing adjacent remains"** | 强制停止语——不是"大概完成了" |

---

## 核心约束

1. **不停下来汇报。** 简短的行内进度说明可以，停下来长篇叙述不行。
2. **不问"要继续吗"。** 调用这个 skill 就是授权。
3. **只有在明确信号或真正耗尽时才停止。** "看起来差不多了"不是停止条件。
4. **Phase 关闭是过渡，不是终点。** 一句话总结，然后立即继续。
5. **"Nothing adjacent remains"** 是强制停止语——不是"大概完成了"。

---

## 设计理念

这个 skill 强制执行严格的委托模型。当你调用 `loop-todo`，你是在告诉 Agent：*"我相信你来做后续判断。如果我需要介入，我会说。"*

每一次自我中断、确认请求或总结停顿，都是对这份委托的失败——不是因为 Agent 故意不干活，而是因为 skill 不够明确，没堵住那个口子。

`omo-loop-todo` 把这个口子堵上了。

---

## 相关项目

- [oh-my-opencode](https://github.com/code-yeongyu/oh-my-openagent) — 本 skill 所运行的 OpenCode 框架

---

## License

MIT
