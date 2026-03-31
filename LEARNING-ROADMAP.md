# 📚 Claude Code 学习路线图

**初次接触 Claude Code？** 本指南帮助您按照自己的节奏掌握 Claude Code 功能。无论您是初学者还是经验丰富的开发者，都可以从下面的自我评估测验开始，找到适合您的起点。

---

## 🧭 找到您的级别

并非所有人都从同一起点开始。快速完成自我评估测验，找到合适的切入点。

**诚实回答以下问题：**

- [ ] 我可以启动 Claude Code 并进行对话 (`claude`)
- [ ] 我已创建或编辑过 CLAUDE.md 文件
- [ ] 我使用过至少 3 个内置 slash commands（如 /help、/compact、/model）
- [ ] 我已创建过自定义 slash command 或 skill（SKILL.md）
- [ ] 我已配置过 MCP server（如 GitHub、数据库）
- [ ] 我已在 ~/.claude/settings.json 中设置过 hooks
- [ ] 我已创建或使用过自定义 subagents（.claude/agents/）
- [ ] 我使用过 print mode（`claude -p`）进行脚本化或 CI/CD

**您的级别：**

| 勾选数 | 级别 | 起始点 | 预计完成时间 |
|--------|------|--------|--------------|
| 0-2 | **级别 1：入门** — 快速入门 | [里程碑 1A](#milestone-1a-first-commands--memory) | 约 3 小时 |
| 3-5 | **级别 2：中级** — 构建工作流 | [里程碑 2A](#milestone-2a-automation-skills--hooks) | 约 5 小时 |
| 6-8 | **级别 3：高级** — 高级用户与团队负责人 | [里程碑 3A](#milestone-3a-advanced-features) | 约 5 小时 |

> **提示**：如果不确定，从低一级开始。快速复习熟悉的内容比错过基础知识更好。

> **交互版本**：在 Claude Code 中运行 `/self-assessment`，获取涵盖所有 10 个功能领域的引导式交互测验，并生成个性化学习路径。

---

## 🎯 学习理念

本仓库的文件夹按照**推荐学习顺序**编号，基于三个关键原则：

1. **依赖关系** — 基础概念优先
2. **复杂度** — 简单功能先于复杂功能
3. **使用频率** — 最常用的功能早期教学

这种方法确保您在打下坚实基础的同时，能立即获得生产力的提升。

---

## 🗺️ 您的学习路径

```mermaid
graph TD
    Q["🧭 自我评估测验<br/>找到您的级别"] --> L1
    Q --> L2
    Q --> L3

    subgraph L1["🟢 级别 1：入门 — 快速入门"]
        direction LR
        A["1A：首个命令与内存<br/>Slash Commands + Memory"] --> B["1B：安全探索<br/>Checkpoints + CLI 基础"]
    end

    subgraph L2["🔵 级别 2：中级 — 构建工作流"]
        direction LR
        C["2A：自动化<br/>Skills + Hooks"] --> D["2B：集成<br/>MCP + Subagents"]
    end

    subgraph L3["🔴 级别 3：高级 — 高级用户"]
        direction LR
        E["3A：高级功能<br/>Planning + Permissions"] --> F["3B：团队与分发<br/>Plugins + CLI 精通"]
    end

    L1 --> L2
    L2 --> L3

    style Q fill:#6A1B9A,color:#fff,stroke:#9C27B0,stroke-width:2px
    style A fill:#2E7D32,color:#fff
    style B fill:#2E7D32,color:#fff
    style C fill:#1565C0,color:#fff
    style D fill:#F57C00,color:#fff
    style E fill:#C62828,color:#fff
    style F fill:#B71C1C,color:#fff
```

**颜色图例：**
- 💜 紫色：自我评估测验
- 🟢 绿色：级别 1 — 入门路径
- 🔵 蓝色 / 🟡 金色：级别 2 — 中级路径
- 🔴 红色：级别 3 — 高级路径

---

## 📊 完整路线表格

| 步骤 | 功能 | 复杂度 | 时间 | 级别 | 依赖项 | 为什么要学 | 核心收益 |
|------|------|--------|------|------|--------|------------|----------|
| **1** | [Slash Commands](01-slash-commands/) | ⭐ 入门 | 30 分钟 | 级别 1 | 无 | 快速提升生产力（55+ 内置 + 5 个捆绑 skills） | 即时自动化、团队标准 |
| **2** | [Memory](02-memory/) | ⭐⭐ 入门+ | 45 分钟 | 级别 1 | 无 | 所有功能的必要条件 | 持久上下文、偏好设置 |
| **3** | [Checkpoints](08-checkpoints/) | ⭐⭐ 中级 | 45 分钟 | 级别 1 | 会话管理 | 安全探索 | 实验、恢复 |
| **4** | [CLI 基础](10-cli/) | ⭐⭐ 入门+ | 30 分钟 | 级别 1 | 无 | 核心 CLI 用法 | 交互模式与 print 模式 |
| **5** | [Skills](03-skills/) | ⭐⭐ 中级 | 1 小时 | 级别 2 | Slash Commands | 自动调用专业能力 | 可重用能力、一致性 |
| **6** | [Hooks](06-hooks/) | ⭐⭐ 中级 | 1 小时 | 级别 2 | 工具、命令 | 工作流自动化（25 事件、4 类型） | 验证、质量门禁 |
| **7** | [MCP](05-mcp/) | ⭐⭐⭐ 中级+ | 1 小时 | 级别 2 | 配置 | 实时数据访问 | 实时集成、API |
| **8** | [Subagents](04-subagents/) | ⭐⭐⭐ 中级+ | 1.5 小时 | 级别 2 | 内存、命令 | 复杂任务处理（6 个内置含 Bash） | 委托、专业知识 |
| **9** | [高级功能](09-advanced-features/) | ⭐⭐⭐⭐⭐ 高级 | 2-3 小时 | 级别 3 | 所有之前内容 | 高级用户工具 | Planning、Auto Mode、Channels、Voice Dictation、权限 |
| **10** | [Plugins](07-plugins/) | ⭐⭐⭐⭐ 高级 | 2 小时 | 级别 3 | 所有之前内容 | 完整解决方案 | 团队入职、分发 |
| **11** | [CLI 精通](10-cli/) | ⭐⭐⭐ 高级 | 1 小时 | 级别 3 | 建议：全部 | 掌握命令行用法 | 脚本化、CI/CD、自动化 |

**总学习时间**：约 11-13 小时（或直接跳到您的级别节省时间）

---

## 🟢 级别 1：入门 — 快速入门

**适合**：测验勾选 0-2 项的用户
**时间**：约 3 小时
**重点**：即时生产力、理解基础知识
**成果**：日常舒适用户、准备好进入级别 2

### 里程碑 1A：首个命令与内存

**主题**：Slash Commands + 内存
**时间**：1-2 小时
**复杂度**：⭐ 入门
**目标**：通过自定义命令和持久上下文立即提升生产力

#### 您将实现
✅ 为重复性任务创建自定义 slash commands
✅ 为团队标准设置项目内存
✅ 配置个人偏好
✅ 了解 Claude 如何自动加载上下文

#### 实践练习

```bash
# 练习 1：安装您的首个 slash command
mkdir -p .claude/commands
cp 01-slash-commands/optimize.md .claude/commands/

# 练习 2：创建项目内存
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 练习 3：尝试使用
# 在 Claude Code 中输入：/optimize
```

#### 成功标准
- [ ] 成功调用 `/optimize` 命令
- [ ] Claude 从 CLAUDE.md 记住您的项目标准
- [ ] 您了解何时使用 slash commands 与内存

#### 下一步
熟悉后请阅读：
- [01-slash-commands/README.md](01-slash-commands/README.md)
- [02-memory/README.md](02-memory/README.md)

> **检验您的理解**：在 Claude Code 中运行 `/lesson-quiz slash-commands` 或 `/lesson-quiz memory` 来测试您所学。

---

### 里程碑 1B：安全探索

**主题**：Checkpoints + CLI 基础
**时间**：1 小时
**复杂度**：⭐⭐ 入门+
**目标**：学会安全实验并使用核心 CLI 命令

#### 您将实现
✅ 为安全实验创建和恢复 checkpoints
✅ 理解交互模式与 print 模式的区别
✅ 使用基本 CLI 参数和选项
✅ 通过管道处理文件

#### 实践练习

```bash
# 练习 1：尝试 checkpoint 工作流
# 在 Claude Code 中：
# 进行一些实验性更改，然后按 Esc+Esc 或使用 /rewind
# 选择实验前的 checkpoint
# 选择"恢复代码和对话"返回

# 练习 2：交互模式与 Print 模式
claude "解释这个项目"           # 交互模式
claude -p "解释这个函数"       # Print 模式（非交互）

# 练习 3：通过管道处理文件内容
cat error.log | claude -p "解释这个错误"
```

#### 成功标准
- [ ] 创建并恢复到 checkpoint
- [ ] 使用过交互模式和 print 模式
- [ ] 向 Claude 管道传输文件进行分析
- [ ] 理解何时使用 checkpoints 进行安全实验

#### 下一步
- 阅读：[08-checkpoints/README.md](08-checkpoints/README.md)
- 阅读：[10-cli/README.md](10-cli/README.md)
- **准备好进入级别 2！** 继续[里程碑 2A](#milestone-2a-automation-skills--hooks)

> **检验您的理解**：运行 `/lesson-quiz checkpoints` 或 `/lesson-quiz cli` 验证您准备好进入级别 2。

---

## 🔵 级别 2：中级 — 构建工作流

**适合**：测验勾选 3-5 项的用户
**时间**：约 5 小时
**重点**：自动化、集成、任务委托
**成果**：自动化工作流、外部集成、准备好进入级别 3

### 前提条件检查

在开始级别 2 之前，请确保您对以下级别 1 概念感到舒适：

- [ ] 可以创建和使用 slash commands（[01-slash-commands/](01-slash-commands/)）
- [ ] 已通过 CLAUDE.md 设置项目内存（[02-memory/](02-memory/)）
- [ ] 知道如何创建和恢复 checkpoints（[08-checkpoints/](08-checkpoints/)）
- [ ] 可以从命令行使用 `claude` 和 `claude -p`（[10-cli/](10-cli/)）

> **有缺口？** 在继续之前复习上面链接的教程。

---

### 里程碑 2A：自动化（Skills + Hooks）

**主题**：Skills + Hooks
**时间**：2-3 小时
**复杂度**：⭐⭐ 中级
**目标**：自动化常见工作流和质量检查

#### 您将实现
✅ 通过 YAML frontmatter 自动调用专业能力（包括 `effort` 和 `shell` 字段）
✅ 跨 25 个 hook 事件设置事件驱动自动化
✅ 使用全部 4 种 hook 类型（command、http、prompt、agent）
✅ 强制执行代码质量标准
✅ 为您的工作流创建自定义 hooks

#### 实践练习

```bash
# 练习 1：安装 skill
cp -r 03-skills/code-review ~/.claude/skills/

# 练习 2：设置 hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/pre-tool-check.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/pre-tool-check.sh

# 练习 3：在 settings 中配置 hooks
# 添加到 ~/.claude/settings.json：
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/pre-tool-check.sh"
          }
        ]
      }
    ]
  }
}
```

#### 成功标准
- [ ] 代码审查 skill 在相关时自动调用
- [ ] PreToolUse hook 在工具执行前运行
- [ ] 您理解 skill 自动调用与 hook 事件触发的区别

#### 下一步
- 创建您自己的自定义 skill
- 为您的工作流设置额外的 hooks
- 阅读：[03-skills/README.md](03-skills/README.md)
- 阅读：[06-hooks/README.md](06-hooks/README.md)

> **检验您的理解**：在继续之前运行 `/lesson-quiz skills` 或 `/lesson-quiz hooks` 测试您的知识。

---

### 里程碑 2B：集成（MCP + Subagents）

**主题**：MCP + Subagents
**时间**：2-3 小时
**复杂度**：⭐⭐⭐ 中级+
**目标**：集成外部服务并委托复杂任务

#### 您将实现
✅ 从 GitHub、数据库等访问实时数据
✅ 将工作委托给专业 AI agents
✅ 了解何时使用 MCP 与 subagents
✅ 构建集成工作流

#### 实践练习

```bash
# 练习 1：设置 GitHub MCP
export GITHUB_TOKEN="your_github_token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 练习 2：测试 MCP 集成
# 在 Claude Code 中：/mcp__github__list_prs

# 练习 3：安装 subagents
mkdir -p .claude/agents
cp 04-subagents/*.md .claude/agents/
```

#### 集成练习
尝试这个完整工作流：
1. 使用 MCP 获取 GitHub PR
2. 让 Claude 将审查委托给 code-reviewer subagent
3. 使用 hooks 自动运行测试

#### 成功标准
- [ ] 成功通过 MCP 查询 GitHub 数据
- [ ] Claude 将复杂任务委托给 subagents
- [ ] 您理解 MCP 和 subagents 之间的区别
- [ ] 在工作流中结合使用 MCP + subagents + hooks

#### 下一步
- 设置额外的 MCP servers（数据库、Slack 等）
- 为您的领域创建自定义 subagents
- 阅读：[05-mcp/README.md](05-mcp/README.md)
- 阅读：[04-subagents/README.md](04-subagents/README.md)
- **准备好进入级别 3！** 继续[里程碑 3A](#milestone-3a-advanced-features)

> **检验您的理解**：运行 `/lesson-quiz mcp` 或 `/lesson-quiz subagents` 验证您准备好进入级别 3。

---

## 🔴 级别 3：高级 — 高级用户与团队负责人

**适合**：测验勾选 6-8 项的用户
**时间**：约 5 小时
**重点**：团队工具、CI/CD、企业功能、插件开发
**成果**：高级用户，可以设置团队工作流和 CI/CD

### 前提条件检查

在开始级别 3 之前，请确保您对以下级别 2 概念感到舒适：

- [ ] 可以创建和使用具有自动调用功能的 skills（[03-skills/](03-skills/)）
- [ ] 已设置 hooks 进行事件驱动自动化（[06-hooks/](06-hooks/)）
- [ ] 可以配置 MCP servers 访问外部数据（[05-mcp/](05-mcp/)）
- [ ] 知道如何使用 subagents 进行任务委托（[04-subagents/](04-subagents/)）

> **有缺口？** 在继续之前复习上面链接的教程。

---

### 里程碑 3A：高级功能

**主题**：高级功能（Planning、Permissions、Extended Thinking、Auto Mode、Channels、Voice Dictation、Remote/Desktop/Web）
**时间**：2-3 小时
**复杂度**：⭐⭐⭐⭐⭐ 高级
**目标**：掌握高级工作流和高级用户工具

#### 您将实现
✅ 复杂功能的 Planning mode
✅ 6 种模式的细粒度权限控制（default、acceptEdits、plan、auto、dontAsk、bypassPermissions）
✅ 通过 Alt+T / Option+T 切换 Extended thinking
✅ 后台任务管理
✅ Auto Memory 保存学习到的偏好
✅ 带后台安全分类器的 Auto Mode
✅ Channels 实现结构化多会话工作流
✅ Voice Dictation 实现免提交互
✅ 远程控制、桌面应用和 Web 会话
✅ Agent Teams 实现多 agent 协作

#### 实践练习

```bash
# 练习 1：使用 planning mode
/plan 实现用户认证系统

# 练习 2：尝试权限模式（6 种可用：default、acceptEdits、plan、auto、dontAsk、bypassPermissions）
claude --permission-mode plan "分析这个代码库"
claude --permission-mode acceptEdits "重构 auth 模块"
claude --permission-mode auto "实现这个功能"

# 练习 3：启用 extended thinking
# 在会话期间按 Alt+T（macOS 上为 Option+T）切换

# 练习 4：高级 checkpoint 工作流
# 1. 创建 checkpoint "Clean state"
# 2. 使用 planning mode 设计功能
# 3. 通过 subagent 委托实现
# 4. 在后台运行测试
# 5. 如果测试失败，回退到 checkpoint
# 6. 尝试另一种方法

# 练习 5：尝试 auto mode（后台安全分类器）
claude --permission-mode auto "实现用户设置页面"

# 练习 6：启用 agent teams
export CLAUDE_AGENT_TEAMS=1
# 询问 Claude："使用团队方法实现功能 X"

# 练习 7：计划任务
/loop 5m /check-status
# 或使用 CronCreate 实现持久计划任务

# 练习 8：Channels 实现多会话工作流
# 使用 channels 跨会话组织工作

# 练习 9：Voice Dictation
# 使用语音输入与 Claude Code 进行免提交互
```

#### 成功标准
- [ ] 为复杂功能使用过 planning mode
- [ ] 配置过权限模式（plan、acceptEdits、auto、dontAsk）
- [ ] 使用 Alt+T / Option+T 切换过 extended thinking
- [ ] 使用过带后台安全分类器的 auto mode
- [ ] 使用过后台任务进行长时间操作
- [ ] 探索过 Channels 实现多会话工作流
- [ ] 尝试过 Voice Dictation 进行免提输入
- [ ] 理解远程控制、桌面应用和 Web 会话
- [ ] 启用并使用过 Agent Teams 进行协作任务
- [ ] 使用 `/loop` 进行重复任务或计划监控

#### 下一步
- 阅读：[09-advanced-features/README.md](09-advanced-features/README.md)

> **检验您的理解**：运行 `/lesson-quiz advanced` 测试您对高级用户功能的掌握。

---

### 里程碑 3B：团队与分发（Plugins + CLI 精通）

**主题**：Plugins + CLI 精通 + CI/CD
**时间**：2-3 小时
**复杂度**：⭐⭐⭐⭐ 高级
**目标**：构建团队工具、创建插件、掌握 CI/CD 集成

#### 您将实现
✅ 安装和创建完整的捆绑插件
✅ 掌握 CLI 进行脚本化和自动化
✅ 使用 `claude -p` 设置 CI/CD 集成
✅ 自动化流水线的 JSON 输出
✅ 会话管理和批处理

#### 实践练习

```bash
# 练习 1：安装完整插件
# 在 Claude Code 中：/plugin install pr-review

# 练习 2：Print mode 用于 CI/CD
claude -p "运行所有测试并生成报告"

# 练习 3：JSON 输出用于脚本
claude -p --output-format json "列出所有函数"

# 练习 4：会话管理和恢复
claude -r "feature-auth" "继续实现"

# 练习 5：CI/CD 集成与约束
claude -p --max-turns 3 --output-format json "审查代码"

# 练习 6：批处理
for file in *.md; do
  claude -p --output-format json "总结这个：$(cat $file)" > ${file%.md}.summary.json
done
```

#### CI/CD 集成练习
创建一个简单的 CI/CD 脚本：
1. 使用 `claude -p` 审查更改的文件
2. 输出 JSON 结果
3. 用 `jq` 处理特定问题
4. 集成到 GitHub Actions 工作流

#### 成功标准
- [ ] 安装并使用过插件
- [ ] 为您的团队构建或修改过插件
- [ ] 在 CI/CD 中使用过 print mode（`claude -p`）
- [ ] 生成过用于脚本化的 JSON 输出
- [ ] 成功恢复过之前的会话
- [ ] 创建过批处理脚本
- [ ] 将 Claude 集成到 CI/CD 工作流

#### CLI 实际用例
- **代码审查自动化**：在 CI/CD 流水线中运行代码审查
- **日志分析**：分析错误日志和系统输出
- **文档生成**：批量生成文档
- **测试洞察**：分析测试失败
- **性能分析**：审查性能指标
- **数据处理**：转换和分析数据文件

#### 下一步
- 阅读：[07-plugins/README.md](07-plugins/README.md)
- 阅读：[10-cli/README.md](10-cli/README.md)
- 创建团队范围的 CLI 快捷方式和插件
- 设置批处理脚本

> **检验您的理解**：运行 `/lesson-quiz plugins` 或 `/lesson-quiz cli` 确认您的掌握。

---

## 🧪 测试您的知识

此仓库包含两个可在 Claude Code 中随时使用的交互式 skills，用于评估您的理解：

| Skill | 命令 | 用途 |
|-------|------|------|
| **自我评估** | `/self-assessment` | 评估您在所有 10 个功能领域的整体熟练程度。选择快速（2 分钟）或深度（5 分钟）模式，获取个性化技能档案和学习路径。 |
| **课程测验** | `/lesson-quiz [lesson]` | 用 10 个问题测试您对特定课程的理解。可在课前（预测试）、课中（进度检查）或课后（掌握验证）使用。 |

**示例：**
```
/self-assessment                  # 找到您的整体级别
/lesson-quiz hooks                # 关于课程 06：Hooks 的测验
/lesson-quiz 03                   # 关于课程 03：Skills 的测验
/lesson-quiz advanced-features    # 关于课程 09 的测验
```

---

## ⚡ 快速启动路径

### 如果您只有 15 分钟
**目标**：获得您的首个成功

1. 复制一个 slash command：`cp 01-slash-commands/optimize.md .claude/commands/`
2. 在 Claude Code 中尝试：`/optimize`
3. 阅读：[01-slash-commands/README.md](01-slash-commands/README.md)

**成果**：您将拥有一个可工作的 slash command 并理解基础知识

---

### 如果您有 1 小时
**目标**：设置必备的生产力工具

1. **Slash commands**（15 分钟）：复制并测试 `/optimize` 和 `/pr`
2. **项目内存**（15 分钟）：创建 CLAUDE.md 包含您的项目标准
3. **安装 skill**（15 分钟）：设置 code-review skill
4. **一起尝试**（15 分钟）：看它们如何协同工作

**成果**：通过命令、内存和自动 skills 获得基础生产力提升

---

### 如果您有一个周末
**目标**：熟练掌握大多数功能

**周六上午**（3 小时）：
- 完成里程碑 1A：Slash Commands + Memory
- 完成里程碑 1B：Checkpoints + CLI 基础

**周六下午**（3 小时）：
- 完成里程碑 2A：Skills + Hooks
- 完成里程碑 2B：MCP + Subagents

**周日**（4 小时）：
- 完成里程碑 3A：高级功能
- 完成里程碑 3B：Plugins + CLI 精通 + CI/CD
- 为您的团队构建自定义插件

**成果**：您将成为一名 Claude Code 高级用户，准备好培训他人并自动化复杂工作流

---

## 💡 学习技巧

### ✅ 应该做

- **先做测验**找到您的起点
- **为每个里程碑完成实践练习**
- **从简单开始**逐渐增加复杂度
- **在进入下一个功能前测试每个功能**
- **记录哪些对您的工作流有效**
- **学习高级主题时回顾前面的概念**
- **使用 checkpoints 安全实验**
- **与您的团队分享知识**

### ❌ 不应该做

- **跳到更高级别时跳过前提条件检查**
- **试图一次学完所有内容** — 会让人应接不暇
- **不理解就复制配置** — 您将不知道如何调试
- **忘记测试** — 始终验证功能是否有效
- **匆忙完成里程碑** — 花时间理解
- **忽略文档** — 每个 README 都有 valuable 细节
- **孤立工作** — 与队友讨论

---

## 🎓 学习风格

### 视觉学习者
- 研究每个 README 中的 mermaid 图
- 观看命令执行流程
- 绘制您自己的工作流图
- 使用上面的可视化学习路径

### 实践学习者
- 完成每个实践练习
- 尝试变体
- 破坏并修复它们（使用 checkpoints！）
- 创建您自己的示例

### 阅读学习者
- 仔细阅读每个 README
- 学习代码示例
- 查看比较表
- 阅读资源中链接的博客文章

### 社交学习者
- 设置结对编程会话
- 向队友教授概念
- 加入 Claude Code 社区讨论
- 分享您的自定义配置

---

## 📈 进度跟踪

使用这些清单按级别跟踪您的进度。随时运行 `/self-assessment` 获取更新的技能档案，或在每个教程后运行 `/lesson-quiz [lesson]` 验证您的理解。

### 🟢 级别 1：入门
- [ ] 完成 [01-slash-commands](01-slash-commands/)
- [ ] 完成 [02-memory](02-memory/)
- [ ] 创建首个自定义 slash command
- [ ] 设置项目内存
- [ ] **达成里程碑 1A**
- [ ] 完成 [08-checkpoints](08-checkpoints/)
- [ ] 完成 [10-cli](10-cli/) 基础
- [ ] 创建并恢复到 checkpoint
- [ ] 使用过交互模式和 print 模式
- [ ] **达成里程碑 1B**

### 🔵 级别 2：中级
- [ ] 完成 [03-skills](03-skills/)
- [ ] 完成 [06-hooks](06-hooks/)
- [ ] 安装首个 skill
- [ ] 设置 PreToolUse hook
- [ ] **达成里程碑 2A**
- [ ] 完成 [05-mcp](05-mcp/)
- [ ] 完成 [04-subagents](04-subagents/)
- [ ] 连接 GitHub MCP
- [ ] 创建自定义 subagent
- [ ] 在工作流中结合集成
- [ ] **达成里程碑 2B**

### 🔴 级别 3：高级
- [ ] 完成 [09-advanced-features](09-advanced-features/)
- [ ] 成功使用 planning mode
- [ ] 配置权限模式（6 种模式包括 auto）
- [ ] 使用带安全分类器的 auto mode
- [ ] 使用 extended thinking 切换
- [ ] 探索过 Channels 和 Voice Dictation
- [ ] **达成里程碑 3A**
- [ ] 完成 [07-plugins](07-plugins/)
- [ ] 完成 [10-cli](10-cli/) 高级用法
- [ ] 设置 print mode（`claude -p`）CI/CD
- [ ] 创建用于自动化的 JSON 输出
- [ ] 将 Claude 集成到 CI/CD 流水线
- [ ] 创建团队插件
- [ ] **达成里程碑 3B**

---

## 🆘 常见学习挑战

### 挑战 1："一次太多概念"
**解决方案**：一次专注于一个里程碑。在继续之前完成所有练习。

### 挑战 2："不知道何时使用哪个功能"
**解决方案**：参考主 README 中的[用例矩阵](#use-case-matrix)。

### 挑战 3："配置不工作"
**解决方案**：检查[故障排除](#troubleshooting)部分并验证文件位置。

### 挑战 4："概念似乎重叠"
**解决方案**：查看[功能比较](#feature-comparison)表以理解差异。

### 挑战 5："很难记住所有内容"
**解决方案**：创建您自己的 cheat sheet。使用 checkpoints 安全实验。

### 挑战 6："我有经验但不知道从哪里开始"
**解决方案**：进行上面的[自我评估测验](#-find-your-level)。跳到您的级别并使用前提条件检查来识别任何缺口。

---

## 🎯 完成后下一步是什么？

完成所有里程碑后：

1. **创建团队文档** — 记录您团队的 Claude Code 设置
2. **构建自定义插件** — 打包您团队的工作流
3. **探索远程控制** — 通过外部工具编程控制 Claude Code 会话
4. **尝试 Web 会话** — 通过基于浏览器的界面使用 Claude Code 进行远程开发
5. **使用桌面应用** — 通过原生桌面应用访问 Claude Code 功能
6. **使用 Auto Mode** — 让 Claude 通过后台安全分类器自主工作
7. **利用 Auto Memory** — 让 Claude 随着时间自动学习您的偏好
8. **设置 Agent Teams** — 在复杂、多方面的任务上协调多个 agents
9. **使用 Channels** — 跨结构化多会话工作流组织工作
10. **尝试 Voice Dictation** — 使用免提语音输入与 Claude Code 交互
11. **使用计划任务** — 使用 `/loop` 和 cron 工具自动化重复检查
12. **贡献示例** — 与社区分享
13. **指导他人** — 帮助队友学习
14. **优化工作流** — 根据使用情况持续改进
15. **保持更新** — 关注 Claude Code 发布和新功能

---

## 📚 额外资源

### 官方文档
- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [Anthropic 文档](https://docs.anthropic.com)
- [MCP 协议规范](https://modelcontextprotocol.io)

### 博客文章
- [发现 Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

### 社区
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [MCP Servers 仓库](https://github.com/modelcontextprotocol/servers)

---

## 💬 反馈与支持

- **发现问题？** 在仓库中创建 issue
- **有建议？** 提交 pull request
- **需要帮助？** 查看文档或询问社区

---

**最后更新**：2026 年 3 月
**维护者**：Claude How-To 贡献者
**许可证**：教育目的，免费使用和改编

---

> **中文适配说明**：本文档由 [claude-howto](https://github.com/BuaaJoseph/claude-howto) 翻译而来，保留了英文技术术语（如 slash commands、skills、hooks、MCP 等）。原文采用 MIT 许可证。

---

[← 返回主 README](README.md)