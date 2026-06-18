# Excursion Format

Excursion 文件存放在 `./excursions/` 中。它们是用户在学习过程中因好奇心偏离主线时产生的"旁支记录"。

Excursion 的核心原则：**承认好奇心的价值，但不让它暗中绑架教学计划。**记录它、回答它、然后让用户主动选择方向。

## Naming

```
L{NUMBER}-E{NUMBER}-<slug>.md
```

- `L` = 母 lesson 编号（4 位数字）
- `E` = 插曲编号（从 1 开始递增）
- 子插曲追加 `-E{NUMBER}`：`L0005-E1-E1-<slug>.md`（L2）、`L0005-E1-E1-E1-<slug>.md`（L3）
- slug：简短的 kebab-case 主题描述

扫描 `./excursions/` 找到同一 lesson 下的最高 E 编号并加一。

## Template

```md
# 插曲: <主题>

- **编号**: L0005-E1
- **触发自**: Lesson 0005 / 用户主动提问 / 外部学习
- **触发原因**: "用户听到 X 概念后好奇与 Y 的区别" | "插曲"
- **深度**: L1
- **状态**: open | resolved
- **关联学习记录**: LR-0006（解决后填写）

## 问题
<用户问了什么>

## 回答
<用当前 lesson 的知识体系解释。尽量不向外拓展新概念。>

## 解决确认
- [ ] 用户确认理解
- [ ] 用户选择: 继续深入 / 回到主线
```

## Rules

- **Always answer within current lesson's framework.** 如果正在学 skill 设计，用 skill 设计的语言回答，不引入 agent/workflow 等未学概念。这是在限制 curiosity drift 的蔓延范围。
- **Short answers only.** Excursion 不是 mini-lesson。目标是快速满足好奇心然后回到主线，不是开启一段新的深入学习。
- **Explicit direction choice.** 每次插曲回答后，必须让用户选择：继续深入还是回主线。不许 AI 暗中替用户决定方向。
- **Depth tracking.** 每次子插曲深度 +1。深度 3 时触发阈值提醒，建议另开课程。
- **File creation is lazy.** 只在确认是插曲后创建文件。不要让 `excursions/` 目录充满空文件。
- **Resolve before moving on.** 不要留下 open 状态的插曲。要么解决它，要么创建新课程来容纳它。
- **New course creation.** 当用户同意为深度插曲另开课程时：创建新 workspace（新目录），包含 MISSION.md 和 COURSE_PLAN.md；在原 workspace 的 excursion 文件中记录指向新课程的链接；让用户选择先学哪个。
