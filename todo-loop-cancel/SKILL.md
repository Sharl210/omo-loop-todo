---
name: todo-loop-cancel
description: Use when the user says "todo-loop-cancel", "取消 loop", "取消循环", or explicitly invokes this skill to stop the loop-todo loop. This is the only valid signal to stop the autonomous todo loop.
---

# Todo Loop Cancel

**This is the only skill that can stop `loop-todo`.**

When `todo-loop-cancel` is invoked, the loop stops immediately. No negotiation. No "are you sure?". No "let me finish this one last thing.". The loop ends and does not restart on its own.

## What to Do

When invoked, do these three things in order:

1. **Clear the todo list.** Mark all remaining items as cancelled. The loop is over.
2. **Confirm cancellation.** State clearly: "Loop 已取消。任务列表已清空。"
3. **Do not continue the loop.** Do not regenerate a new todo list. Do not start a new round. Do not say "continuing..." The loop is over.

## What NOT to Do

- Do not continue working after cancellation.
- Do not say "I'll just finish this one more thing."
- Do not regenerate a new todo list after clearing the old one.
- Do not interpret "stop", "停止", "done" as this signal — only the explicit keyword triggers cancellation.
- Do not ask for confirmation before stopping.
- Do not restart the loop after cancellation.

## Hard Rule

```
Once todo-loop-cancel is invoked, the loop stops. 
No continuation. No "one more item". No negotiation.
```

## Cancel Response Template

```
[清空 todo 列表]

Loop 已取消。任务列表已清空。
```

If work was in progress when cancelled, briefly note what was interrupted — but do not continue working:

```
[清空 todo 列表]

Loop 已取消。以下工作未完成：
- [中断的 item 描述]

如需重新开始 loop，请重新调用 loop-todo。
```

## Relationship with loop-todo

- `loop-todo` runs until `todo-loop-cancel` is invoked.
- `loop-todo` treats all other stop signals as continuation signals.
- Only `todo-loop-cancel` is the real stop.

(End of file)
