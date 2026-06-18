# Learning Record Format

Learning records 存放在 `./learning-records/` 中，并使用 sequential numbering：`0001-slug.md`、`0002-slug.md` 等。懒创建目录，只在写入第一条 record 时创建。

它们是 teaching 领域里的 ADR：记录 non-obvious lessons、key insights，以及会影响未来 sessions 的 prior knowledge。它们用于计算 zone of proximal development。

## Template

```md
# {Short title of what was learned or established}

{1-3 sentences: what was learned (or what prior knowledge was established), and why it matters for future sessions.}
```

这就是完整格式。Learning record 可以只有一个 paragraph。它的价值在于记录"这个现在已经被知道"，以及"为什么这会改变下一步教什么"，而不是填满 sections。

## Optional fields

只有当这些 fields 真的增加价值时才包含。大多数 records 不需要它们。

- **类型**：`lesson` | `插曲` | `prior-knowledge` | `correction`
  标记这条 record 的来源。`lesson` 是主线课程习得，`插曲` 是从 excusion 中获得的知识补充，`prior-knowledge` 是用户披露的已有知识，`correction` 是 misconception 被纠正。
- **Status**：`active | superseded by LR-NNNN` — 当早期理解后来被证明错误并被替换时很有用。
- **关联插曲**：当 `类型: 插曲` 时，填写对应 excursion 文件编号（如 `L0005-E1`），建立双向链接。
- **Evidence** — 用户如何展示了这种理解（回答了一个问题、完成了一个 exercise、引用了 prior experience）。当这个 claim 之后可能被重新审视时很有用。
- **Implications** — 这为未来 sessions 解锁或排除了什么。当影响不明显时值得记录。

## 插曲类型学习记录

当一条 learning record 的 `类型: 插曲` 时，额外要求：

- **关联插曲** 字段必填，指向 `excursions/` 中的对应文件
- 若用户说明了触发原因，记录在正文中；否则写 `触发原因: 插曲`
- 插曲记录的价值在于：标记"这条知识是旁支获取的"，便于未来判断知识结构的完整性

## Numbering

扫描 `./learning-records/`，找到当前最高编号并加一。

## When to write a learning record

满足以下任一条件时写一条：

1. **用户展示了对某个 non-trivial 内容的真实理解** — 不只是接触过，而是有证据表明他们能正确使用这个 concept。这会为下一步教什么设定新的 floor。
2. **用户披露了 prior knowledge** — "I already know X." 记录下来，避免 future sessions 重复教学。也要记录他们声称的 _depth_。标记 `类型: prior-knowledge`。
3. **一个 misconception 被纠正** — 用户之前相信某个错误说法，现在理解了为什么。这类记录价值很高，因为它们能预测相关 topics 中未来可能卡住的地方。标记 `类型: correction`。
4. **Mission 因学习而转移** — 用户发现自己关心的东西和原先以为的不同。Cross-link 到 [[MISSION.md]] 并更新它。
5. **一个插曲被解决** — 插曲回答完毕且用户确认理解。标记 `类型: 插曲`，填写 `关联插曲`。

### What does _not_ qualify

- 只是覆盖过的 material。覆盖不等于学习。等待 evidence。
- 任何已经在 [[GLOSSARY.md]] 中作为 term definition 简洁捕获的内容。不要重复。
- 逐 session 的 activity logs。Learning records 不是 journal，而是 decision-grade insights。

## Supersession

当后续 record 与早期 record 矛盾时（用户理解加深或被纠正），把旧 record 标记为 `Status: superseded by LR-NNNN`，不要删除。理解如何演化的历史本身也是有用信号。
