# Learn with AI: Socrates, Feynman, and the Greats

一个让 AI **促进思考，而不是代替思考**的主动学习 Skill。

`ai-learning-coach` 将费曼学习法、苏格拉底问法、主动回忆、最小提示、错误驱动学习、钢人反驳、迁移练习和间隔复习整合成一套八步学习循环。它默认先让学习者尝试，再逐步提供帮助，减少“看懂了 AI 的回答，却没有真正学会”的错觉。

## 核心原则

> 先让大脑工作，再让 AI 介入；让 AI 制造有益的阻力，而不是消除所有阻力。

普通 AI 助手往往直接给出完整答案。本 Skill 更像一位学习教练：一次提出一个关键问题，定位最早的思维断点，只提供足以继续前进的最小提示，并用闭卷输出和迁移题检验掌握程度。

## 八步学习循环

| 步骤 | 方法 | 学习者要做什么 | AI 的职责 |
| --- | --- | --- | --- |
| 1 | 先行尝试 | 解释、预测、推导或解题 | 在讲解前引出已有理解 |
| 2 | 苏格拉底追问 | 检查定义、证据、假设与因果 | 一次只问一个关键问题 |
| 3 | 最小提示 | 在提示后继续独立思考 | 按层级给出最少帮助 |
| 4 | 错误诊断 | 找出并修复思维断点 | 定位最早错误及其机制 |
| 5 | 钢人挑战 | 回应反例和最强反方观点 | 防止确认偏误与过度概括 |
| 6 | 迁移练习 | 在陌生情境中选择并应用知识 | 改变问题表象，不提前透露方法 |
| 7 | 闭卷提取 | 从记忆中解释、求解或教学 | 检验正确性、完整性与独立性 |
| 8 | 间隔复习 | 在未来重新提取知识 | 根据表现安排复习节奏 |

流程会根据任务灵活缩放，不会为了凑齐八步而机械执行无关环节。用户也可以随时要求跳步或直接查看完整解释。

## 安装

### Codex 用户级安装

克隆仓库：

```bash
git clone https://github.com/JoenardoQ/Learn-with-AI-Socrates-and-Feynman-and-Greats.git
```

将 `ai-learning-coach` 文件夹复制到用户级 Skills 目录：

```bash
mkdir -p ~/.codex/skills
cp -R Learn-with-AI-Socrates-and-Feynman-and-Greats/ai-learning-coach ~/.codex/skills/
```

Windows PowerShell：

```powershell
New-Item -ItemType Directory -Force "$HOME\.codex\skills" | Out-Null
Copy-Item -Recurse ".\Learn-with-AI-Socrates-and-Feynman-and-Greats\ai-learning-coach" "$HOME\.codex\skills\"
```

新建一个 Codex 任务后即可调用。用户级安装使同一 Codex 用户环境中的不同项目和 Agent 都能发现该 Skill。

### 项目级使用

如果只想在单个项目中共享，可以把 `ai-learning-coach` 文件夹放进该项目约定的 Skills 目录，并随项目版本控制。不同 Agent 框架的发现目录可能不同，但核心入口都是 `SKILL.md`。

## 快速开始

显式调用最可靠：

```text
$ai-learning-coach 教我理解贝叶斯定理。我有高中数学基础，不要太早给出答案。
```

用于检查已有理解：

```text
$ai-learning-coach 检查我对机会成本的理解。一次只问一个问题。
```

用于复习材料：

```text
$ai-learning-coach 带我复习下面这篇材料。先让我闭卷回忆，再分析遗漏。
```

用于短时学习：

```text
$ai-learning-coach 我只有十分钟，用压缩版八步循环帮我掌握递归。
```

## 你始终拥有控制权

Skill 默认暂缓完整答案，但不会锁死流程。可以直接告诉它：

```text
跳过反方挑战，进入迁移练习。
```

```text
我已经独立尝试过，现在给我完整推导。
```

```text
降低难度，先给一个完整示例，然后让我做一道相似题。
```

对于安全关键问题，Skill 会优先给出必要的安全信息，而不是为了教学流程延迟提醒。

## 跨 Agent 延续学习

准备换任务或 Agent 时，要求生成交接状态：

```text
$ai-learning-coach 请生成 LEARNING_STATE 交接块，我要换一个 Agent 继续。
```

示例：

```yaml
LEARNING_STATE:
  topic: 贝叶斯定理
  target: 能独立解释并解决基础题
  current_step: 迁移练习
  independent_ability: 能正确写出公式
  weak_spots: 容易混淆先验概率与后验概率
  hints_given: 使用条件概率树理解更新过程
  next_exercise: 医学检测的基准率问题
  review_interval: 1 day
```

将它交给另一个 Agent：

```text
$ai-learning-coach 根据以下 LEARNING_STATE 继续，不要重复已经完成的步骤：

[粘贴状态块]
```

新 Agent 会从下一项行动继续，并保留已经发现的薄弱点、已给提示和复习计划。

## 掌握标准

本 Skill 不把“看过解释”视为掌握。默认需要同时满足：

1. 学习者能够闭卷给出正确回答；
2. 学习者能够解决至少一个表象不同的迁移问题。

如果学习提前结束，它只会报告当前证据和下一项练习，不会虚称已经掌握。

## 项目结构

```text
.
├── ai-learning-coach/
│   ├── SKILL.md
│   └── agents/
│       └── openai.yaml
├── LICENSE
└── README.md
```

## 贡献

欢迎提交 Issue 或 Pull Request，尤其是：

- 不同学科的真实使用案例；
- 更有效的迁移题设计；
- 对提示层级和错误分类的改进；
- 不同 Agent 环境的安装说明。

## License

[MIT](LICENSE) © 2026 Qiao, Yanning
