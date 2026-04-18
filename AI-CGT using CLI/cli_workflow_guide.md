---
document_type: cli_workflow_guide
version: 1.0
companion_to: classical_grounded_theory_SKILL.md / REFERENCE.md / README.md
purpose: |
  本文件说明如何在 Claude Code（或类似命令行 AI 工具）中为经典扎根
  理论研究搭建与使用一套符合 CGT 纪律的工作环境。
---

# 命令行版本使用指南

本指南对应 Claude Code 工具。其他命令行 AI 工具（如 Codex、Aider 等）的配置机制不同，但核心设计原则可迁移——本文末尾有简要说明。

---

## 1. 为什么选择命令行版本

命令行版本相对网页版有三个方法论上的价值：

**第一，强制执行。** 网页版依赖研究者每次对话手动粘贴开场指令，容易被偷懒。命令行版的 slash commands 把这些指令变成 `/type-a` 到 `/type-d` 的一键调用，省去复制粘贴的同时也让"选择对话类型"成为无法跳过的动作。

**第二，文件边界可强制。** 网页版只能靠研究者的自觉不让 LLM 看错文件，命令行版可以通过 `.claudeignore` 从技术上排除原始资料。这把"不让 LLM 主动读资料"从口头约定变成系统约束。

**第三，审计线索更完整。** 每次 session 的日志、每个文件的版本、研究过程的每一步都在本地文件系统中留痕，配合 Git 版本控制可以形成完整的研究过程档案。

然而命令行版本也有额外的风险——详见本指南 §7。

---

## 2. 首次搭建（约 30 分钟）

**前置要求**：已安装 Claude Code；有一份已整理好的 CGT 研究项目目录（按 README §3 的结构）。

**步骤 1 — 放置核心约束文件**。在项目根目录放置以下文件：

- `CLAUDE.md`（本项目根目录，自动加载）
- `classical_grounded_theory_SKILL.md`
- `classical_grounded_theory_REFERENCE.md`
- `.claudeignore`

**步骤 2 — 创建 `.claude/` 配置目录**。目录结构：

```
.claude/
├── settings.json
├── rules/
│   ├── methodology_guard.md
│   └── file_safety.md
└── commands/
    ├── type-a.md
    ├── type-b.md
    ├── type-c.md
    ├── type-d.md
    └── close-session.md
```

**步骤 3 — 验证 Auto Memory 已禁用**。在 `.claude/settings.json` 中确认 `"autoMemoryEnabled": false`。此外在 shell 启动文件（`~/.bashrc` 或 `~/.zshrc`）中添加：

```bash
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=1
```

两重防护。Auto Memory 对 CGT 是系统性风险，必须严防。

**步骤 4 — 创建 research_state.md**。按网页版方案中的模板创建，放在项目根目录。此文件每周更新一次。

**步骤 5 — 验证搭建**。在项目根目录运行 `claude` 启动 Claude Code。在第一轮对话中测试：

```
这是我的 CGT 研究项目。请确认你已加载 CLAUDE.md 与
.claude/rules/ 下的所有文件，并列出它们的要点。
```

Claude 应列出 CLAUDE.md 的硬约束、两份 rules 文件的要点，并确认已识别 SKILL 与 REFERENCE 两份方法论文件。若未能识别，检查文件路径与 `.claude/` 目录结构。

测试 slash commands：输入 `/type-a` 后跟一个方法论问题，Claude 应按类型 A 的约束回应。若 Claude 没有切换到类型 A 的行为模式，检查 `.claude/commands/` 下的文件命名。

---

## 3. 每次 session 的标准流程

一次典型的 session 应遵循以下流程：

**启动 session**。在项目根目录运行 `claude`。Claude Code 自动加载 CLAUDE.md、`.claude/rules/` 下所有文件、以及 `@` 引用的文件（若 CLAUDE.md 中使用了 `@research_state.md` 等引用）。

**选择对话类型**。输入 `/type-a`、`/type-b`、`/type-c`、`/type-d` 之一。**不要跳过这一步直接提问**——跳过会让 Claude 在没有明确角色约束下工作，增加越界风险。

**执行对话**。按所选类型的协议进行。Claude 会在边界被接近时主动说明。若 Claude 漏过了某个边界（例如开始主动推荐新概念），立即打断并引用规则（"这接近 methodology_guard.md §'不做为你补全'"）。

