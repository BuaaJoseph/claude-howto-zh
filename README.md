# Claude How To 中文指南

> 📢 **中文适配说明**
> 
> 本项目是基于 [luongnv89/claude-howto](https://github.com/luongnv89/claude-howto) 的中文翻译版本，针对中文用户做了本地化适配。
> 
> 原文项目采用 MIT 许可证，允许自由使用和分发。

---

**许可证**: MIT License - Copyright (c) 2024 luongnv89

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![版本](https://img.shields.io/badge/version-2.2.0-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

# 一个周末掌握 Claude Code

从只会输入 `claude` 到熟练编排 agents、hooks、skills 和 MCP 服务器——通过可视化教程、可直接复制的模板和循序渐进的学习路径。

**[15 分钟入门](#-15-minutes-get-started)** | **[找到你的起点](#-not-sure-where-to-start)** | **[浏览功能目录](CATALOG.md)**

---

> 📢 **中文适配说明**
> 
> 本项目是基于 [luongnv89/claude-howto](https://github.com/luongnv89/claude-howto) 的中文翻译版本，针对中文用户做了本地化适配。
> 
> 原文项目采用 MIT 许可证，允许自由使用和分发。

---

## 目录

- [问题](#the-problem)
- [Claude How To 如何解决](#how-claude-how-to-fixes-this)
- [工作原理](#how-it-works)
- [不确定从哪里开始？](#not-sure-where-to-start)
- [15 分钟入门](#-15-minutes-get-started)
- [你能用它做什么？](#what-can-you-build-with-this)
- [常见问题](#faq)
- [贡献指南](#contributing)
- [许可证](#license)

---

## 问题

你安装了 Claude Code，运行了几个 prompt。然后呢？

- **官方文档只描述功能，不教你如何组合使用。** 你知道有 slash 命令，但不知道如何把它们和 hooks、memory、subagents 串联成真正省时的工作流。
- **没有清晰的学习路径。** 应该先学 MCP 还是 hooks？先学 Skills 还是 subagents？你最终只能泛泛浏览，什么都学不精。
- **例子太简单。** 一个 "hello world" 的 slash 命令没法帮你构建一个生产级的代码审查 pipeline——需要用到 memory、委托给专业 agents、自动运行安全扫描。

你把 Claude Code 90% 的能力闲置了——而且你不知道自己不知道什么。

---

## Claude How To 如何解决

这不是又一份功能文档。它是一个**结构化、可视化、以例子驱动的指南**，教你在真实项目中立即使用每一个 Claude Code 功能。

| | 官方文档 | 本指南 |
|--|---------------|------------|
| **形式** | 参考文档 | 带 Mermaid 图的可视化教程 |
| **深度** | 功能描述 | 底层原理讲解 |
| **例子** | 基础代码片段 | 生产级模板，可直接复制使用 |
| **结构** | 按功能分类 | 渐进式学习路径（入门到高级） |
| **上手** | 自行摸索 | 带时间估计的指导路线 |
| **自测** | 无 | 交互式测验，找出问题并定制学习路径 |

### 你将获得：

- **10 个教程模块**——涵盖所有 Claude Code 功能，从 slash 命令到自定义 agent 团队
- **即用型配置**——slash 命令、CLAUDE.md 模板、hook 脚本、MCP 配置、subagent 定义和完整插件包
- **Mermaid 图表**——展示每个功能的内部原理，让你理解 *为什么*，而不仅是 *怎么做*
- **指导性学习路径**——11-13 小时从入门到精通
- **内置自测**——直接在 Claude Code 中运行 `/self-assessment` 或 `/lesson-quiz` 找出知识盲区

**[开始学习路径 ->](LEARNING-ROADMAP.md)**

---

## 工作原理

### 1. 定位你的水平

做[自测题](LEARNING-ROADMAP.md#-find-your-level)或在 Claude Code 中运行 `/self-assessment`，根据你已掌握的知识获得定制化学习路线。

### 2. 跟随指导路径

按顺序完成 10 个模块——每个都建立在前一个的基础上。学习时直接把模板复制到你的项目中。

### 3. 将功能组合成工作流

真正的力量在于组合功能。学习将 slash commands + memory + subagents + hooks 接线成自动化 pipeline，处理代码审查、部署和文档生成。

### 4. 检验你的理解

每个模块后运行 `/lesson-quiz [topic]`。测验会指出你遗漏的内容，让你快速填补空白。

**[15 分钟入门](#-15-minutes-get-started)**

---

## 被 5,900+ 开发者验证

- **5,900+ GitHub stars**——来自每天使用 Claude Code 的开发者
- **690+ forks**——团队正在适配用于自己的工作流
- **积极维护**——与每个 Claude Code 版本同步（最新：v2.2.0，2026 年 3 月）
- **社区驱动**——开发者贡献的真实配置

[![Star 历史图表](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不确定从哪里开始？

做自测题或选择你的水平：

| 水平 | 你能... | 从这里开始 | 时间 |
|-------|-----------|------------|------|
| **入门** | 启动 Claude Code 并聊天 | [Slash 命令](01-slash-commands/) | ~2.5 小时 |
| **中级** | 使用 CLAUDE.md 和自定义命令 | [Skills](03-skills/) | ~3.5 小时 |
| **高级** | 配置 MCP 服务器和 hooks | [高级功能](09-advanced-features/) | ~5 小时 |

**完整学习路径（10 个模块）：**

| 序号 | 模块 | 水平 | 时间 |
|-------|--------|-------|------|
| 1 | [Slash 命令](01-slash-commands/) | 入门 | 30 分钟 |
| 2 | [Memory](02-memory/) | 入门+ | 45 分钟 |
| 3 | [Checkpoints](08-checkpoints/) | 中级 | 45 分钟 |
| 4 | [CLI 基础](10-cli/) | 入门+ | 30 分钟 |
| 5 | [Skills](03-skills/) | 中级 | 1 小时 |
| 6 | [Hooks](06-hooks/) | 中级 | 1 小时 |
| 7 | [MCP](05-mcp/) | 中级+ | 1 小时 |
| 8 | [Subagents](04-subagents/) | 中级+ | 1.5 小时 |
| 9 | [高级功能](09-advanced-features/) | 高级 | 2-3 小时 |
| 10 | [Plugins](07-plugins/) | 高级 | 2 小时 |

**[完整学习路线图 ->](LEARNING-ROADMAP.md)**

---

## 15 分钟入门

```bash
# 1. 克隆本指南
git clone https://github.com/BuaaJoseph/claude-howto-zh.git
cd claude-howto-zh

# 2. 复制你的第一个 slash 命令
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 试试看——在 Claude Code 中输入：
# /optimize

# 4. 还想学更多？设置项目 memory：
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 安装一个 skill：
cp -r 03-skills/code-review ~/.claude/skills/
```

想要完整配置？这是**1 小时速成配置**：

```bash
# Slash 命令（15 分钟）
cp 01-slash-commands/*.md .claude/commands/

# 项目 memory（15 分钟）
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 安装一个 skill（15 分钟）
cp -r 03-skills/code-review ~/.claude/skills/

# 周末目标：添加 hooks、subagents、MCP 和插件
# 按照学习路径指引进行设置
```

**[查看完整安装参考](#installation-quick-reference)**

---

## 你能用它做什么？

| 使用场景 | 需要组合的功能 |
|----------|------------------------|
| **自动化代码审查** | Slash Commands + Subagents + Memory + MCP |
| **团队入职引导** | Memory + Slash Commands + Plugins |
| **CI/CD 自动化** | CLI Reference + Hooks + 后台任务 |
| **文档生成** | Skills + Subagents + Plugins |
| **安全审计** | Subagents + Skills + Hooks（只读模式）|
| **DevOps 流水线** | Plugins + MCP + Hooks + 后台任务 |
| **复杂重构** | Checkpoints + 规划模式 + Hooks |

---

## 常见问题

**这是免费的吗？**
是的。MIT 许可证，永久免费。个人项目、工作、团队都可以用——只需包含许可证声明。

**有维护吗？**
积极维护。指南与每个 Claude Code 版本同步。当前版本：v2.2.0（2026 年 3 月），兼容 Claude Code 2.1+。

**这和官方文档有什么区别？**
官方文档是功能参考。本指南是带图表、生产级模板和渐进式学习路径的教程。两者互补——从这里开始学习，需要具体细节时查阅文档。

**全部学完要多久？**
完整路径 11-13 小时。但 15 分钟就能获得即时价值——只需复制一个 slash 命令模板并尝试。

**可以配合 Claude Sonnet / Haiku / Opus 使用吗？**
可以。所有模板都支持 Claude Sonnet 4.6、Claude Opus 4.6 和 Claude Haiku 4.5。

**可以贡献吗？**
当然可以。详见 [CONTRIBUTING.md](CONTRIBUTING.md)。欢迎新例子、bug 修复、文档改进和社区模板。

**可以离线阅读吗？**
可以。运行 `uv run scripts/build_epub.py` 生成包含所有内容和渲染图表的 EPUB 电子书。

---

## 现在开始掌握 Claude Code

你已安装了 Claude Code。你与 10 倍生产力之间的距离，只是知道如何使用它。本指南为你提供结构化路径、可视化解释和即用型模板。

MIT 许可证。永久免费。克隆它，fork 它，让它成为你的。

**[开始学习路径 ->](LEARNING-ROADMAP.md)** | **[浏览功能目录](CATALOG.md)** | **[15 分钟入门](#-15-minutes-get-started)**

---

<details>
<summary>快速导航 — 所有功能</summary>

| 功能 | 描述 | 文件夹 |
|---------|-------------|--------|
| **功能目录** | 完整参考和安装命令 | [CATALOG.md](CATALOG.md) |
| **Slash 命令** | 用户触发的快捷方式 | [01-slash-commands/](01-slash-commands/) |
| **Memory** | 跨会话持久化上下文 | [02-memory/](02-memory/) |
| **Skills** | 可复用能力 | [03-skills/](03-skills/) |
| **Subagents** | 专业 AI 助手 | [04-subagents/](04-subagents/) |
| **MCP 协议** | 外部工具访问 | [05-mcp/](05-mcp/) |
| **Hooks** | 事件驱动自动化 | [06-hooks/](06-hooks/) |
| **Plugins** | 功能捆绑包 | [07-plugins/](07-plugins/) |
| **Checkpoints** | 会话快照和回退 | [08-checkpoints/](08-checkpoints/) |
| **高级功能** | 规划、思考、后台任务 | [09-advanced-features/](09-advanced-features/) |
| **CLI 参考** | 命令、标志和选项 | [10-cli/](10-cli/) |
| **博客文章** | 真实使用例子 | [博客文章](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能对比</summary>

| 功能 | 调用方式 | 持久化 | 最佳用途 |
|---------|-----------|------------|----------|
| **Slash 命令** | 手动（`/cmd`）| 仅当前会话 | 快速快捷方式 |
| **Memory** | 自动加载 | 跨会话 | 长期学习 |
| **Skills** | 自动触发 | 文件系统 | 自动化工作流 |
| **Subagents** | 自动委托 | 隔离上下文 | 任务分发 |
| **MCP 协议** | 自动查询 | 实时 | 实时数据访问 |
| **Hooks** | 事件触发 | 配置 | 自动化和验证 |
| **Plugins** | 单命令 | 全部功能 | 完整解决方案 |
| **Checkpoints** | 手动/自动 | 基于会话 | 安全实验 |
| **规划模式** | 手动/自动 | 规划阶段 | 复杂实现 |
| **后台任务** | 手动 | 任务期间 | 长时间运行操作 |
| **CLI 参考** | 终端命令 | 会话/脚本 | 自动化和脚本 |

</details>

<details>
<summary>安装快速参考</summary>

```bash
# Slash 命令
cp 01-slash-commands/*.md .claude/commands/

# Memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Skills
cp -r 03-skills/code-review ~/.claude/skills/

# Subagents
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Plugins
/plugin install pr-review

# Checkpoints（自动启用，可在设置中配置）
# 详见 08-checkpoints/README.md

# 高级功能（在设置中配置）
# 详见 09-advanced-features/config-examples.json

# CLI 参考（无需安装）
# 详见 10-cli/README.md 使用示例
```

</details>

<details>
<summary>01. Slash 命令</summary>

**位置**: [01-slash-commands/](01-slash-commands/)

**是什么**: 存储为 Markdown 文件的用户触发快捷方式

**例子**:
- `optimize.md` - 代码优化分析
- `pr.md` - Pull request 准备
- `generate-api-docs.md` - API 文档生成器

**安装**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**使用**:
```
/optimize
/pr
/generate-api-docs
```

**了解更多**: [发现 Claude Code Slash 命令](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. Memory</summary>

**位置**: [02-memory/](02-memory/)

**是什么**: 跨会话持久化上下文

**例子**:
- `project-CLAUDE.md` - 团队级项目标准
- `directory-api-CLAUDE.md` - 目录级规则
- `personal-CLAUDE.md` - 个人偏好

**安装**:
```bash
# 项目 memory
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# 目录 memory
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 个人 memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**使用**: 由 Claude 自动加载

</details>

<details>
<summary>03. Skills</summary>

**位置**: [03-skills/](03-skills/)

**是什么**: 带指令和脚本的可复用、自动触发能力

**例子**:
- `code-review/` - 带脚本的全面代码审查
- `brand-voice/` - 品牌调性一致性检查
- `doc-generator/` - API 文档生成器

**安装**:
```bash
# 个人 skills
cp -r 03-skills/code-review ~/.claude/skills/

# 项目 skills
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**使用**: 相关时自动触发

</details>

<details>
<summary>04. Subagents</summary>

**位置**: [04-subagents/](04-subagents/)

**是什么**: 带隔离上下文和自定义提示的专业 AI 助手

**例子**:
- `code-reviewer.md` - 全面的代码质量分析
- `test-engineer.md` - 测试策略和覆盖率
- `documentation-writer.md` - 技术文档
- `secure-reviewer.md` - 安全审查（只读）
- `implementation-agent.md` - 完整功能实现

**安装**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**使用**: 由主 agent 自动委托

</details>

<details>
<summary>05. MCP 协议</summary>

**位置**: [05-mcp/](05-mcp/)

**是什么**: Model Context Protocol，用于访问外部工具和 API

**例子**:
- `github-mcp.json` - GitHub 集成
- `database-mcp.json` - 数据库查询
- `filesystem-mcp.json` - 文件操作
- `multi-mcp.json` - 多个 MCP 服务器

**安装**:
```bash
# 设置环境变量
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 通过 CLI 添加 MCP 服务器
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 或手动添加到项目 .mcp.json（详见 05-mcp/）
```

**使用**: 配置后 MCP 工具自动对 Claude 可用

</details>

<details>
<summary>06. Hooks</summary>

**位置**: [06-hooks/](06-hooks/)

**是什么**: 响应 Claude Code 事件自动执行的 shell 脚本

**例子**:
- `format-code.sh` - 写入前自动格式化代码
- `pre-commit.sh` - 提交前运行测试
- `security-scan.sh` - 扫描安全问题
- `log-bash.sh` - 记录所有 bash 命令
- `validate-prompt.sh` - 验证用户提示
- `notify-team.sh` - 事件发生时发送通知

**安装**:
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

在 `~/.claude/settings.json` 中配置 hooks：
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```

**使用**: Hooks 在事件发生时自动执行

**Hook 类型**（4 种，25 个事件）：
- **Tool Hooks**: `PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`
- **Session Hooks**: `SessionStart`、`SessionEnd`、`Stop`、`StopFailure`、`SubagentStart`、`SubagentStop`
- **Task Hooks**: `UserPromptSubmit`、`TaskCompleted`、`TaskCreated`、`TeammateIdle`
- **Lifecycle Hooks**: `ConfigChange`、`CwdChanged`、`FileChanged`、`PreCompact`、`PostCompact`、`WorktreeCreate`、`WorktreeRemove`、`Notification`、`InstructionsLoaded`、`Elicitation`、`ElicitationResult`

</details>

<details>
<summary>07. Plugins</summary>

**位置**: [07-plugins/](07-plugins/)

**是什么**: 命令、agents、MCP 和 hooks 的捆绑集合

**例子**:
- `pr-review/` - 完整 PR 审查工作流
- `devops-automation/` - 部署和监控
- `documentation/` - 文档生成

**安装**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**使用**: 使用捆绑的 slash 命令和功能

</details>

<details>
<summary>08. Checkpoints 和 Rewind</summary>

**位置**: [08-checkpoints/](08-checkpoints/)

**是什么**: 保存会话状态并回退到之前的点以探索不同方法

**核心概念**:
- **Checkpoint**: 会话状态的快照
- **Rewind**: 返回之前的 checkpoint
- **Branch Point**: 从同一 checkpoint 探索多种方法

**使用**:
```
# Checkpoints 在每个用户 prompt 时自动创建
# 要回退，按两次 Esc 或使用：
/rewind

# 然后选择五个选项：
# 1. 恢复代码和会话
# 2. 恢复会话
# 3. 恢复代码
# 4. 从这里总结
# 5. 算了
```

**使用场景**:
- 尝试不同的实现方法
- 从错误中恢复
- 安全实验
- 比较替代方案
- A/B 测试不同设计

</details>

<details>
<summary>09. 高级功能</summary>

**位置**: [09-advanced-features/](09-advanced-features/)

**是什么**: 用于复杂工作流和自动化的进阶能力

**包含**:
- **规划模式** — 编码前创建详细实现计划
- **扩展思考** — 复杂问题的深度推理（用 `Alt+T` / `Option+T` 切换）
- **后台任务** — 无阻塞运行长时间操作
- **权限模式** — `default`、`acceptEdits`、`plan`、`dontAsk`、`bypassPermissions`
- **无头模式** — 在 CI/CD 中运行 Claude Code：`claude -p "Run tests and generate report"`
- **会话管理** — `/resume`、`/rename`、`/fork`、`claude -c`、`claude -r`
- **配置** — 在 `~/.claude/settings.json` 中自定义行为

详见 [config-examples.json](09-advanced-features/config-examples.json) 的完整配置。

</details>

<details>
<summary>10. CLI 参考</summary>

**位置**: [10-cli/](10-cli/)

**是什么**: Claude Code 的完整命令行参考

**快速示例**:
```bash
# 交互模式
claude "explain this project"

# 打印模式（非交互）
claude -p "review this code"

# 处理文件内容
cat error.log | claude -p "explain this error"

# 脚本的 JSON 输出
claude -p --output-format json "list functions"

# 恢复会话
claude -r "feature-auth" "continue implementation"
```

**使用场景**: CI/CD 流水线集成、脚本自动化、批量处理、多会话工作流、自定义 agent 配置

</details>

<details>
<summary>示例工作流</summary>

### 完整代码审查工作流

```markdown
# 使用: Slash Commands + Subagents + Memory + MCP

用户: /review-pr

Claude:
1. 加载项目 memory（编码标准）
2. 通过 GitHub MCP 获取 PR
3. 委托给 code-reviewer subagent
4. 委托给 test-engineer subagent
5. 综合发现
6. 提供全面审查
```

### 自动化文档

```markdown
# 使用: Skills + Subagents + Memory

用户: "生成 auth 模块的 API 文档"

Claude:
1. 加载项目 memory（文档标准）
2. 检测到文档生成请求
3. 自动调用 doc-generator skill
4. 委托给 api-documenter subagent
5. 创建带示例的完整文档
```

### DevOps 部署

```markdown
# 使用: Plugins + MCP + Hooks

用户: /deploy production

Claude:
1. 运行预部署 hook（验证环境）
2. 委托给部署专家 subagent
3. 通过 Kubernetes MCP 执行部署
4. 监控进度
5. 运行后部署 hook（健康检查）
6. 报告状态
```

</details>

<details>
<summary>目录结构</summary>

```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md (本文件)
```

</details>

<details>
<summary>最佳实践</summary>

### 应该做
- 从简单的 slash 命令开始
- 逐步添加功能
- 用 memory 记录团队标准
- 先在本地测试配置
- 记录自定义实现
- 版本控制项目配置
- 与团队分享插件

### 不应该做
- 不要创建冗余功能
- 不要硬编码凭证
- 不要跳过文档
- 不要把简单任务复杂化
- 不要忽视安全最佳实践
- 不要提交敏感数据

</details>

<details>
<summary>故障排除</summary>

### 功能不加载
1. 检查文件位置和命名
2. 验证 YAML frontmatter 语法
3. 检查文件权限
4. 检查 Claude Code 版本兼容性

### MCP 连接失败
1. 验证环境变量
2. 检查 MCP 服务器安装
3. 测试凭证
4. 检查网络连接

### Subagent 不委托
1. 检查工具权限
2. 验证 agent 描述清晰度
3. 检查任务复杂度
4. 独立测试 agent

</details>

<details>
<summary>测试</summary>

本项目包含全面的自动化测试：

- **单元测试**: 使用 pytest 的 Python 测试（Python 3.10、3.11、3.12）
- **代码质量**: 使用 Ruff 的 lint 和格式化
- **安全**: 使用 Bandit 的漏洞扫描
- **类型检查**: 使用 mypy 的静态类型分析
- **构建验证**: EPUB 生成测试
- **覆盖率跟踪**: Codecov 集成

```bash
# 安装开发依赖
uv pip install -r requirements-dev.txt

# 运行所有单元测试
pytest scripts/tests/ -v

# 带覆盖率报告运行测试
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 运行代码质量检查
ruff check scripts/
ruff format --check scripts/

# 运行安全扫描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 运行类型检查
mypy scripts/ --ignore-missing-imports
```

测试在每次推送到 `main`/`develop` 和每个 PR 到 `main` 时自动运行。详见 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 生成</summary>

想离线阅读本指南？生成 EPUB 电子书：

```bash
uv run scripts/build_epub.py
```

这会创建 `claude-howto-guide.epub`，包含所有内容和渲染的 Mermaid 图表。

详见 [scripts/README.md](scripts/README.md) 的更多选项。

</details>

<details>
<summary>贡献</summary>

发现问题或想贡献例子？我们非常欢迎你的帮助！

**请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详细指南：**
- 贡献类型（例子、文档、功能、bug、反馈）
- 如何设置开发环境
- 目录结构以及如何添加内容
- 写作指南和最佳实践
- 提交和 PR 流程

**我们的社区标准：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我们如何对待彼此
- [SECURITY.md](SECURITY.md) - 安全政策和漏洞报告

### 报告安全问题

如果你发现安全漏洞，请负责任地报告：

1. **使用 GitHub 私有漏洞报告**: https://github.com/luongnv89/claude-howto/security/advisories
2. **或阅读** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 了解详细说明
3. **不要** 为安全漏洞开公开 issue

快速开始：
1. Fork 并克隆仓库
2. 创建描述性分支（`add/feature-name`、`fix/bug`、`docs/improvement`）
3. 按照指南做修改
4. 提交带有清晰描述的 pull request

**需要帮助？** 开 issue 或 discussion，我们会引导你完成流程。

</details>

<details>
<summary>更多资源</summary>

- [Claude Code 文档](https://code.claude.com/docs/en/overview)
- [MCP 协议规范](https://modelcontextprotocol.io)
- [Skills 仓库](https://github.com/luongnv89/skills) - 即用型 skills 集合
- [Anthropic  Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny 的 Claude Code 工作流](https://x.com/bcherny/status/2007179832300581177) - Claude Code 创作者分享他的系统化工作流：并行 agents、共享 CLAUDE.md、Plan 模式、slash 命令、subagents 和自主长时间运行会话的验证 hooks。

</details>

---

## 贡献

我们欢迎贡献！请查看我们的[贡献指南](CONTRIBUTING.md)了解如何开始。

## 贡献者

感谢所有为本项目做出贡献的人！

| 贡献者 | PRs |
|-------------|-----|
| [wjhrdy](https://github.com/wjhrdy) | [#1 - add a tool to create an epub](https://github.com/luongnv89/claude-howto/pull/1) |
| [VikalpP](https://github.com/VikalpP) | [#7 - fix(docs): Use tilde fences for nested code blocks in concepts guide](https://github.com/luongnv89/claude-howto/pull/7) |

---

## 许可证

MIT 许可证 - 见 [LICENSE](LICENSE)。免费使用、修改和分发。唯一要求是包含许可证声明。

---

**最后更新**: 2026 年 3 月
**Claude Code 版本**: 2.1+
**兼容模型**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
