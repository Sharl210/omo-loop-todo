# testing/transcripts/

本目录只存放**测试证据索引**，不存放完整长对话原文。

## 目录用途

记录每次压力测试的元信息，供后续 agent 快速定位到对应 transcript 位置，而不是把完整对话内容塞进 `SKILL.md` 或其他维护文件。

## 允许存放的内容

- transcript 文件名（引用指向）
- 测试场景 id、对应的 pressure-scenarios.md 条目
- 简要行为摘要（1-3 句）
- 通过 / 失败标签
- 关键失误点的一行说明

## 不允许存放的内容

- 完整对话转录原文（超过 20 行的 transcript 正文）
- agent 生成的完整思考过程
- 与 Skill 规则正文无关的个人备注

## 为什么不把长 transcript 堆进 SKILL.md

`SKILL.md` 是高压可执行文件，必须保持精炼、可搜索。完整长对话包含大量上下文噪音和局部合理化措辞，一旦混入会：

- 拖垮 `SKILL.md` 的加载速度和可维护性
- 让核心不变量被细节淹没，后续 agent 难以快速定位规则
- 每次新增测试案例都要改 `SKILL.md`，形成 git 冲突和维护负担

transcript 的真正价值在于**提炼出的 pattern**（记录在 `baseline-failures.md`、`regression-cases.md`），而不是原文本身。这里最多保留索引信息和短摘要供溯源，不存放完整长对话原文，也不参与 `SKILL.md` 正文的演进。
