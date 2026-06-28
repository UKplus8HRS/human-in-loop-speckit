# Human-in-loop Speckit 🧭

语言： [English](README.md) | **简体中文**

Human-in-loop Speckit 是一个 Codex skill，用来做「人在回路中」的 Spec Kit 风格规划。

它不是让 AI 根据一句模糊提示直接开始写代码，而是让 Codex 先像采访者和编辑一样工作：先问问题，再把你的自然语言扩写成 Markdown 草稿，等你确认后，才把这些文档作为执行依据。

这个项目受到 GitHub Spec Kit 和 harness engineering 工作流启发，但它不是 Spec Kit 的 fork，也没有修改上游 Spec Kit。现在它支持两种深度：

- **Basic mode**：适合小任务、低风险、快速协作。
- **Full mode**：更接近 Spec Kit 的完整生命周期，包含 constitution、clarification、research、data model、contracts、quickstart、checklist、analysis、verification、handoff、convergence。

## 为什么需要它 ✨

AI coding agent 最怕的不是代码难，而是目标、边界、验收标准都不清楚。

如果你只说「帮我做个功能」，agent 很容易：

- 自己猜需求；
- 忽略边界；
- 写完代码后自己宣布完成；
- 没有可复查的验收证据；
- 多 agent 并行时互相踩文件或做重复工作。

Human-in-loop Speckit 解决的是这个前置问题：先把「要做什么、不要做什么、怎么判断完成」写清楚，再进入实现。

核心规则很简单：

```text
未经确认的 Markdown 只是草稿。
草稿不能作为执行依据。
```

## 工作流深度 ⚙️

只有在任务很小、你明确想快一点时，才用 `basic`。

普通功能开发、多 agent 分工、需求不清楚、需要严格验收时，默认应该用 `full`：

```text
constitution -> goal -> spec -> clarify -> plan -> research
-> data-model/contracts/quickstart -> tasks -> checklist
-> analysis -> verification -> handoff -> implement -> converge
```

如果仓库本身已经是原生 Spec Kit 项目，优先复用上游 `speckit-*` skills；这个 skill 负责在外面加 human approval gate。

## 它会产出什么 📄

这个 skill 会根据你选择的深度生成 basic 或 full 文档集。

Basic mode：

| 文档 | 用途 |
| --- | --- |
| `goal.md` | 任务是什么、为什么做、成功是什么样 |
| `spec.md` | 用户、用户故事、功能要求、非目标、约束、验收标准 |
| `plan.md` | 技术方案、复用点、文件边界、风险、需要确认的决策 |
| `tasks.md` | 有顺序的任务、依赖关系、可并行项、checkpoint |
| `verification.md` | 自动检查、人工检查、需要报告的证据、不能声称完成的条件 |
| `handoff.md` | 给 Codex、Superpowers 或 subagents 的执行交接说明 |

Full mode 额外包含：

| 文档 | 用途 |
| --- | --- |
| `constitution.md` | 原则、治理规则、不可违反的项目约束 |
| `clarifications.md` | 最多 5 个高影响问题，并把答案整合回 spec |
| `research.md` | 未知点、依赖和技术选择的决策、理由、备选方案 |
| `data-model.md` | 实体、字段、关系、生命周期、校验规则 |
| `contracts/` | API、CLI、事件、parser、UI 或集成契约 |
| `quickstart.md` | 端到端验证场景和期望结果 |
| `checklists/requirements.md` | 需求质量检查表 |
| `analysis.md` | 只读一致性和覆盖率分析 |
| `convergence.md` | 实现后的差距评估和剩余任务 |

如果当前仓库已经是 Spec Kit 项目，它会尽量对齐 `specs/<feature>/spec.md`、`plan.md` 和 `tasks.md`。否则默认使用类似 `work/harness/<short-slug>/` 的轻量目录。

## 和 Spec Kit 的区别 🔍

Spec Kit 是完整的规范驱动开发工具包，可以初始化项目、生成 spec/plan/tasks，并安装 agent 集成。

Human-in-loop Speckit 更像给同样的生命周期加一层人类确认机制，重点是「采访、草稿、确认、再执行」。

| 维度 | Spec Kit | Human-in-loop Speckit |
| --- | --- | --- |
| 主要模式 | 结构化命令和生成文档 | 采访、草稿、确认、再写入 |
| 人的角色 | 提供需求和提示 | 共同编辑每个关键文档 |
| 生命周期深度 | constitution、clarify、plan、tasks、analyze、implement、converge | 同类概念，但每一步都有人工确认门 |
| 执行门槛 | plan/tasks 驱动实现 | tasks、verification、analysis、handoff 都确认后才执行 |
| 验证方式 | checklist/analyze 等流程 | checklist、analysis、证据、不能声称完成条件、convergence |
| subagents | 可通过 agent 工具使用 | 只在 approved handoff 之后分发 |

