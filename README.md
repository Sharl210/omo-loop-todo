# omo-loop-todo

> Stop asking "should I continue?" — the agent already knows.

`loop-todo` is an OpenCode skill that turns one delegation into autonomous, multi-phase task execution. The agent creates a structured todo list, executes it, regenerates the next list, and keeps going — until you explicitly say stop or the work is genuinely exhausted. No check-ins, no progress reports, no confirmation requests.

---

## The Problem It Solves

Without this skill, even a well-intentioned agent will:

- Stop after one phase to "give a quick update"
- Ask "should I continue?" when the answer is obviously yes
- Replace work with a phase summary when the phase is just a transition
- Stop because "this looks good enough" or "I'm not sure if there's more"
- Use manager-speak: *"The current phase objective has been substantially achieved..."*

`loop-todo` makes those behaviors violations. The agent doesn't get to choose when to stop.

---

## How It Works

```
你: loop 一下这个重构
模型: [创建 todo 列表，开始执行第一个 item]
模型: [完成第一个 item，继续下一个]
模型: [到达最终 item] → [从新状态生成下一个 todo 列表，继续]
模型: [phase 边界] → [一句话总结，立即转入下一阶段]
模型: ... 继续直到你说停止或没有什么可做了
```

**You don't say "continue". The skill is the continue signal.**

---

## Quick Start

### Install

Copy `SKILL.md` into your OpenCode skills directory:

```bash
mkdir -p ~/.config/opencode/skills/loop-todo
# Copy SKILL.md from this repo into that directory
```

### Use

```
你: 帮我把这个模块重构一下，loop 一下
```

That's it. The agent will loop until you say `stop` or `停止`.

---

## What Changed in v2.0

The Extreme Pressure Edition adds:

| Addition | Purpose |
|----------|---------|
| **Iron Law** (3 lines) | No ambiguity about what stops the loop |
| **Rationalization Table** (13 excuses) | Explicit counters for every plausible-sounding escape |
| **Red Flags List** (10 items) | Self-detection triggers to catch violations before they happen |
| **Hard Blocks** (10 prohibitions) | Zero-exception rules — no "but in this case..." |
| **Anti-Mechanical Guardrails** | Prevents fake loops that look productive but aren't |
| **Pressure Response Rules** | Keeps looping even under time/exhaustion pressure |
| **"Nothing adjacent remains"** | Required stopping phrase — not "probably done" |

---

## Core Constraints

1. **No stopping to report.** Brief inline updates are fine. Pausing to narrate is not.
2. **No asking to continue.** The invocation is the delegation.
3. **Stop only on explicit signal or genuine exhaustion.** "Looks good enough" is not a stop condition.
4. **Phase closure is a transition.** One-line summary, then immediately continue.
5. **"Nothing adjacent remains"** is the required stopping phrase — not "probably done" or "looks complete."

---

## Philosophy

This skill enforces a strict delegation model. When you invoke `loop-todo`, you are telling the agent: *"I trust you to own the continuation. I will interrupt if I need to."*

Every self-interruption, confirmation request, or summary pause is a failure of that delegation — not because the agent is being difficult, but because the skill wasn't explicit enough to prevent it.

`omo-loop-todo` makes it explicit.

---

## See Also

- [oh-my-opencode](https://github.com/code-yeongyu/oh-my-openagent) — the OpenCode framework this skill runs on

---

## License

MIT
