# omo-loop-todo

OpenCode skill for delegated autonomous task continuation — the agent creates structured todo lists, executes them, and regenerates the next list without asking, until the user explicitly stops or the task is genuinely exhausted.

## Installation

Copy `SKILL.md` into your OpenCode skills directory:

```
~/.config/opencode/skills/loop-todo/SKILL.md
```

Or symlink it:

```bash
mkdir -p ~/.config/opencode/skills/loop-todo
ln -s /path/to/this/repo/SKILL.md ~/.config/opencode/skills/loop-todo/SKILL.md
```

## What It Does

When you invoke `loop-todo` (or equivalent), the agent enters recurring todo mode:

1. Creates a structured todo list from the current context
2. Executes items one by one
3. Regenerates the next list automatically — without asking
4. Continues until you explicitly stop or nothing adjacent remains

## Key Constraints

- **No progress-reporting pauses** — brief inline updates only, never loop-breaking stops
- **No asking for confirmation to continue** — the invocation itself is the delegation
- **Stop only on explicit signal or genuine exhaustion** — "looks good enough" is not a stop condition
- **Phase closure is a transition** — summarize in one line and immediately continue
- **Nothing adjacent remains** is the required stopping phrase — not "probably done"

## Design Philosophy

This skill enforces a strict delegation model: when you invoke loop-todo, the agent owns autonomous continuation. Every self-interruption, summary pause, or confirmation request is a failure of that delegation. The skill includes explicit prohibition lists, rationalization counters, and pressure-response rules to prevent the agent from escaping the loop through plausible-sounding excuses.

## Version

- **v2.0 — Extreme Pressure Edition**: Added Iron Law, Rationalization Table (13 entries), Red Flags List (10 items), Hard Blocks (10 prohibitions), expanded stop conditions, and pressure response rules.
- v1.0 — Original delegation model.