如果你想要原生工具链，用 Spec Kit。  
如果你想要同等结构，但每个关键文档都由人和 Codex 共同确认，就用这个 skill。

## 安装 🚀

克隆仓库，然后把 skill 目录复制到你的 Codex skills 目录：

```powershell
git clone https://github.com/UKplus8HRS/human-in-loop-speckit.git
Copy-Item -Recurse .\human-in-loop-speckit\skills\human-in-loop-speckit $env:USERPROFILE\.codex\skills\
```

然后重启或新开一个 Codex 会话，让 skill 列表刷新。

安装后目录应该类似：

```text
%USERPROFILE%\.codex\skills\human-in-loop-speckit\SKILL.md
%USERPROFILE%\.codex\skills\human-in-loop-speckit\agents\openai.yaml
%USERPROFILE%\.codex\skills\human-in-loop-speckit\references\artifact-contract.md
```

## 可选：安装 GitHub Spec Kit 🛠️

这个 skill 不强依赖 Spec Kit CLI。  
如果你也想安装上游 Spec Kit，可以安装 `uv` 和 `specify-cli`：

```powershell
py -m pip install --user uv
& "$env:APPDATA\Python\Python314\Scripts\uv.exe" tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

确认命令可用：

```powershell
$env:Path = "$env:USERPROFILE\.local\bin;$env:APPDATA\Python\Python314\Scripts;$env:Path"
specify --version
```

初始化一个 Codex 风格的 Spec Kit 项目：

```powershell
specify init --here --force --integration codex --integration-options="--skills" --ignore-agent-tools --script ps
```

不同版本的 Spec Kit 可能会把生成的 skills 放到 `.agents/skills`。

## 使用方式 💬

在 Codex 里这样触发：

```text
$human-in-loop-speckit 帮我把这个粗略想法整理成 goal/spec/plan/tasks/verification。先问我问题，不要直接实现。
```

Full mode 示例：

```text
$human-in-loop-speckit 使用 full mode。带我完成 constitution、spec、clarification、plan、research、tasks、checklist、analysis、verification 和 handoff，再进入实现。
```

Codex 应该进入短循环：

```text
我正在草拟 goal.md。
当前假设：这个任务是为了在实现前改进规划流程。

问题：
1. 什么结果算成功？
2. 哪些事情这次明确不做？
3. Codex 声称完成前需要报告什么证据？
```

你回答后，Codex 会整理草稿，询问你是否确认或修改。只有你确认后，它才写入文档并进入下一步。

## 示例流程 🧪

```text
你：我想给 Codex 做一个开发小应用的 harness。

Codex 使用这个 skill：
1. 通过提问生成 goal.md。
2. 把目标扩写成 spec.md。
3. 读相关代码后再写 plan.md。
4. 把已确认的计划拆成 tasks.md。
5. 写 verification.md，明确必须提供的证据。
6. 只有确认后才写 handoff.md 给 subagents。
7. 开始实现或分发任务。
8. 每一步都报告 checkpoint。
```

checkpoint 格式：

```text
Done: <做了什么>
Verified: <验证了什么>
Remaining: <还剩什么>
Blocked: <如果阻塞，写清楚缺什么输入>
```

## 文档状态模型 ✅

每个生成的文档建议带上 front matter：

```yaml
---
status: draft
owner: human
last_reviewed: null
---
```

状态含义：

- `draft`：Codex 可以编辑，但不能作为执行依据。
- `needs-user-review`：等待人类 review。
- `approved`：可以被后续文档或实现使用。
- `executing`：正在执行。
- `verified`：需要的证据已经收集完成。

## 仓库结构 🗂️

```text
.
|-- README.md
|-- README.zh-CN.md
|-- LICENSE
|-- skills/
|   `-- human-in-loop-speckit/
|       |-- SKILL.md
|       |-- agents/
|       |   `-- openai.yaml
|       `-- references/
|           |-- artifact-contract.md
|           `-- speckit-parity.md
```

## 验证 🔎

如果你本地有 Codex 系统自带的 `skill-creator`，可以验证 skill 结构：

```powershell
py "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" .\skills\human-in-loop-speckit
```

期望输出：

```text
Skill is valid!
```

## 设计原则 🧩

- 有歧义就先问，不要替用户猜关键决策。
- 只围绕已确认文档做最小必要改动。
- 路由、重试、状态码处理、测试命令这类确定性行为交给代码或明确指令。
- 模型负责采访、摘要、分类、提取、review、草稿扩写。
- 验证项要说明为什么重要，而不只是列命令。
- 遇到冲突要表面化，不要把两种模式平均混在一起。
- 人类始终掌握 approval gate。

## 和上游项目的关系 🤝

- GitHub Spec Kit: https://github.com/github/spec-kit
- Claude Code Superpowers: 可作为后续 dispatch 思路，但本仓库不打包它。

本仓库是独立项目，不隶属于 GitHub、OpenAI 或上游 Spec Kit。

## License 📜

MIT. See `LICENSE`.
