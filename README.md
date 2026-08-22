# better_learn — 管得住好奇心的 AI 教学 Skill

> 一个 Claude Code 自定义 skill，在 [Matt Pocock 的 teach](https://github.com/mattpocock/teach) 基础上，按自己的学习习惯加了一点东西。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 🤔 为什么会有这个 skill？

我用 [Matt Pocock 写的 teach](https://github.com/mattpocock/teach) 学了一阵子。teach 本身已经非常完善——三级渐进加载、文件系统做状态管理、格式文件当合约用、靠学习记录判断难度适配度，设计得很漂亮。

但用着用着我发现了一个不符合我学习习惯的问题：

**我很容易在学到新东西时，因为好奇心顺着一个相关概念问下去。** 比如学 skill 设计的时候听到"agent"，突然就很想知道 agent 和 workflow 有什么区别。AI 也会跟着我跑偏，把本来该讲 skill 的课变成了 agent 讨论。等我回过神来，教学计划已经偏了很远。

teach 的设计里没有机制来应对这个过程——它假设学习者会按部就班地走完每节课。

但我的学习方式不是这样的。对我来说专注学习的时间是有限的，我希望能优先把我一开始想学的学完，听着好像跟我的好奇心冲突，不过好奇虽然不该被禁止，但需要被管理：

1. 不让它暗中绑架学习计划
2. 同时也不粗暴地掐灭它

所以我在 teach 的基础上，按自己的学习习惯加了一个东西：**插曲追踪（Excursion）**。

我并不想大改 teach——mattpocock 已经把它做得很好了。我只是需要一个小机制来贴合自己的学习习惯。

---

## 🎯 加的东西：Excursion（插曲）机制

当你的提问偏离当前课程主线时，AI 会识别出来，记录成一次"插曲"，然后用你当前学到的知识来回答（不是马上引入全新领域的概念）。回答完之后，它会明确告诉你——"这属于插曲，你想继续深入还是回到主线？"，把方向选择权交还给你。

如果好奇心太强，在插曲里又追问了插曲，追到第三层，它会拦住你——"已经偏离 3 层了，要不要为这个方向单独开一门课？"这不是在拒绝你的好奇心，而是在给它一个正当的去处。

```
你提问偏离主线
    ↓
① 识别为插曲 → 创建 excursions/ 文件
    ↓
② 尽量用你已学过的知识回答
    ↓
③ 回答后问你要继续深入还是回到主线
    ↓
选 A → 继续探索（追踪深度）
选 B → 回到课程计划
    ↓
偏离 3 层 → 询问是否另开课程
```

### 插曲文件命名

| 层级 | 命名 | 含义 |
|------|------|------|
| 一级插曲 | `L0005-E1-<slug>.md` | 第 5 课的第 1 个插曲 |
| 二级子插曲 | `L0005-E1-E1-<slug>.md` | 插曲的插曲 |
| 三级子插曲 | `L0005-E1-E1-E1-<slug>.md` | 触发阈值 |

### 两种提问，两种处理方式

better_learn 会区分你的提问是哪种：

**① 外部学习带来的真实疑惑**
你在自己的实验、其他课程、外部资料中遇到一个没弄懂的问题，想搞清楚。这种 AI **不会视为插曲**，会认真为你解答。

**② 学习途中的突发奇想或好奇心**
你正在上课，突然冒出一个相关但不在计划内的念头。AI 会尽量简短地回答，不进一步勾起你的好奇心，然后提醒你："该回到主线了。"如果你忍不住继续追问，达到 3 层它会建议你另开一门课，把这份好奇记录下来，以后专门学。

这样做的好处是：**你既不会分心，又不会忘记曾经想学的东西**——它被记在了 excursions/ 里，等你学完当前课程或有空时再回来看。

---

## 📦 Skill 结构

```
better_learn/
│
├── SKILL.md                       # 主指令（含教学逻辑 + 插曲处理流程）
│
├── COURSE-PLAN-FORMAT.md          # 教学路线图格式（锚点 + 计划变更闸门）
├── EXCURSION-FORMAT.md            # 插曲记录格式（命名规则 + 深度追踪）
├── LEARNING-RECORD-FORMAT.md      # 学习记录格式（含"类型"和"关联插曲"字段）
├── MISSION-FORMAT.md              # 学习使命格式
├── RESOURCES-FORMAT.md            # 外部资源格式
├── GLOSSARY-FORMAT.md             # 术语表格式
│
└── README.md
```

使用 better_learn 教学时，会在你的工作目录中创建：

```
你的项目/
├── MISSION.md                     # 你想学什么、为什么
├── COURSE_PLAN.md                 # 教学路线图（每次 session 的锚点）
├── NOTES.md                       # 草稿笔记
├── RESOURCES.md                   # 学习资源追踪
├── excursions/                    # 插曲追踪目录
├── learning-records/              # 关键决策和 insights 记录
├── lessons/                       # 自包含的 HTML 课程
└── reference/                     # 速查表 / cheat sheets
```

---

## ⚡ 快速安装

### 方法 1：让 AI 帮你装（推荐）

在你的项目目录中告诉 AI：

> "帮我把 better_learn skill 装到 Claude Code 里，仓库在 `https://github.com/ZKH0717/better-learn-skill.git`"

AI 会帮你完成 clone 和配置。

### 方法 2：手动 clone

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/ZKH0717/better-learn-skill.git better_learn
```

### 方法 3：下载 ZIP

打开 [better-learn-skill](https://github.com/ZKH0717/better-learn-skill)，点 `Code` → `Download ZIP`，解压到你的 `~/.claude/skills/better_learn/` 目录。

### 验证

启动 Claude Code 后输入 `/better_learn`，看到教学引导说明安装成功。

---

## 🤖 在 Codex 中使用

这个 skill 用的是开放标准的 `SKILL.md` 格式，Codex 同样能加载。有两个差异需要注意：

| | Claude Code | Codex |
|---|---|---|
| 安装路径 | `~/.claude/skills/` | `~/.codex/skills/` |
| 调用方式 | `/better_learn` | `$better_learn`（`$` 提及） |

Codex 安装：

```bash
mkdir -p ~/.codex/skills
cd ~/.codex/skills
git clone https://github.com/ZKH0717/better-learn-skill.git better_learn
```

装完在 Codex 里输入 `$better_learn` 即可。

> **如果没生效**：Codex 的 skills 功能一度是 experimental、默认关闭的，需在 `~/.codex/config.toml` 里打开开关（具体键名随版本略有差异）。较新版本可能默认开启。

---

## 🚀 使用方法

```bash
cd my-project     # 进入你想学习的工作目录
claude            # 启动 Claude Code
# 然后输入：
/better_learn
```

- **第一次用** → AI 会问你想学什么、为什么想学
- **已有进度** → 自动从上次进度继续
- **指定 topic** → `/better_learn 我想学 Git 工作流`

### 小提示

- 觉得太难/太简单 → 直接告诉 AI
- 想调整学习计划 → 说"我想调整学习计划"，如果理由来自外部学习（看了其他课程、自己实践了），AI 会更新路线图
- 想回顾 → 打开 `lessons/*.html` 或告诉 AI 你想复习

---

## 🧠 一点设计想法

**为什么不在 teach 上直接改，而是另起一个 skill？**

teach 的代码写得很好，我不想把它改成一个"teach 变体"。我更倾向于把它作为一个独立的、专注做一件事的 skill：在 teach 的框架之上加插曲追踪。这样两个 skill 可以共存，各司其职。

**为什么用文件系统做状态管理？**

AI 对话没有稳定的记忆。把学习状态写到文件里，下次对话直接加载——这是我从 teach 学来的好设计。

---

## 🤝 一起让它更好

better_learn 还很年轻。它在我的学习场景中验证有效，但每个学习者都有自己的习惯和痛点。

我知道可以做得更好的方向：

- 更多插曲回答策略
- 学习 analytics（插曲频率、常见跑偏方向）
- 多语言支持
- 安装脚本、初始化向导

有什么想法，直接提 [Issue](https://github.com/ZKH0717/better-learn-skill/issues) 或 [PR](https://github.com/ZKH0717/better-learn-skill/pulls)。

---

## 📄 License

MIT © ZKH0717

---

<p align="center">
  <sub>
    如果你也在研究怎么能让 AI 更好地教人，欢迎来聊聊。
  </sub>
</p>
