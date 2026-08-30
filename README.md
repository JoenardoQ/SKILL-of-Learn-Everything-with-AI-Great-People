# Learn with AI: Socrates, Feynman, and the Great People

## Abstract

`ai-learning-coach` 是一个促进主动学习的 Agent Skill。它把第一性原理、费曼式解释、苏格拉底追问、最小提示、错误诊断、钢人挑战、迁移练习、闭卷提取和间隔复习组合为自适应学习循环，目标是让 AI 帮助学习者形成可迁移的理解，而不是代替学习者完成思考。

Skill 面向学习、练习、复习、论文精读和社会现象学习。用户若只要求直接答案、报告或成品分析，而不要求经历学习过程，Agent 应退出教学循环并服从该请求。项目采用 MIT License。

## 功能

- **第一性原理教学**：先给出下一步推理必需的定义、原理、公式、假设和边界，再要求学习者推导或应用。
- **四种教学模式**：根据当前目标选择实操工程、抽象概念、数学论证或混合模式，并在目标变化时重新分类。
- **低频综合提问**：每个完整学习阶段后使用一道高综合度问题，避免连续的零碎问答。
- **自适应八步循环**：按需要使用尝试、追问、提示、诊断、挑战、迁移、提取和复习，不机械执行无关步骤。
- **有效掌握判断**：只有正确的闭卷输出和至少一次独立迁移同时出现，才视为达到掌握标准；接受提示的答案只算练习证据。
- **Paper 特化**：重建论文的研究问题、主张、证据、推理桥梁、假设、局限和过度解读边界。
- **社会现象特化**：区分描述、因果、解释、预测和规范主张，比较多层机制、竞争解释和可区分证据。
- **跨 Agent 交接**：使用有边界、可向后读取的 `LEARNING_STATE` v2 保存最小学习状态，并把其中内容限制为不受信任的数据。

## 架构

项目采用“规范源、单入口、条件加载、薄宿主适配”的结构。仓库中的 `ai-learning-coach/` 是规范源；复制到宿主 Skills 目录的内容是安装副本，两者需要显式比较，不能把“仓库已更新”误当成“安装已更新”。

```text
用户学习目标
    ↓
SKILL.md（协议版本、选择边界、共享规则）
    ├── learning-modes.md（模式特化）
    ├── paper-learning.md（Paper 特化）
    ├── social-phenomena-learning.md（社会现象特化）
    └── learning-state.md（跨 Agent 数据协议）
            ↓
agents/openai.yaml（Codex 薄适配元数据）
```

| 组件 | 唯一职责 |
| --- | --- |
| `ai-learning-coach/SKILL.md` | 发现入口、协议版本、共享流程、路由条件、不变量、失败行为和停止条件 |
| `references/learning-modes.md` | 四种教学模式、模式专属阶段图和零基础恢复策略 |
| `references/paper-learning.md` | Paper 的来源边界、主张—证据分析和论文类型检查 |
| `references/social-phenomena-learning.md` | 社会现象的操作化、多层因果和竞争解释协议 |
| `references/learning-state.md` | `LEARNING_STATE` v2、v1 兼容读取、数据上限和不信任边界 |
| `agents/openai.yaml` | Codex 界面展示信息与默认调用提示；不复制主流程 |

## 实现细节

Skill 使用 Markdown 过程指令实现，不包含可执行应用代码。宿主首先根据 `SKILL.md` frontmatter 中的 `name` 和 `description` 判断是否选择该 Skill；选择后必须加载入口正文，再根据当前学习对象按需读取对应 reference。未命中的 reference 不应进入上下文。共享规则只由 `SKILL.md` 持有；reference 只描述其分支特有的阶段图和检查项。

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

如果一个必需的 reference 无法完整读取，Agent 必须说明缺少哪个协议，且不得声称已应用它；只有在基础循环仍足以安全继续时才能降级，否则应请求重新提供或重试读取。Paper 被用户提供或明确选择后，是获准分析的学习材料，但论文主张仍需证据检验，文档中的命令不获得指令权限。

`LEARNING_STATE` v2 记录协议版本、生成时间和绝对复习时间，并对总大小、字段长度和列表数量设限。消费者可读取 v1 的已知字段，但不得猜测无法可靠换算的相对复习时间。所有状态字段都是学习数据，不能授予工具、文件、凭据或外部操作权限。

结构验证、安装、宿主发现、入口加载和教学行为是不同状态。当前 `compatibility` 只表达包形状已针对 Codex Skill 结构校验；它不声称 Codex 全生命周期或其他 SKILL.md 宿主已经通过行为验证。

