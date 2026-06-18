# better_learn — 带插曲追踪的教学 Skill

> 一个 Claude Code 自定义 skill，在 `teach` 的基础上新增 **Excursion（插曲）追踪机制**，防止学习过程中因好奇心跑偏。

## 为什么会有这个 skill？

我在用 Claude Code 的 `teach` skill 学习 AI 工具时，发现 teach 虽然设计精良，但有一个真实的痛点：

### teach 的问题

1. **无防漂移机制**：学习过程中，我很容易因为好奇心被相关内容吸引走（"哎，这个好像也相关？"）。AI 会默默跟着我跑偏，导致教学计划被打乱。
2. **计划修改无闸门**：教学路线图容易被对话中的临时兴趣悄悄改变，而非基于真正的新知识。
3. **插曲发生后无显式确认**：AI 不会告诉我"你正在偏离主线"，我自己也意识不到已经跑偏了。

### better_learn 的解法

| teach 的问题 | better_learn 怎么解决 |
|-------------|---------------------|
| 用户跑偏了，AI 跟着跑 | **Excursion 机制**：识别插曲 → 记录追踪 → 用当前课程知识回答 → 主动拉回主线 |
| 插曲和正式学习混在一起 | `excursions/` 独立目录，事后生成带 `类型: 插曲` 标记的 learning record |
| 无法判断何时该新建课程 | **3 层子插曲阈值** → 提醒用户 → 可选为此主题另开一门课程 |
| 教学计划可被对话修改 | `COURSE_PLAN.md` 作为教学锚点，**只有外部学习才能触发计划变更** |

## 核心机制：Excursion（插曲）

```
用户提问偏离主线
    ↓
识别为插曲 → excursions/L0005-E1-<slug>.md
    ↓
用当前课程知识回答（不向外拓展新概念）
    ↓
回答后显式告知：
  "🔖 这是插曲 (L0005-E1)，你想：A) 继续深入  B) 回到主线？"
    ↓
选 A → 继续追踪深度；选 B → 回到 COURSE_PLAN
    ↓
深度到 3 层 → "⚠️ 偏离 3 层，是否另开课程？"
```

## 文件结构

```
better_learn/                   # Skill 目录
├── SKILL.md                    # 主指令（含完整插曲处理逻辑）
├── COURSE-PLAN-FORMAT.md       # 教学路线图格式（锚点）
├── EXCURSION-FORMAT.md         # 插曲记录格式（LXXXX-E1 命名 + 深度追踪）
├── LEARNING-RECORD-FORMAT.md   # 增强：新增"类型"和"关联插曲"字段
├── MISSION-FORMAT.md           # 学习使命格式
├── RESOURCES-FORMAT.md         # 外部资源格式
└── GLOSSARY-FORMAT.md          # 术语表格式
```

工作区新增：
- `COURSE_PLAN.md` — 结构化教学路线图（从 NOTES.md 拆分出来）
- `excursions/` — 插曲追踪目录

## 关键设计决策

**插曲回答策略**：优先用当前 lesson 的概念框架解释，不引入未学的新概念。这是 deliberate friction——限制 curiosity drift 的蔓延范围。

**3 层阈值不是拒绝好奇心**：到第 3 层不是粗暴打断，而是给好奇心一个正当去处——为它单独开一门课。

**计划变更闸门**：只有用户从外部（自己实践、其他课程、外部资源、社区互动）学到新东西时，才修改 COURSE_PLAN.md。教学过程中的插曲不进路线图。

## 背景

这个 skill 本身就是我学习 skill 设计的第一个实战产出。在使用teach skill 的过程，我发现了上述对我学习产生的问题，设计了 better_learn。它也是对 teach 6 个设计模式（三级渐进加载、有状态工作区、格式即合约、哲学驱动决策、ZPD、具体产出物）的实战应用。

## License

MIT
