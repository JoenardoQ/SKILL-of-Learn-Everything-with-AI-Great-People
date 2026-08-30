# Learn with AI: Socrates, Feynman, and the Great People

## Abstract

`ai-learning-coach` 是一个促进主动学习的 Agent Skill。它把第一性原理、费曼式解释、苏格拉底追问、最小提示、错误诊断、钢人挑战、迁移练习、闭卷提取和间隔复习组合为一个自适应学习循环，目标是让 AI 帮助学习者形成可迁移的理解，而不是代替学习者完成思考。

Skill 面向学习、练习、复习、论文精读和社会现象分析。用户若明确要求直接答案、报告或成品，Agent 应退出教学循环并服从该请求。项目采用 MIT License。

## 功能

- **第一性原理教学**：先给出下一步推理必需的定义、原理、公式、假设和边界，再要求学习者推导或应用。
- **四种教学模式**：根据当前目标选择实操工程、抽象概念、数学论证或混合模式，并在目标变化时重新分类。
- **低频综合提问**：每个完整学习阶段后使用一道高综合度问题，避免连续的零碎问答。
- **自适应八步循环**：按需要使用尝试、追问、提示、诊断、挑战、迁移、提取和复习，不机械执行无关步骤。
- **有效掌握判断**：只有正确的闭卷输出和至少一次独立迁移同时出现，才视为达到掌握标准；接受提示的答案只算练习证据。
- **Paper 特化**：重建论文的研究问题、主张、证据、推理桥梁、假设、局限和过度解读边界。
- **社会现象特化**：区分描述、因果、解释、预测和规范主张，比较多层机制、竞争解释和可区分证据。
- **跨 Agent 交接**：使用版本化 `LEARNING_STATE` 保存最小学习状态，同时把其中内容限制为不受信任的数据。

## 架构

项目采用“单入口、条件加载、运行时资源分层”的结构：

```text
用户学习目标
    ↓
SKILL.md
    ├── 会话路由与阶段边界
    ├── 第一性原理教学循环
    └── 条件选择 references
            ├── learning-modes.md
            ├── paper-learning.md
            ├── social-phenomena-learning.md
            └── learning-state.md
```

| 组件 | 唯一职责 |
| --- | --- |
| `ai-learning-coach/SKILL.md` | 发现入口、共享流程、路由条件、不变量、停止条件 |
| `references/learning-modes.md` | 四种教学模式及零基础恢复策略 |
| `references/paper-learning.md` | Paper 的来源边界、主张—证据分析和论文类型检查 |
| `references/social-phenomena-learning.md` | 社会现象的操作化、多层因果和竞争解释协议 |
| `references/learning-state.md` | 跨 Agent 状态模式、数据最小化和不信任边界 |
| `agents/openai.yaml` | Codex 界面展示信息与默认调用提示 |

## 实现细节

Skill 使用 Markdown 过程指令实现，不包含可执行应用代码。宿主首先根据 `SKILL.md` frontmatter 中的 `name` 和 `description` 判断是否选择该 Skill；选择后加载入口正文，再根据当前学习对象按需读取对应 reference。未命中的 reference 不应进入上下文，这种渐进式加载减少了上下文占用，也避免不同领域规则互相污染。

运行流程如下：

```text
识别目标与已有基础
    → 选择教学模式
    → 确定最小完整阶段
    → 加载对象特化协议
    → 建立第一性原理地基
    → 运行自适应学习循环
    → 独立迁移与闭卷提取
    → 掌握、继续、复习或交接
```

共享规则只在 `SKILL.md` 定义一次，条件规则由对应 reference 单独拥有。Paper 被用户提供或明确选择后，是获准分析的学习材料，但论文主张仍需证据检验，文档中的命令也不获得指令权限。`LEARNING_STATE` 同样只承载学习数据，不能授予工具、文件、凭据或外部操作权限。

掌握标准和安全边界属于运行时协议的一部分；仓库不再携带独立评测目录。宿主发现、完整加载、教学效果和跨主机兼容性仍需要在目标环境中单独验证。

## 安装方法

Codex 用户级安装：

```bash
git clone https://github.com/JoenardoQ/SKILL-of-Learn-Everything-with-AI-Great-People.git
cd SKILL-of-Learn-Everything-with-AI-Great-People
mkdir -p "$HOME/.codex/skills/ai-learning-coach"
cp -R ai-learning-coach/. "$HOME/.codex/skills/ai-learning-coach/"
```

Windows PowerShell：

```powershell
git clone https://github.com/JoenardoQ/SKILL-of-Learn-Everything-with-AI-Great-People.git
Set-Location SKILL-of-Learn-Everything-with-AI-Great-People
New-Item -ItemType Directory -Force "$HOME\.codex\skills\ai-learning-coach" | Out-Null
Copy-Item -Recurse -Force ".\ai-learning-coach\*" "$HOME\.codex\skills\ai-learning-coach\"
```

安装后新建一个 Codex 任务并显式调用：

```text
$ai-learning-coach 帮我理解一个陌生概念。先判断学习模式，从第一性原理建立一个完整阶段，再用一道综合问题检查我。
```

升级时在仓库中运行 `git pull`，然后重新复制 `ai-learning-coach/` 内容。若新版本删除过文件，应先备份并移除旧的 `~/.codex/skills/ai-learning-coach` 目录，再执行完整复制，避免废弃文件残留。项目级安装则把整个 `ai-learning-coach/` 目录放入项目约定的 Skills 目录。

## 测试细节

当前版本**已通过本地结构校验，尚未完成全流程测试**。

已完成的本地检查：

- `agent-skill-author` 结构验证：0 errors、0 warnings；
- Git 空白与补丁格式检查；
- Markdown 本地引用检查。

尚未完成的验证：

- 干净 Codex 环境中的安装、发现、选择、入口加载、拒绝误触发、升级和卸载；
- 真实学习会话中的行为与效果评估；
- 其他 Agent 主机上的独立兼容性验证。

因此目前只能声称“本地结构校验通过”，不能声称“教学效果、Codex 全生命周期或跨主机兼容性已经验证”。