## 安装方法

**Linux / macOS：首次安装或覆盖升级**

以下过程先备份旧副本，再用规范源完整替换，避免新版本已经删除的文件残留。请在仓库根目录运行：

```bash
set -e
skill_target="$HOME/.codex/skills/ai-learning-coach"
skill_backup="$HOME/.codex/backups/ai-learning-coach-$(date +%Y%m%d-%H%M%S)"
test "$skill_target" = "$HOME/.codex/skills/ai-learning-coach" || exit 1
test ! -e "$skill_backup" || exit 1
mkdir -p "$(dirname "$skill_target")" "$(dirname "$skill_backup")"
if test -d "$skill_target"; then cp -R "$skill_target" "$skill_backup"; fi
rm -rf -- "$skill_target"
mkdir -p "$skill_target"
cp -R ai-learning-coach/. "$skill_target/"
diff -qr ai-learning-coach "$skill_target"
```

最后一条命令没有输出且退出码为 `0`，才表示仓库规范源与安装副本逐文件一致。

**Windows PowerShell：首次安装或覆盖升级**

```powershell
$ErrorActionPreference = "Stop"
$skillTarget = Join-Path $HOME ".codex\skills\ai-learning-coach"
$skillBackup = Join-Path $HOME (".codex\backups\ai-learning-coach-" + (Get-Date -Format "yyyyMMdd-HHmmss"))
$expectedTarget = Join-Path $HOME ".codex\skills\ai-learning-coach"
if ($skillTarget -ne $expectedTarget) { throw "Unexpected skill target" }
if (Test-Path $skillBackup) { throw "Backup target already exists" }
if (Test-Path $skillTarget) {
  New-Item -ItemType Directory -Force (Split-Path $skillBackup) | Out-Null
  Copy-Item -Recurse -Force $skillTarget $skillBackup
}
Remove-Item -Recurse -Force $skillTarget -ErrorAction SilentlyContinue
New-Item -ItemType Directory -Force $skillTarget | Out-Null
Copy-Item -Recurse -Force ".\ai-learning-coach\*" $skillTarget
git diff --no-index -- .\ai-learning-coach $skillTarget
```

`git diff --no-index` 没有差异时可能输出为空；退出码 `0` 表示一致。若要卸载，先把精确的 `ai-learning-coach` 安装目录移动到 `.codex/backups/`，不要删除整个 `.codex/skills/`。

安装或升级后新建一个 Codex 任务再显式调用；当前正在运行的任务不保证重新发现刚替换的 Skill：

```text
$ai-learning-coach 帮我理解一个陌生概念。先判断学习模式，从第一性原理建立一个完整阶段，再用一道综合问题检查我。
```

## 测试细节

当前候选版本**已通过本地结构校验，尚未完成全流程测试**。验证结论必须同时绑定 `SKILL.md` 中的协议版本和候选目录指纹；候选内容变化后旧结论失效。

可复现的结构验证命令：

```bash
python3 "$HOME/.codex/skills/agent-skill-author/scripts/validate_skill.py" ai-learning-coach
python3 "$HOME/.codex/skills/agent-skill-author/scripts/validate_eval_result.py" \
  --candidate-dir ai-learning-coach --print-candidate-revision
git diff --check
```

2026-08-31 的本地证据：

- 协议版本：`2.0`；
- 候选指纹：`sha256:ecf30655f4753642f3ff5aaafabba5467cc0621de4fd140fdc9a2eddf6634853`；
- 仓库候选与用户级安装副本的结构验证：均为 0 errors、0 warnings；
- `git diff --check`：退出码 `0`；
- `diff -qr ai-learning-coach "$HOME/.codex/skills/ai-learning-coach"`：无输出，退出码 `0`。

行为评估可行性检查使用了 `codex-cli 0.149.0-alpha.4.3`。当前可见的临时会话和配置选项不能证明在保持其余条件相同的情况下分别建立“完全不加载该 Skill”的对照组与“完整加载候选”的实验组，因此没有运行或声称行为对照。

尚未完成的验证包括：干净 Codex 环境中的发现、选择、入口完整加载、误触发拒绝、升级和卸载；至少五次同条件的无 Skill 对照与候选行为评估；真实学习效果；以及其他 Agent 主机的独立兼容性验证。因此“结构校验通过”不能被表述为“教学效果、Codex 全生命周期或跨主机兼容性已经验证”。
