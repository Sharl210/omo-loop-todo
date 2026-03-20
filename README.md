# omo-loop-todo

> Stop asking "should I continue?" — the agent already knows.

`loop-todo` 是一个 OpenCode 技能，把一次委托变成永久自主执行。Agent 创建结构化待办列表、执行、再生成下一个列表、继续——**永远不停**。除非你使用 `todo-loop-cancel`。

---

## 核心规则

**loop-todo 永不停止。唯一停止方式是 `todo-loop-cancel`。**

- ❌ "stop" — 不是停止信号，继续
- ❌ "停止" — 不是停止信号，继续
- ❌ "done" — 不是停止信号，继续
- ❌ 用户沉默 — 继续
- ✅ `todo-loop-cancel` — 唯一合法停止信号

如果用户说"stop"或"停止"，回复：**"继续中。如果需要取消 loop，请说 todo-loop-cancel。"** 然后继续工作。

---

## 它解决了什么问题

没有这个 skill，Agent 会在你觉得还能继续的时候停下来：

- "汇报一下进度吧" → 其实你只是想让他跑着别停
- "继续吧" → 每次都要说，太累
- "这阶段差不多了" → 其实还有大量相邻工作可以做

`loop-todo` 让"继续"成为唯一默认输出。

---

## 工作流程

```
你: loop 一下这个重构
Agent: [创建 todo 列表，开始执行第一个 item]
Agent: [完成第一个，继续下一个]
Agent: [到达最终 item] → [从新状态生成下一个 todo 列表，继续]
Agent: [当前任务耗尽] → [扩展到相邻领域，继续]
Agent: ... 永远继续，直到你说 todo-loop-cancel

你: 停止
Agent: 继续中。如果需要取消 loop，请说 todo-loop-cancel。

你: todo-loop-cancel
Agent: [清空 todo 列表]
Agent: Loop 已取消。任务列表已清空。
```

---

## 快速上手

### 安装

将两个 `SKILL.md` 复制到 OpenCode 的 skills 目录：

```bash
mkdir -p ~/.config/opencode/skills/loop-todo
mkdir -p ~/.config/opencode/skills/todo-loop-cancel
# 把本仓库的 loop-todo/SKILL.md 和 todo-loop-cancel/SKILL.md 分别复制到上述目录
```

### 使用

```
你: 帮我把这个模块重构一下，loop-todo
```

Agent 会永久 loop。直到你说：

```
你: todo-loop-cancel
```

---

## 两个技能

### loop-todo

永久循环委托。永不停止，不断扩展。

包含：
- **Iron Law**：三行核心禁令
- **Rationalization Table**（12 种借口）：封堵所有停止话术
- **Red Flags List**（10 条）：自我检测红旗
- **Hard Blocks**（10 条禁止项）：零例外规则
- **Scope Expansion Rules**（4 层扩展规则）：任务耗尽时的扩展路径
- **User "Stop" Response Protocol**：对非取消信号的标准化回应

### todo-loop-cancel

唯一停止技能。触发后清列表 + 确认取消 + 不继续。

---

## Scope Expansion Rules

当当前任务耗尽时，按以下顺序扩展：

1. **相邻扩展** — 同模块的相邻文件、函数、concern
2. **质量改进** — 测试、错误处理、文档、性能、一致性
3. **更宽上下文** — 系统性问题、基础设施影响
4. **探索性工作** — 主动探索代码库寻找新价值

总有更多可做。Skill 只在 `todo-loop-cancel` 时结束。

---

## 设计理念

当你调用 `loop-todo`，你是在告诉 Agent：*"除非我说 todo-loop-cancel，否则不要停。"*

"stop"、"停止"、"done" 都是噪音。`todo-loop-cancel` 才是信号。

每一次 Agent 因为这些噪音停下来，都是 skill 不够明确。"看起来差不多了"、"让我汇报一下"、"要继续吗"——这些借口背后都是同一个问题：skill 没有说清楚**只有取消指令才是停止**。

v3 把它说清楚了。

---

## 相关项目

- [oh-my-opencode](https://github.com/code-yeongyu/oh-my-openagent) — 本技能所运行的 OpenCode 框架

---

## License

MIT
