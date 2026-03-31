# Claude Code 示例 - 快速参考卡

## 🚀 安装快速命令

### Slash Commands
```bash
# 安装全部
cp 01-slash-commands/*.md .claude/commands/

# 安装特定
cp 01-slash-commands/optimize.md .claude/commands/
```

### Memory
```bash
# 项目内存
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 个人内存
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### Skills
```bash
# 个人 skills
cp -r 03-skills/code-review ~/.claude/skills/

# 项目 skills
cp -r 03-skills/code-review .claude/skills/
```

### Subagents
```bash
# 安装全部
cp 04-subagents/*.md .claude/agents/

# 安装特定
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# 设置凭据
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安装配置（项目作用域）
cp 05-mcp/github-mcp.json .mcp.json

# 或用户作用域：添加到 ~/.claude.json
```

### Hooks
```bash
# 安装 hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在 settings 中配置（~/.claude/settings.json）
```

### Plugins
```bash
# 从示例安装（如果已发布）
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### Checkpoints
```bash
# Checkpoints 在每个用户 prompt 时自动创建
# 要回退，按 Esc 两次或使用：
/rewind

# 然后选择：恢复代码和对话、恢复对话、恢复代码、从这里总结、或算了
```

### 高级功能
```bash
# 在 settings 中配置（.claude/settings.json）
# 参见 09-advanced-features/config-examples.json

# Planning mode
/plan 任务描述

# 权限模式（使用 --permission-mode 标志）
# default        - 风险操作时请求批准
# acceptEdits    - 自动接受文件编辑，其他询问
# plan           - 只读分析，无修改
# dontAsk        - 接受除风险操作外的所有操作
# auto           - 后台分类器自动决定权限
# bypassPermissions - 接受所有操作（需要 --dangerously-skip-permissions）

# 会话管理
/resume                # 恢复之前的对话
/rename "name"         # 命名当前会话
/fork                  # 派生当前会话
claude -c              # 继续最近的对话
claude -r "session"    # 按名称/ID 恢复会话
```

---

## 📋 功能速查表

| 功能 | 安装路径 | 使用 |
|---------|-------------|-------|
| **Slash Commands（55+）** | `.claude/commands/*.md` | `/command-name` |
| **Memory** | `./CLAUDE.md` | 自动加载 |
| **Skills** | `.claude/skills/*/SKILL.md` | 自动调用 |
| **Subagents** | `.claude/agents/*.md` | 自动委托 |
| **MCP** | `.mcp.json`（项目）或 `~/.claude.json`（用户） | `/mcp__server__action` |
| **Hooks（25 事件）** | `~/.claude/hooks/*.sh` | 事件触发（4 种类型） |
| **Plugins** | 通过 `/plugin install` | 全部捆绑 |
| **Checkpoints** | 内置 | `Esc+Esc` 或 `/rewind` |
| **Planning Mode** | 内置 | `/plan <task>` |
| **权限模式（6 种）** | 内置 | `--allowedTools`、`--permission-mode` |
| **Sessions** | 内置 | `/session <command>` |
| **后台任务** | 内置 | 在后台运行 |
| **远程控制** | 内置 | WebSocket API |
| **Web Sessions** | 内置 | `claude web` |
| **Git Worktrees** | 内置 | `/worktree` |
| **Auto Memory** | 内置 | 自动保存到 CLAUDE.md |
| **Task List** | 内置 | `/task list` |
| **捆绑 Skills（5）** | 内置 | `/simplify`、`/loop`、`/claude-api`、`/voice`、`/browse` |

---

## 🎯 常见用例

### 代码审查
```bash
# 方法 1：Slash command
cp 01-slash-commands/optimize.md .claude/commands/
# 使用：/optimize

# 方法 2：Subagent
cp 04-subagents/code-reviewer.md .claude/agents/
# 使用：自动委托

# 方法 3：Skill
cp -r 03-skills/code-review ~/.claude/skills/
# 使用：自动调用

# 方法 4：Plugin（最佳）
/plugin install pr-review
# 使用：/review-pr
```

### 文档
```bash
# Slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# Subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# Skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# Plugin（完整解决方案）
/plugin install documentation
```

### DevOps
```bash
# 完整 plugin
/plugin install devops-automation

# 命令：/deploy、/rollback、/status、/incident
```

### 团队标准
```bash
# 项目内存
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 为您的团队编辑
vim CLAUDE.md
```

### 自动化与 Hooks
```bash
# 安装 hooks（25 事件，4 种类型：command、http、prompt、agent）
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 示例：
# - Pre-commit 测试：pre-commit.sh
# - 自动格式化代码：format-code.sh
# - 安全扫描：security-scan.sh

# 用于完全自主工作流的 Auto Mode
claude --enable-auto-mode -p "重构并测试 auth 模块"
# 或使用 Shift+Tab 循环切换模式
```

### 安全重构
```bash
# Checkpoints 在每个 prompt 前自动创建
# 尝试重构
# 如果成功：继续
# 如果失败：按 Esc+Esc 或使用 /rewind 返回
```

### 复杂实现
```bash
# 使用 planning mode
/plan 实现用户认证系统

# Claude 创建详细计划
# 审查并批准
# Claude 系统化实现
```

### CI/CD 集成
```bash
# 以无头模式运行（非交互）
claude -p "运行所有测试并生成报告"

# 带 CI/CD 权限模式
claude -p "运行测试" --permission-mode dontAsk

# 用于完全自主 CI 任务的 Auto Mode
claude --enable-auto-mode -p "运行测试并修复失败"

# 用于自动化的 hooks
# 参见 09-advanced-features/README.md
```

### 学习与实验
```bash
# 使用 plan mode 进行安全分析
claude --permission-mode plan

# 安全实验 - checkpoints 自动创建
# 如果需要回退：按 Esc+Esc 或使用 /rewind
```

### Agent Teams
```bash
# 启用 agent teams
export CLAUDE_AGENT_TEAMS=1

# 或在 settings.json 中
{ "agentTeams": { "enabled": true } }

# 开始时："使用团队方法实现功能 X"
```

### 计划任务
```bash
# 每 5 分钟运行一个命令
/loop 5m /check-status

# 一次性提醒
/loop 30m "提醒我检查部署"
```

---

## 📁 文件位置参考

```
您的项目/
├── .claude/
│   ├── commands/              # Slash commands 放在这里
│   ├── agents/                # Subagents 放在这里
│   ├── skills/                # 项目 skills 放在这里
│   └── settings.json          # 项目设置（hooks 等）
├── .mcp.json                  # MCP 配置（项目作用域）
├── CLAUDE.md                  # 项目内存
└── src/
    └── api/
        └── CLAUDE.md          # 目录特定内存

用户主目录/
├── .claude/
│   ├── commands/              # 个人命令
│   ├── agents/               # 个人 agents
│   ├── skills/               # 个人 skills
│   ├── hooks/                # Hook 脚本
│   ├── settings.json         # 用户设置
│   ├── managed-settings.d/   # 托管设置（企业/组织）
│   └── CLAUDE.md             # 个人内存
└── .claude.json              # 个人 MCP 配置（用户作用域）
```

---

## 🔍 查找示例

### 按类别
- **Slash Commands**：`01-slash-commands/`
- **Memory**：`02-memory/`
- **Skills**：`03-skills/`
- **Subagents**：`04-subagents/`
- **MCP**：`05-mcp/`
- **Hooks**：`06-hooks/`
- **Plugins**：`07-plugins/`
- **Checkpoints**：`08-checkpoints/`
- **高级功能**：`09-advanced-features/`
- **CLI**：`10-cli/`

### 按用例
- **性能**：`01-slash-commands/optimize.md`
- **安全**：`04-subagents/secure-reviewer.md`
- **测试**：`04-subagents/test-engineer.md`
- **文档**：`03-skills/doc-generator/`
- **DevOps**：`07-plugins/devops-automation/`

### 按复杂度
- **简单**：Slash commands
- **中等**：Subagents、Memory
- **高级**：Skills、Hooks
- **完整**：Plugins

---

## 🎓 学习路径

### 第一天
```bash
# 阅读概述
cat README.md

# 安装一个命令
cp 01-slash-commands/optimize.md .claude/commands/

# 尝试它
/optimize
```

### 第二天-第三天
```bash
# 设置内存
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# 安装 subagent
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 第四天-第五天
```bash
# 设置 MCP
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# 尝试 MCP 命令
/mcp__github__list_prs
```

### 第二周
```bash
# 安装 skill
cp -r 03-skills/code-review ~/.claude/skills/

# 让它自动调用
# 只需说："审查这段代码的问题"
```

### 第三周+
```bash
# 安装完整 plugin
/plugin install pr-review

# 使用捆绑功能
/review-pr
/check-security
/check-tests
```

---

## 新功能（2026 年 3 月）

| 功能 | 描述 | 使用 |
|---------|-------------|-------|
| **Auto Mode** | 带后台分类器的完全自主操作 | `--enable-auto-mode` 标志，`Shift+Tab` 循环切换模式 |
| **Channels** | Discord 和 Telegram 集成 | `--channels` 标志，Discord/Telegram bots |
| **Voice Dictation** | 对 Claude 说命令和上下文 | `/voice` 命令 |
| **Hooks（25 事件）** | 扩展的 hook 系统，4 种类型 | command、http、prompt、agent hook 类型 |
| **MCP Elicitation** | MCP servers 可以在运行时请求用户输入 | server 需要澄清时自动提示 |
| **WebSocket MCP** | MCP 连接的 WebSocket 传输 | 在 `.mcp.json` 中配置 `ws://` URLs |
| **Plugin LSP** | plugins 的语言服务器协议支持 | `userConfig`、`${CLAUDE_PLUGIN_DATA}` 变量 |
| **远程控制** | 通过 WebSocket API 控制 Claude Code | `claude --remote` 用于外部集成 |
| **Web Sessions** | 基于浏览器的 Claude Code 界面 | `claude web` 启动 |
| **桌面应用** | 原生桌面应用 | 从 claude.ai/download 下载 |
| **Task List** | 管理后台任务 | `/task list`、`/task status <id>` |
| **Auto Memory** | 从对话自动保存内存 | Claude 自动将关键上下文保存到 CLAUDE.md |
| **Git Worktrees** | 用于并行开发的隔离工作区 | `/worktree` 创建隔离工作区 |
| **模型选择** | 在 Sonnet 4.6 和 Opus 4.6 之间切换 | `/model` 或 `--model` 标志 |
| **Agent Teams** | 协调多个 agents 处理任务 | 使用 `CLAUDE_AGENT_TEAMS=1` 环境变量启用 |
| **计划任务** | 使用 `/loop` 的重复任务 | `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 集成** | 浏览器自动化 | `--chrome` 标志或 `/chrome` 命令 |
| **键盘自定义** | 自定义键位 | `/keybindings` 命令 |

---

## 提示与技巧

### 自定义
- 从示例开始 그대로 使用
- 根据您的需求修改
- 与团队分享前测试
- 对您的配置进行版本控制

### 最佳实践
- 使用内存进行团队标准
- 使用 plugins 实现完整工作流
- 使用 subagents 处理复杂任务
- 使用 slash commands 处理快速任务

### 故障排除
```bash
# 检查文件位置
ls -la .claude/commands/
ls -la .claude/agents/

# 验证 YAML 语法
head -20 .claude/agents/code-reviewer.md

# 测试 MCP 连接
echo $GITHUB_TOKEN
```

---

## 📊 功能矩阵

| 需求 | 使用这个 | 示例 |
|------|----------|---------|
| 快速快捷方式 | Slash Command（55+） | `01-slash-commands/optimize.md` |
| 团队标准 | Memory | `02-memory/project-CLAUDE.md` |
| 自动工作流 | Skill | `03-skills/code-review/` |
| 专用任务 | Subagent | `04-subagents/code-reviewer.md` |
| 外部数据 | MCP（+ Elicitation、WebSocket） | `05-mcp/github-mcp.json` |
| 事件自动化 | Hook（25 事件，4 种类型） | `06-hooks/pre-commit.sh` |
| 完整解决方案 | Plugin（+ LSP 支持） | `07-plugins/pr-review/` |
| 安全实验 | Checkpoint | `08-checkpoints/checkpoint-examples.md` |
| 完全自主 | Auto Mode | `--enable-auto-mode` 或 `Shift+Tab` |
| 聊天集成 | Channels | `--channels`（Discord、Telegram） |
| CI/CD 流水线 | CLI | `10-cli/README.md` |

---

## 🔗 快速链接

- **主指南**：`README.md`
- **完整索引**：`INDEX.md`
- **概要**：`EXAMPLES_SUMMARY.md`
- **原始指南**：`claude_concepts_guide.md`

---

## 📞 常见问题

**Q：我应该使用哪个？**
A：从 slash commands 开始，根据需要添加功能。

**Q：我可以混合使用功能吗？**
A：可以！它们可以协同工作。Memory + Commands + MCP = 强大的组合。

**Q：我如何与团队分享？**
A：将 `.claude/` 目录提交到 git。

**Q：密钥怎么办？**
A：使用环境变量，永远不要硬编码。

**Q：我可以修改示例吗？**
A：当然可以！它们是自定义的模板。

---

## ✅ 检查清单

开始使用检查清单：

- [ ] 阅读 `README.md`
- [ ] 安装 1 个 slash command
- [ ] 尝试该命令
- [ ] 创建项目 `CLAUDE.md`
- [ ] 安装 1 个 subagent
- [ ] 设置 1 个 MCP 集成
- [ ] 安装 1 个 skill
- [ ] 尝试一个完整 plugin
- [ ] 根据您的需求自定义
- [ ] 与团队分享

---

**快速开始**：`cat README.md`

**完整索引**：`cat INDEX.md`

**本参考卡**：请随时备用以快速参考！