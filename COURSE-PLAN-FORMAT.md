# COURSE_PLAN.md Format

`COURSE_PLAN.md` 位于 workspace root。它是教学路线的**锚点**——结构化记录学什么、按什么顺序、当前在哪、计划何时及为何改变。每次 session 开始时首先加载它来定位进度。

与 `NOTES.md` 分离：COURSE_PLAN 是路线图（只读给用户看），NOTES 是草稿和偏好（自由书写）。

## Template

```md
# Course Plan: {Topic}

## Current Status

- **当前 lesson**: L000X — {lesson 主题}
- **上次 session**: {日期} — {简要做了什么}
- **下次 session 首选**: {下一步做什么}

## Roadmap

| Lesson | 主题 | 动手产出 | 对应 mission 成功标准 |
|--------|------|----------|----------------------|
| 0001 | ... | ... | ... |
| 0002 | ... | ... | ... |
| ... | ... | ... | ... |

## Active Excursions

{列出当前 open 状态的插曲，方便追踪未解决的旁支}

- `L000X-E1` — {主题} (open)
- ...

## Plan Change Log

{只记录因外部学习触发的计划变更。格式：日期 + 变更内容 + 外部触发源}

- {日期}: {变更描述} — 触发: {外部学习来源}
```

## Rules

- **Plan as roadmap, not script.** 路线图是可调整的。每次 lesson 完成后更新 `Current Status`。
- **Change gate.** 只有用户从外部学到新东西（自己实践、其他课程、外部资源、社区互动）时，才修改 Roadmap。教学过程中的插曲**不触发**路线图变更。
- **Keep it scannable.** 用户应能在 10 秒内找到"上次学到哪了"。
- **Record every plan change.** Plan Change Log 是外部学习的 audit trail，能看出用户的知识演进轨迹。
- **Excursions are tracked, not ignored.** Active Excursions 区域提醒你和用户：还有未解决的旁支。每次 session 开始时检查是否有残留 open excursion。
- **Lesson numbering is sequential.** 即使路线图有 lesson 被跳过或重排，编号始终递增不重复。