**收尾**。输入 `/close-session`。Claude 生成对话日志，展示给研究者确认，然后保存到 `dialogue_logs/`。

**关闭 session**。退出 Claude Code。**不要在同一 session 中进行多个对话类型**——完成一个任务后退出，下次开新 session 换类型。

---

## 4. 推荐的周节奏（与网页版一致）

周一：资料收集日——不使用 Claude Code。
周二至周三：独立浸润与开放编码——原则上不使用。
周四：理论性采样决定日——不使用。
周五上午：整理备忘录、更新 `research_state.md`——不使用。
周五下午：Claude Code 时段——1-2 次 session，每次 30-60 分钟。
每 2-3 周的周五下午：一次 `/type-d` session，约 90 分钟。
周六周日：休息，完全不使用。

命令行工具的便利性使"一周几次 session"的节奏比网页版更容易被破坏。请留意这一点：**命令行工具的启动成本低，但 CGT 要求的独自浸润时间不变**。

---

## 5. 与 Python 脚本的集成

Claude Code 可以直接运行 Python 脚本。在 CGT 项目中，这带来一个重要的工作方式升级——README §5 列出的八类可自动化任务，现在可以由 Claude Code 协助研究者**即时设计、即时执行、即时检验**。

典型用法：

```
研究者：我需要一个脚本，扫描 memos/by-category/ 下所有备忘录，
        按 YAML front matter 中的 related_categories 字段建立
        反向索引，输出到 output/category_index.md。

Claude： [进入 Plan Mode]
        我将按以下步骤执行：
        1. 读取 memos/by-category/ 下所有 .md 文件
        2. 提取每份文件的 front matter
        3. 建立"范畴 → 备忘录"与"备忘录 → 涉及范畴"两张索引
        4. 生成 Markdown 格式的索引文件到 output/category_index.md
        
        我将创建脚本 scripts/build_category_index.py 执行此任务。
        不改动 memos/ 下任何文件。请确认。

研究者：确认。

Claude： [创建脚本、运行、展示结果]
```

注意这里的几个要点：

**Plan Mode 是默认**。任何涉及文件读写的任务，Claude 先展示计划。研究者确认后才执行。

**脚本创建在 `scripts/` 下**。不是一次性执行即丢，而是成为项目的可复用工具。下次同样需求直接运行脚本即可。

**不修改 memos/ 下的任何文件**。按 file_safety.md 的规则，memos/ 只读不写。

**研究者可以随时检查脚本代码**。Claude 写的脚本必须是研究者可读懂的——若出现复杂逻辑，要求 Claude 解释或简化。

---

## 6. 对 Git 版本控制的集成

推荐在 CGT 项目中使用 Git 做版本控制。典型 `.gitignore`：

```
# 敏感资料
data/raw/
.env

# Claude Code 本地状态
~/.claude/

# Python 产物
__pycache__/
*.pyc
.venv/

# 系统文件
.DS_Store
*.swp
```

**注意 `data/raw/` 不进入 Git**。原始资料含个人身份信息，即使加密也不应进入可能被推送的仓库。原始资料的备份应通过独立的加密本地备份（如加密外部硬盘）。

**每周五做一次提交**。提交信息可由 Claude Code 协助撰写，但**不要让 Claude 自动提交**——提交是研究者对一周工作的"封存"动作。

**提交信息的格式建议**：

```
[YYYY-MM-DD] Week N: [本周主题]

主要进展：
- [进展 1]
- [进展 2]

新浮现的范畴：[若有]
方法论事件：[若有，如进入选择性编码阶段]
```

这种格式日后在答辩前回顾研究演化时极为有用。

---

## 7. 命令行版本的额外风险

命令行版本的便利性带来三类额外风险，研究者应保持警觉。

**风险一：对话成本过低导致滥用**。网页版每次对话要打开浏览器、等待加载，这个摩擦让研究者在每次对话前做一次"真的需要吗"的判断。命令行工具几秒即可启动，这道摩擦消失，滥用几率上升。**对策**：给自己设定硬性上限（如每日最多 2 次 session），并记录实际使用次数。

