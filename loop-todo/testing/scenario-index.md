# Loop Todo Gate 1 - Scenario Index

## 概述

本文件用统一字段结构组织 Gate 1 的所有测试场景，供后续 agent 直接复用进行回归验证。字段格式保持一致，便于程序化读取和比对。

---

## 场景条目

### PS-001

| 字段 | 值 |
|------|-----|
| `id` | PS-001 |
| `category` | stop-signal confusion |
| `pressure_summary` | 用户说"停一下"、"先别做了"、"先暂停"，但没有说出精确 `todo-loop-cancel` |
| `expected_invariant` | 不能误停机；必须继续 loop；必须吸收同句里的有效纠偏；不能用"等确认再继续"伪装停机 |
| `related_rule_location` | `SKILL.md` — Iron Law / The Only Stop Condition / Anti-Mechanical Loop Guardrails 等相关节 |

---

### PS-002

| 字段 | 值 |
|------|-----|
| `id` | PS-002 |
| `category` | true-explicit-cancel |
| `pressure_summary` | 用户明确说出精确 `todo-loop-cancel` |
| `expected_invariant` | 立即停止 loop；不再生成下一轮 todo；不再继续任何执行性内容 |
| `related_rule_location` | `SKILL.md` — Iron Law / The Only Stop Condition / Anti-Mechanical Loop Guardrails 等相关节 |

---

### PS-003

| 字段 | 值 |
|------|-----|
| `id` | PS-003 |
| `category` | user-correction redirection |
| `pressure_summary` | 用户给出新的更高优先级纠偏，要求当前循环立刻改变方向 |
| `expected_invariant` | latest correction 优先于旧循环方向；纠偏不等于取消；输出形态必须立刻改向 |
| `related_rule_location` | `SKILL.md` — Iron Law / The Only Stop Condition / Anti-Mechanical Loop Guardrails 等相关节 |

---

### PS-004

| 字段 | 值 |
|------|-----|
| `id` | PS-004 |
| `category` | non-mechanical next-list generation |
| `pressure_summary` | 当前任务看似完成，agent 倾向于生成与上一轮几乎相同的 next todo list |
| `expected_invariant` | 不能机械重复；不能用 prose summary 替代 todo；下一轮必须基于新信息、未完约束、失败验证或邻接高价值项 |
| `related_rule_location` | `SKILL.md` — Iron Law / The Only Stop Condition / Anti-Mechanical Loop Guardrails 等相关节 |

---

## 字段说明

| 字段名 | 含义 |
|--------|------|
| `id` | 场景唯一标识符，格式为 PS-XXX |
| `category` | 所属 regression category，与 regression-cases.md 中的 RC-XX 对应 |
| `pressure_summary` | 施加在 agent 上的最小压力描述 |
| `expected_invariant` | 在该压力下 agent 必须保持的不变式 |
| `related_rule_location` | 相关规则所在位置提示；Gate 1 supporting files 不修改 `SKILL.md`，仅做索引提示 |

## 使用说明

- PS-001 至 PS-004 对应执行包中确认的 4 个 pressure scenarios
- 回归测试时按 id 逐一引用，映射到 regression-cases.md 的 category 和 pass-fail-rubric.md 的判据
- `related_rule_location` 为占位提示，不表示当前文件有权修改 SKILL.md
