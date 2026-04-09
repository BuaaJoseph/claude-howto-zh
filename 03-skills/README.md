<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Agent Skills Guide

Agent Skills 是可复用的、基于文件系统的能力扩展，能增强 Claude 的功能。它们将领域专业知识、工作流和最佳实践打包为可发现的组件，Claude 会在相关场景下自动使用。

## 概述

**Agent Skills** 是模块化的能力，可将通用 AI 助手转变为专业选手。与提示词（针对一次性任务的对话级指令）不同，Skills 采用按需加载模式，无需在多次对话中重复提供相同指导。

### 核心优势

- **专业化 Claude**：针对特定领域任务定制能力
- **减少重复**：创建一次，自动在所有对话中使用
- **组合能力**：将多个 Skills 组合以构建复杂工作流
- **规模化工作流**：在多个项目和团队中复用 Skills
- **保证质量**：将最佳实践直接嵌入工作流

Skills 遵循 [Agent Skills](https://agentskills.io) 开放标准，可在多种 AI 工具中使用。Claude Code 在此标准基础上增加了调用控制、子agent 执行和动态上下文注入等额外功能。

> **注意**：自定义斜杠命令已合并到 Skills 中。`.claude/commands/` 文件仍然有效，并支持相同的 frontmatter 字段。推荐使用 Skills 进行新开发。当同一路径同时存在两者时（如 `.claude/commands/review.md` 和 `.claude/skills/review/SKILL.md`），Skill 优先。

## Skills 如何工作：渐进式披露

Skills 采用**渐进式披露**架构——Claude 按需分阶段加载信息，而非预先加载所有内容。这实现了高效的上下文管理，同时保持无限的可扩展性。

### 三个加载级别

```mermaid
graph TB
    subgraph "Level 1: Metadata (Always Loaded)"
        A["YAML Frontmatter"]
        A1["~100 tokens per skill"]
        A2["name + description"]
    end

    subgraph "Level 2: Instructions (When Triggered)"
        B["SKILL.md Body"]
        B1["Under 5k tokens"]
        B2["Workflows & guidance"]
    end

    subgraph "Level 3: Resources (As Needed)"
        C["Bundled Files"]
        C1["Effectively unlimited"]
        C2["Scripts, templates, docs"]
    end

    A --> B
    B --> C
```

| 级别 | 加载时机 | Token 消耗 | 内容 |
|-------|------------|------------|---------|
| **Level 1: 元数据** | 始终加载（启动时） | 每个 Skill 约 100 tokens | YAML frontmatter 中的 `name` 和 `description` |
| **Level 2: 指令** | Skill 被触发时 | 低于 5k tokens | SKILL.md 正文，包含指令和指导 |
| **Level 3+: 资源** | 按需加载 | 实际无限制 | 绑定的文件，通过 bash 执行，不加载内容到上下文 |

这意味着你可以安装多个 Skills 而不消耗上下文——Claude 只知道每个 Skill 的存在和使用时机，在实际触发前不会加载具体内容。

## Skill 加载流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as System
    participant Skill as Skill

    User->>Claude: "Review this code for security issues"
    Claude->>System: Check available skills (metadata)
    System-->>Claude: Skill descriptions loaded at startup
    Claude->>Claude: Match request to skill description
    Claude->>Skill: bash: read code-review/SKILL.md
    Skill-->>Claude: Instructions loaded into context
    Claude->>Claude: Determine: Need templates?
    Claude->>Skill: bash: read templates/checklist.md
    Skill-->>Claude: Template loaded
    Claude->>Claude: Execute skill instructions
    Claude->>User: Comprehensive code review
```

## Skill 类型与位置

| 类型 | 位置 | 作用域 | 共享 | 适用场景 |
|------|----------|-------|--------|----------|
| **Enterprise** | 托管设置 | 所有组织用户 | 是 | 组织级标准 |
| **Personal** | `~/.claude/skills/<skill-name>/SKILL.md` | 个人 | 否 | 个人工作流 |
| **Project** | `.claude/skills/<skill-name>/SKILL.md` | 团队 | 是（通过 git） | 团队标准 |
| **Plugin** | `<plugin>/skills/<skill-name>/SKILL.md` | 启用位置 | 取决于插件 | 与插件绑定 |

当多级目录中存在同名 Skills 时，高优先级优先：**enterprise > personal > project**。Plugin Skills 使用 `plugin-name:skill-name` 命名空间，不会冲突。

### 自动发现

**嵌套目录**：当你在子目录中操作文件时，Claude Code 会自动发现嵌套的 `.claude/skills/` 目录中的 Skills。例如，当你在编辑 `packages/frontend/` 中的文件时，Claude Code 也会查找 `packages/frontend/.claude/skills/` 中的 Skills。这支持 monorepo 设置中每个包拥有自己的 Skills。

**`--add-dir` 目录**：通过 `--add-dir` 添加的目录中的 Skills 会自动加载，并支持实时变更检测。对这些目录中 Skill 文件的任何修改会立即生效，无需重启 Claude Code。

**描述预算**：Skill 描述（Level 1 元数据）上限为**上下文窗口的 2%**（回退：**16,000 字符**）。如果你安装了很多 Skills，部分可能会被排除。运行 `/context` 检查是否有警告。可通过 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量覆盖预算。

## 创建自定义 Skills

### 基本目录结构

```
my-skill/
├── SKILL.md           # 主指令（必需）
├── template.md        # Claude 填充的模板
├── examples/
│   └── sample.md      # 展示预期格式的示例
└── scripts/
    └── validate.sh    # Claude 可执行的脚本
```

### SKILL.md 格式

```yaml
---
name: your-skill-name
description: 简要描述此 Skill 的功能及使用场景
---

# Your Skill Name

## Instructions
提供清晰、逐步的指导。

## Examples
展示使用此 Skill 的具体示例。
```

### 必需字段

- **name**：仅使用小写字母、数字和连字符（最多 64 字符）。不能包含 "anthropic" 或 "claude"。
- **description**：Skill 的功能及使用场景（最多 1024 字符）。这对于 Claude 决定何时激活该 Skill 至关重要。

### 可选的 Frontmatter 字段

```yaml
---
name: my-skill
description: 此 Skill 的功能及使用场景
argument-hint: "[filename] [format]"        # 自动补全提示
disable-model-invocation: true              # 仅用户可调用
user-invocable: false                       # 从斜杠菜单隐藏
allowed-tools: Read, Grep, Glob             # 限制工具访问
model: opus                                 # 指定使用的模型
effort: high                                # 工作量级别覆盖 (low, medium, high, max)
context: fork                               # 在隔离的子 agent 中运行
agent: Explore                              # 子 agent 类型（配合 context: fork 使用）
shell: bash                                 # 命令使用的 shell：bash（默认）或 powershell
hooks:                                      # Skill 级别的钩子
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate.sh"
---
```

| 字段 | 描述 |
|-------|-------------|
| `name` | 仅小写字母、数字和连字符（最多 64 字符）。不能包含 "anthropic" 或 "claude"。 |
| `description` | Skill 的功能及使用场景（最多 1024 字符）。对自动调用匹配至关重要。 |
| `argument-hint` | 在 `/` 自动补全菜单中显示的提示（如 `"[filename] [format]"`）。 |
| `disable-model-invocation` | `true` = 仅用户可通过 `/name` 调用。Claude 永远不会自动调用。 |
| `user-invocable` | `false` = 从 `/` 菜单隐藏。仅 Claude 可以自动调用。 |
| `allowed-tools` | Skill 可以使用而不弹出权限提示的工具列表（逗号分隔）。 |
| `model` | Skill 激活时的模型覆盖（如 `opus`、`sonnet`）。 |
| `effort` | Skill 激活时的工作量级别覆盖：`low`、`medium`、`high` 或 `max`。 |
| `context` | `fork` 在分叉的子 agent 上下文中运行，有独立的上下文窗口。 |
| `agent` | 当 `context: fork` 时的子 agent 类型（如 `Explore`、`Plan`、`general-purpose`）。 |
| `shell` | `!`command`` 替换和脚本使用的 shell：`bash`（默认）或 `powershell`。 |
| `hooks` | 绑定到此 Skill 生命周期的钩子（与全局钩子格式相同）。 |

## Skill 内容类型

Skills 可包含两种类型的内容，每种适用于不同目的：

### 参考内容

为 Claude 添加应用于当前工作的知识——约定、模式、风格指南、领域知识。与对话上下文一起内联运行。

```yaml
---
name: api-conventions
description: 此代码库的 API 设计模式
---

编写 API 端点时：
- 使用 RESTful 命名约定
- 返回一致的错误格式
- 包含请求验证
```

### 任务内容

逐步执行特定操作的指令。通常直接用 `/skill-name` 调用。

```yaml
---
name: deploy
description: 将应用部署到生产环境
context: fork
disable-model-invocation: true
---

部署应用：
1. 运行测试套件
2. 构建应用
3. 推送到部署目标
```

## 控制 Skill 调用

默认情况下，你和 Claude 都可以调用任何 Skill。两个 frontmatter 字段控制三种调用模式：

| Frontmatter | 你可以调用 | Claude 可以调用 |
|---|---|---|
| （默认） | 是 | 是 |
| `disable-model-invocation: true` | 是 | 否 |
| `user-invocable: false` | 否 | 是 |

**使用 `disable-model-invocation: true`** 用于有副作用的工作流：`/commit`、`/deploy`、`/send-slack-message`。你不希望 Claude 因为代码看起来Ready就决定部署。

**使用 `user-invocable: false`** 作为不可作为命令执行的背景知识。`legacy-system-context` Skill 解释旧系统如何运作——对 Claude 有用，但对用户没有实际意义。

## 字符串替换

Skills 支持在内容发送给 Claude 之前解析的动态值：

| 变量 | 描述 |
|----------|-------------|
| `$ARGUMENTS` | 调用 Skill 时传递的所有参数 |
| `$ARGUMENTS[N]` 或 `$N` | 按索引访问特定参数（0 开始） |
| `${CLAUDE_SESSION_ID}` | 当前会话 ID |
| `${CLAUDE_SKILL_DIR}` | 包含 Skill 的 SKILL.md 文件的目录 |
| `` !`command` `` | 动态上下文注入 —— 运行 shell 命令并将输出内联 |

**示例：**

```yaml
---
name: fix-issue
description: 修复 GitHub issue
---

修复 GitHub issue $ARGUMENTS，遵循我们的编码标准。
1. 阅读 issue 描述
2. 实现修复
3. 编写测试
4. 创建提交
```

运行 `/fix-issue 123` 会将 `$ARGUMENTS` 替换为 `123`。

## 注入动态上下文

`!`command`` 语法在 Skill 内容发送给 Claude 之前运行 shell 命令：

```yaml
---
name: pr-summary
description: 总结拉取请求中的变更
context: fork
agent: Explore
---

## 拉取请求上下文
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- 变更的文件: !`gh pr diff --name-only`

## 你的任务
总结此拉取请求...
```

命令立即执行；Claude 只看到最终输出。默认情况下，命令在 `bash` 中运行。在 frontmatter 中设置 `shell: powershell` 可改为使用 PowerShell。

## 在子 Agent 中运行 Skills

添加 `context: fork` 可在隔离的子 agent 上下文中运行 Skill。Skill 内容成为专用子 agent 的任务，有独立的上下文窗口���保���主对话整洁。

`agent` 字段指定使用的 agent 类型：

| Agent 类型 | 适用场景 |
|---|---|
| `Explore` | 只读研究、代码库分析 |
| `Plan` | 创建实施计划 |
| `general-purpose` | 需要所有工具的广泛任务 |
| 自定义 agents | 配置中定义的专业 agents |

**示例 frontmatter：**

```yaml
---
context: fork
agent: Explore
---
```

**完整 Skill 示例：**

```yaml
---
name: deep-research
description: 彻底研究一个主题
context: fork
agent: Explore
---

彻底研究 $ARGUMENTS：
1. 使用 Glob 和 Grep 查找相关文件
2. 阅读和分析代码
3. 总结发现，包含具体文件引用
```

## 实际示例

### 示例 1：代码审查 Skill

**目录结构：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**文件：** `~/.claude/skills/code-review/SKILL.md`

```yaml
---
name: code-review-specialist
description: 全面的代码审查，包含安全、性能和质量分析。当用户要求审查代码、分析代码质量、评估拉取请求，或提到代码审查、安全分析或性能优化时使用。
---

# Code Review Skill

此 Skill 提供全面的代码审查能力，重点关注：

1. **安全分析**
   - 认证/授权问题
   - 数据暴露风险
   - 注入漏洞
   - 加密弱点

2. **性能审查**
   - 算法效率（大 O 分析）
   - 内存优化
   - 数据库查询优化
   - 缓存机会

3. **代码质量**
   - SOLID 原则
   - 设计模式
   - 命名约定
   - 测试覆盖率

4. **可维护性**
   - 代码可读性
   - 函数大小（应小于 50 行）
   - 圈复杂度
   - 类型安全

## 审查模板

对每段审查的代码，提供：

### 摘要
- 整体质量评估（1-5）
- 关键发现数量
- 推荐优先级领域

### 关键问题（如有）
- **问题**：清晰描述
- **位置**：文件和行号
- **影响**：为何重要
- **严重性**：Critical/High/Medium
- **修复**：代码示例

详细清单见 [templates/review-checklist.md](templates/review-checklist.md)。
```

### 示例 2：代码库可视化 Skill

生成交互式 HTML 可视化：

**目录结构：**

```
~/.claude/skills/codebase-visualizer/
├── SKILL.md
└── scripts/
    └── visualize.py
```

**文件：** `~/.claude/skills/codebase-visualizer/SKILL.md`

```yaml
---
name: codebase-visualizer
description: 生成代码库的交互式可折叠树形可视化。在探索新仓库、理解项目结构或识别大文件时使用。
allowed-tools: Bash(python *)
---

# Codebase Visualizer

生成显示项目文件结构的交互式 HTML 树视图。

## 使用方法

从项目根目录运行可视化脚本：

```bash
python ~/.claude/skills/codebase-visualizer/scripts/visualize.py .
```

这会创建 `codebase-map.html` 并在默认浏览器中打开。

## 可视化显示的内容

- **可折叠目录**：点击文件夹展开/折叠
- **文件大小**：显示在每个文件旁
- **颜色**：不同文件类型不同颜色
- **目录总计**：显示每个文件夹的总大小
```

绑定的 Python 脚本负责繁重工作，Claude 负责编排。

### 示例 3：部署 Skill（仅用户调用）

```yaml
---
name: deploy
description: 将应用部署到生产环境
disable-model-invocation: true
allowed-tools: Bash(npm *), Bash(git *)
---

将 $ARGUMENTS 部署到生产环境：

1. 运行测试套件：`npm test`
2. 构建应用：`npm run build`
3. 推送到部署目标
4. 验证部署成功
5. 报告部署状态
```

### 示例 4：品牌调性 Skill（背景知识）

```yaml
---
name: brand-voice
description: 确保所有沟通符合品牌调性和风格指南。在创建营销文案、客户沟通或面向公众的内容时使用。
user-invocable: false
---

## 语气
- **友好但专业** —— 平易近人但不随意
- **清晰简洁** —— 避免行话
- **自信** —— 我们知道自己在做什么
- **同理心** —— 理解用户需求

## 写作指南
- 称呼读者时用"你"
- 使用主动语态
- 句子保持在 20 个词以内
- 从价值主张开始

模板见 [templates/](templates/)。
```

### 示例 5：CLAUDE.md 生成 Skill

```yaml
---
name: claude-md
description: 按照最佳实践创建或更新 CLAUDE.md 文件，以实现最佳的 AI agent 入职培训。当用户提到 CLAUDE.md、项目文档或 AI 入职培训时使用。
---

## 核心原则

**LLM 是无状态的**：CLAUDE.md 是唯一在每个对话中自动包含的文件。

### 金科玉律

1. **少即是多**：保持在 300 行以下（最好低于 100 行）
2. **普遍适用性**：只包含与每个会话相关的信息
3. **不要把 Claude 当作 Linter 使用**：改用确定性工具
4. **永远不要自动生成**：手动精心制作

## 必需部分

- **项目名称**：简明的一行描述
- **技术栈**：主要语言、框架、数据库
- **开发命令**：安装、测试、构建命令
- **关键约定**：仅包含不明显但影响重大的约定
- **已知问题/陷阱**：容易让开发者踩坑的事项
```

### 示例 6：重构 Skill（含脚本）

**目录结构：**

```
refactor/
├── SKILL.md
├── references/
│   ├── code-smells.md
│   └── refactoring-catalog.md
├── templates/
│   └── refactoring-plan.md
└── scripts/
    ├── analyze-complexity.py
    └── detect-smells.py
```

**文件：** `refactor/SKILL.md`

```yaml
---
name: code-refactor
description: 基于 Martin Fowler 方法论的系统化代码重构。当用户要求重构代码、改善代码结构、减少技术债务或消除代码异���时���用。
---

# Code Refactor Skill

强调安全的增量变更的分阶段方法。

## 工作流

阶段 1：研究与分析 → 阶段 2：测试覆盖率评估 →
阶段 3：代码异味识别 → 阶段 4：重构计划创建 →
阶段 5：增量实施 → 阶段 6：审查与迭代

## 核心原则

1. **行为保持**：外部行为必须保持不变
2. **小步前进**：做微小的、可测试的变更
3. **测试驱动**：测试是安全网
4. **持续进行**：重构是持续进行的事件，不是一次性事件

代码异味目录见 [references/code-smells.md](references/code-smells.md)。
重构技术见 [references/refactoring-catalog.md](references/refactoring-catalog.md)。
```

## 支持文件

Skills 可以在目录中包含除 `SKILL.md` 之外的多个文件。这些支持文件（模板、示例、脚本、参考文档）让你保持主 Skill 文件简洁，同时为 Claude 提供按需加载的额外资源。

```
my-skill/
├── SKILL.md              # 主指令（必需，保持在 500 行以下）
├── templates/            # Claude 填充的模板
│   └── output-format.md
├── examples/             # 展示预期格式的示例输出
│   └── sample-output.md
├── references/           # 领域知识和规格
│   └── api-spec.md
└── scripts/              # Claude 可执行的脚本
    └── validate.sh
```

支持文件指南：

- 保持 `SKILL.md` 在**500 行以下**。将详细参考材料、大型示例和规格移到单独的文件。
- 使用**相对路径**从 `SKILL.md` 引用其他文件（如 `[API reference](references/api-spec.md)`）。
- 支持文件在 Level 3 加载（按需），因此在 Claude 实际读取之前不会消耗上下文。

## 管理 Skills

### 查看可用 Skills

直接询问 Claude：
```
有哪些 Skills 可用？
```

或检查文件系统：
```bash
# 列出个人 Skills
ls ~/.claude/skills/

# 列出项目 Skills
ls .claude/skills/
```

### 测试 Skill

两种测试方式：

**让 Claude 自动调用**，询问匹配描述的内容：
```
你能帮我审查这段代码的安全问题吗？
```

**或直接用技能名调用**：
```
/code-review src/auth/login.ts
```

### 更新 Skill

直接编辑 `SKILL.md` 文件。更改在下次 Claude Code 启动时生效。

```bash
# 个人 Skill
code ~/.claude/skills/my-skill/SKILL.md

# 项目 Skill
code .claude/skills/my-skill/SKILL.md
```

### 限制 Claude 的 Skill 访问

有三种方式控制 Claude 可以调用的 Skills：

**在 `/permissions` 中禁用所有 Skills**：
```
# 添加到拒绝规则：
Skill
```

**允许或拒绝特定 Skills**：
```
# 只允许特定 Skills
Skill(commit)
Skill(review-pr *)

# 拒绝特定 Skills
Skill(deploy *)
```

**隐藏单个 Skills**：在 frontmatter 中添加 `disable-model-invocation: true`。

## 最佳实践

### 1. 描述要具体

- **差（模糊）**："帮助处理文档"
- **好（具体）**："从 PDF 文件提取文本和表格，填写表单，合并文档。在处理 PDF 文件或用户提到 PDF、表单或文档提取时使用。"

### 2. 保持 Skills 专注

- 一个 Skill = 一个能力
- ✅ "PDF 表单填写"
- ❌ "文档处理"（太宽泛）

### 3. 包含触发词

在描述中添加匹配用户请求的关键词：
```yaml
description: 分析 Excel 电子表格，生成数据透视表，创建图表。在处理 Excel 文件、电子表格或 .xlsx 文件时使用。
```

### 4. 保持 SKILL.md 在 500 行以下

将详细参考材料移到 Claude 按需加载的单独文件中。

### 5. 引用支持文件

```markdown
## 其他资源

- 完整 API 详情见 [reference.md](reference.md)
- 使用示例见 [examples.md](examples.md)
```

推荐做法

- 使用清晰、有描述性的名称
- 包含全面的指令
- 添加具体示例
- 打包相关的脚本和模板
- 用真实场景测试
- 记录依赖关系

不推荐做法

- 不要为一次性任务创建 Skills
- 不要复制现有功能
- 不要让 Skills 过于宽泛
- 不要跳过描述字段
- 安装来自不可信来源的 Skills 前要先审计

## 故障排除

### 快速参考

| 问题 | 解决方案 |
|-------|----------|
| Claude 不使用 Skill | 用触发词让描述更具体 |
| 找不到 Skill 文件 | 验证路径：`~/.claude/skills/name/SKILL.md` |
| YAML 错误 | 检查 `---` 标记、缩进，不要用 tab |
| Skills 冲突 | 在描述中使用不同的触发词 |
| 脚本不运行 | 检查权限：`chmod +x scripts/*.py` |
| Claude 看不到所有 Skills | Skills 太多；检查 `/context` 警告 |

### Skill 不触发

如果 Claude 没有按预期使用你的 Skill：

1. 检查描述是否包含用户自然会说到的关键词
2. 验证询问"有哪些 Skills 可用？"时能看到该 Skill
3. 尝试重新措辞请求以匹配描述
4. 用 `/skill-name` 直接调用来测试

### Skill 触发过于频繁

如果 Claude 在不需要时使用了你的 Skill：

1. 让描述更具体
2. 添加 `disable-model-invocation: true` 设为手动调用

### Claude 看不到所有 Skills

Skill 描述在**上下文窗口的 2%** 处加载（回退：**16,000 字符**）。运行 `/context` 检查是否有 Skills 被排除的警告。可通过 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量覆盖预算。

## 安全注意事项

**只使用来自可信来源的 Skills。** Skills 通过指令和代码为 Claude 提供能力——恶意的 Skill 可以指挥 Claude 以有害方式调用工具或执行代码。

**关键安全注意事项：**

- **彻底审计**：审计 Skill 目录中的所有文件
- **外部来源有风险**：从外部 URL 获取的 Skills 可能被篡改
- **工具滥用**：恶意的 Skills 可能以有害方式调用工具
- **像安装软件一样对待**：只使用来自可信来源的 Skills

## Skills 与其他功能对比

| 功能 | 调用方式 | 最佳用途 |
|---------|------------|----------|
| **Skills** | 自动或 `/name` | 可复用专业知识、工作流 |
| **斜杠命令** | 用户发起 `/name` | 快速快捷方式（已合并到 Skills） |
| **子 Agents** | 自动委托 | 隔离任务执行 |
| **内存 (CLAUDE.md)** | 始终加载 | 持久项目上下文 |
| **MCP** | 实时 | 外部数据/服务访问 |
| **钩子** | 事件驱动 | 自动副作用 |

## 捆绑 Skills

Claude Code 附带多个内置 Skills，无需安装即可使用：

| Skill | 描述 |
|-------|-------------|
| `/simplify` | 审查变更文件以进行复用、质量和效率检查；生成 3 个并行审查代理 |
| `/batch <instruction>` | 使用 git worktrees 跨代码库编排大规模并行变更 |
| `/debug [description]` | 通过读取调试日志排查当前会话问题 |
| `/loop [interval] <prompt>` | 按间隔重复运行提示（如 `/loop 5m check the deploy`） |
| `/claude-api` | 加载 Claude API/SDK 参考；在 `anthropic`/`@anthropic-ai/sdk` 导入时自动激活 |

这些 Skills 开箱即用，无需安装或配置。它们遵循与自定义 Skills 相同的 SKILL.md 格式。

## 分享 Skills

### 项目 Skills（团队共享）

1. 在 `.claude/skills/` 创建 Skill
2. 提交到 git
3. 团队成员拉取变更 —— Skills 立即可用

### 个人 Skills

```bash
# 复制到个人目录
cp -r my-skill ~/.claude/skills/

# 使脚本可执行
chmod +x ~/.claude/skills/my-skill/scripts/*.py
```

### 插件分发

在插件的 `skills/` 目录中打包 Skills 以实现更广泛分发。

## 进一步探索：Skill 集合和 Skill 管理器

一旦你开始认真构建 Skills，有两件事变得必不可少：一个经过验证的 Skills 库和管理它们的工具。

**[luongnv89/skills](https://github.com/luongnv89/skills)** — 我日常在几乎所有项目中使用的 Skills 集合。亮点包括 `logo-designer`（即时生成项目 Logo）和 `ollama-optimizer`（为本地 LLM 优化硬件性能）。如果想要现成的 Skills，这是很好的起点。

**[luongnv89/asm](https://github.com/luongnv89/asm)** — Agent Skill Manager。处理 Skill 开发、重复检测和测试。`asm link` 命令让你在任何项目中测试 Skill 而无需复制文件——当你拥有多个 Skills 时这是必需的。

## 其他资源

- [官方 Skills 文档](https://code.claude.com/docs/en/skills)
- [Agent Skills 架构博客](https://claude.com/blog/equipping-agents-for-the-real-world-with-agent-skills)
- [Skills 仓库](https://github.com/luongnv89/skills) — 现成 Skills 集合
- [斜杠命令指南](../01-slash-commands/) — 用户发起的快捷方式
- [子 Agents 指南](../04-subagents/) — 委托的 AI 代理
- [内存指南](../02-memory/) — 持久上下文
- [MCP（模型上下文协议）](../05-mcp/) — 实时外部数据
- [钩子指南](../06-hooks/) — 事件驱动自动化