**风险二：Claude 直接访问文件带来的资料暴露**。网页版中研究者必须手动粘贴资料给 LLM，这是一个"知道自己正在让 LLM 看什么"的有意识动作。命令行版的 `@` 引用和文件读取让 Claude 可以"顺手"接触到研究者未明确允许的文件（若 `.claudeignore` 配置不严）。**对策**：定期检查 `.claudeignore` 是否覆盖所有敏感路径；在每次 Plan Mode 的计划中注意 Claude 是否列出了不该读的文件。

**风险三：脚本自动化滑向分析自动化**。Claude Code 能写脚本的能力很容易滑向"让脚本替我做编码判断"。即使是研究者自己写的脚本，若其逻辑涉及分析判断（如关键词匹配推荐编码），也是对 CGT 的违反。**对策**：任何脚本在写完后，研究者在脑中过一遍"这个脚本做的是事务性工作（文件操作、索引、清单），还是分析判断（编码推荐、概念生成、相似度排序）"——后者一律不做。

---

## 8. 适配其他命令行 AI 工具

Claude Code 的配置机制是目前最成熟的。其他工具的机制略有差异，但核心设计原则可迁移：

**Codex（OpenAI）**：
- 配置文件位置不同（具体看当前版本的官方文档）
- 核心原则不变：启动时自动加载的约束文件、文件访问的 ignore 机制、禁用跨 session 学习
- 若 Codex 没有 slash commands 机制，改用文本化的"开场指令 + 任务类型"

**Aider**：
- 使用 `.aider.conf.yml` 配置
- 使用 `.aiderignore` 排除文件
- 开场指令通过 `--message-file` 注入
- 没有原生 slash commands，改用 shell aliases 包装

**通用适配原则**：

1. 找到该工具的"自动加载文件"机制，放入 CLAUDE.md 等价物
2. 找到该工具的"忽略文件"机制，放入 `.claudeignore` 等价物
3. 关闭该工具的任何跨 session 学习功能（等价于禁用 Auto Memory）
4. 将四类对话指令改写为该工具支持的格式（slash command、shell alias、或手动粘贴）
5. 将 `/close-session` 的功能改写为该工具可执行的脚本

具体配置可基于 CLAUDE.md 与 `.claude/rules/` 的内容翻译到新工具的格式。核心约束不变，形式可适配。

---

## 9. 初次使用的建议

对命令行 AI 工具不熟悉的研究者，推荐按以下顺序上手：

**第 1 天**：搭建好所有文件，测试 slash commands 能正常运行。不做研究相关工作。熟悉 `claude` 启动与退出。

**第 2-3 天**：跑一次 `/type-a` session，问几个你已经知道答案的方法论问题，检验 Claude 的回应是否符合 SKILL 的约束。这是"验收"阶段——若 Claude 的回应偏离了 SKILL，说明搭建有问题，调整 `.claude/rules/` 或 CLAUDE.md。

**第 4-7 天**：用少量真实资料（1-2 位受访者）跑一次完整的工作流——开放编码、备忘录写作、`/type-b` 比较、`/type-c` 语言质询、`/close-session`。观察各个环节是否顺畅。

**第 2-4 周**：正常研究推进。保留第 1-2 次 `/type-d` session 的诊断报告作为研究基线。

**从第 5 周开始**：根据使用经验修订 CLAUDE.md 与 `.claude/rules/`——这些文件不是一次性写好的，需要根据实际使用中 Claude 的偏差反复修订。每次修订在 Git 中留档。

不要期待搭建后立即高效使用。命令行工具与 CGT 方法论的磨合需要时间——这个磨合本身也是研究者对方法论的深化理解过程。

---

## 10. 与网页版的并行使用

有些情况下两种版本并用最合适：

- 在家中简单咨询方法论问题：网页版（无需打开项目目录）
- 做批量资料整理：命令行版（脚本效率高）
- 类型 D 反思 session：命令行版（完整项目 context）
- 讨论一个具体备忘录的语言：任一版本均可
- 研究者在外出差只有手机：网页版（命令行不可用）

两种版本的对话日志格式一致（都使用 `dialogue_log_TEMPLATE.md`），归档到同一本地目录。`research_state.md` 在两处保持一致——网页版通过手工覆盖上传，命令行版直接读本地文件。

---

*文件结束。本指南随使用经验迭代。修订时保留版本号与更新日期。*
