<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP (Model Context Protocol)

本文件夹包含 MCP 服务器配置和使用方面的综合文档和示例，供 Claude Code 使用。

## 概述

MCP（模型上下文协议）是 Claude 访问外部工具、API 和实时数据源的标准化方式。与 Memory 不同，MCP 提供对变化数据的实时访问。

主要特点：
- 实时访问外部服务
- 实时数据同步
- 可扩展架构
- 安全认证
- 基于工具的交互

## MCP 架构

```mermaid
graph TB
    A ["Claude"]
    B ["MCP 服务器"]
    C ["外部服务"]

    A -->|请求：list_issues| B
    B -->|查询| C
    C -->|数据| B
    B -->|响应| A

    A -->|请求：create_issue| B
    B -->|操作| C
    C -->|结果| B
    B -->|响应| A

        style A fill:#e1f5fe,stroke:#333,color:#333
        style B fill:#f3e5f5,stroke:#333,color:#333
        style C fill:#e8f5e9,stroke:#333,color:#333
```

## MCP 生态系统

```mermaid
graph TB
    A["Claude"] -->|MCP| B["文件系统<br/>MCP 服务器"]
    A -->|MCP| C["GitHub<br/>MCP 服务器"]
    A -->|MCP| D["数据库<br/>MCP 服务器"]
    A -->| MCP | E ["Slack<br/>MCP 服务器"]
    A -->| MCP | F ["Google文档<br/>MCP服务器"]

    B -- > |文件I/O | G ["本地文件"]
    C -- > | API | H ["GitHub Repos"]
    D -- > | Query | I ["PostgreSQL/MySQL"]
    E -- > | Messages | J ["Slack Workspace"]
    F -- > | Docs | K ["Google云端硬盘"]

        style A fill:#e1f5fe,stroke:#333,color:#333
        style B fill:#f3e5f5,stroke:#333,color:#333
    样式C填充： # f3e5f5 ，笔画： # 333 ，颜色： # 333
        style D fill:#f3e5f5,stroke:#333,color:#333
        style E fill:#f3e5f5,stroke:#333,color:#333
        style F fill:#f3e5f5,stroke:#333,color:#333
    样式G填充： # e8f5e9 ，笔画： # 333 ，颜色： # 333
    样式H填充： # e8f5e9 ，笔画： # 333 ，颜色： # 333
        style I fill:#e8f5e9,stroke:#333,color:#333
        style J fill:#e8f5e9,stroke:#333,color:#333
    样式K填充： # e8f5e9 ，笔画： # 333 ，颜色： # 333
```

## MCP 安装方法

Claude Code支持多种MCP服务器连接传输协议：

### HTTP 传输（推荐）

```bash
# 基本 HTTP 连接
claude mcp add --transport http notion https://mcp.notion.com/mcp

# 带认证头的 HTTP
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "认证orization: Bearer your-token"
```

### Stdio Transport (本地)

对于本地运行的MCP服务器：

```bash
# 本地 Node.js server
claude mcp add --transport stdio myserver -- npx @ myorg/mcp-server

# 带环境变量
claude mcp add --transport stdio myserver --env KEY = value -- npx服务器
```

### SSE 传输（已弃用）

Server-Sent Events transport is deprecated in favor of `http` but still supported:

```bash
claude mcp add --transport sse legacy-server https://example.com/sse
```

### WebSocket 传输

用于永久双向连接的WebSocket传输：

```bash
claude mcp add --transport ws realtime-server wss://example.com/mcp
```

### Windows-Specific 否te

在本机Windows （非WSL ）上，对npxcommand使用`cmd/c` ：

```bash
claude mcp add --transport stdio my-server -- cmd/c npx -y @ some/package
```

### OAuth 2.0 认证entication

Claude Code为需要的MCP服务器支持OAuth 2.0。当连接到启用了OAuth的服务器时， Claude Code会处理整个身份验证流程：

```bash
# 连接到启用 OAuth 的 MCP 服务器 （交互式流程）
claude mcp add --transport http my-service https://my-service.example.com/mcp

# 预配置 OAuth 凭据以进行非交互式设置
claude mcp add --transport http my-service https://my-service.example.com/mcp \
  --client-id "your-client-id"\
  --client-secret "your-client-secret"\
  --callback-port 8080
```

| 特性 | 描述 |
|---------|-------------|
| **Interactive OAuth** | 使用 `/mcp` 触发基于浏览器的 OAuth 流程 |
| **Pre-configured OAuth clients** | 内置 OAuth 客户端用于常见服务 like 否tion, Stripe, and others (v2.1.30+) |
| **Pre-configured credentials** | `--client-id`, `--client-secret`, `--callback-port` 标志用于自动化设置 |
| **Token storage** | 令牌被安全存储在您的系统密钥链中 |
| **Step-up auth** | 支持升级认证用于特权操作 |
| **Discovery caching** | OAuth 发现元数据被缓存以加快重新连接 |
| **Metadata override** | `oauth.authServerMetadataUrl` in `.mcp.json` to override 默认 OAuth metadata discovery |

#### 覆盖 OAuth 元数据发现

如果您的MCP服务器在标准OAuth元数据端点（ `/.well-known/oauth-authorization-server` ）上返回错误，但暴露了工作的OIDC端点，您可以告诉Claude Code从特定URL获取OAuth元数据。在服务器配置的`oauth`对象中设置'authServerMetadataUrl' ：

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "URL": "https://mcp.example.com/mcp",
      "oauth": {
        "authServerMetadataUrl": "https://auth.example.com/.well-known/openid-configuration"
      }
    }
  }
}
```

URL 必须使用 `https://`。此选项需要 Claude Code v2.1.64 或更高版本。

### Claude.ai MCP 连接器

在您的Claude.ai帐户中配置的MCP服务器在Claude Code中自动可用。这意味着您通过Claude.ai Web界面设置的任何MCP连接都可以访问，而无需额外配置。

Claude.ai MCP连接器还提供“--print”模式（ v2.1.83+ ） ，支持非交互式和脚本化使用。

要禁用Claude Code中的Claude.ai MCP服务器，请将“ENABLE_CLAUDEAI_MCP_SERVERS”env设置为“FALSE” ：

```bash
ENABLE_CLAUDEAI_MCP_SERVERS = FALSE CLAUDE
```

> * *注意： * *此功能仅适用于使用Claude.ai帐户登录的用户。

## MCP 设置流程

```mermaid
sequenceDiagram
    参与者用户
    参与者Claude扮演Claude Code
    参与者配置为配置文件
    参与者服务作为外部服务

    用户- > > Claude ：类型/mcp
    Claude- > > Claude ：列出可用的MCP服务器
    Claude- > >用户：显示选项
    用户- > > Claude ：选择GitHub MCP
    Claude- > >配置：更新配置
    配置- > > Claude ：激活连接
    Claude- > >服务：测试连接
    服务-- > > Claude ：身份验证成功
    Claude- > >用户： ✅ MCP已连接！
```

## MCP Tool 搜索

当MCP工具描述超过上下文窗口的10 ％时， Claude Code会自动使工具搜索能够有效地选择正确的工具，而不会使模型上下文不堪重负。

| 设置 | 值 | 描述 |
|---------|-------|-------------|
| `ENABLE_TOOL_SEARCH` | `auto` (默认) | 当工具描述超过上下文 10% 时自动启用 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` | Automatically enables at a custom threshold of `N` tools |
| `ENABLE_TOOL_SEARCH` | `true` | 无论工具数量如何始终启用 |
| `ENABLE_TOOL_SEARCH` | `false` | 禁用；发送所有工具描述 |

> * *注意： * *工具搜索需要Sonnet 4或更高版本，或Opus 4或更高版本。工具搜索不支持Haiku模型。

## Dynamic Tool Updates

Claude Code支持MCP “LIST_CHANGED”通知。当MCP服务器动态添加、删除或修改其可用工具时， Claude Code会接收更新并自动调整其工具列表，无需重新连接或重新启动。

## MCP Elicitation

MCP服务器可以通过交互式对话（ v2.1.49+ ）向用户请求结构化输入。这允许MCP服务器在工作流程中询问其他信息-例如，提示确认、从选项列表中选择或填写必填字段-为MCP服务器交互添加交互性。

## Tool 描述 and Instruction Cap

从v2.1.84开始， Claude Code对每个MCP服务器的工具描述和指令强制执行* * 2 KB上限* *。这可以防止单个服务器使用过于冗长的工具定义来消耗过多的上下文，从而减少上下文膨胀并保持交互效率。

## MCP Prompts as Slash commands

MCP服务器可以公开在Claude Code中显示为斜杠command的提示。可以使用命名约定访问提示：

```
/mcp __<server> __<prompt>
```

例如，如果名为“github”的服务器公开了名为“review”的提示，则可以将其作为“/mcp __ github __ review”调用。

## Server Deduplication

当在多个作用域（本地、项目、用户）定义相同的MCP服务器时，本地配置优先。这允许您使用本地自定义无冲突地覆盖项目级或用户级MCP设置。

## 通过 @ 提及时引用 MCP 资源

您可以使用“@”提及语法在提示中直接引用MCP资源：

```
@server-name:protocol://resource/path
```

例如，要引用特定的数据库资源：

```
@database:postgres://mydb/users
```

这使Claude能够获取并内联MCP资源内容，将其作为对话上下文的一部分。

## MCP 范围

MCP配置可以存储在具有不同共享级别的不同范围内：

| 范围 | 位置 | 描述 | 与...共享 | 需要批准 |
|-------|----------|-------------|-------------|------------------|
| **本地** (默认) | `~/.claude.json` (在项目路径下) | 仅当前用户、当前项目私有 （在旧版本中称为 `project`） | 仅您 | 否 |
| **项目** | `.mcp.json` | 提交到 git 仓库 | 团队成员 | 是（首次使用） |
| **用户** | `~/.claude.json` | 跨所有项目可用 （在旧版本中称为 `global`） | 仅您 | 否 |

### Using 项目 范围

将项目特定的MCP配置存储在“.mcp.json”中：

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "URL": "https://api.github.com/mcp"
    }
  }
}
```

团队成员将在首次使用项目MCP时看到批准提示。

## MCP 配置管理

### 添加 MCP 服务器

```bash
# 添加基于 HTTP 的服务器
claude mcp add --transport http github https://api.github.com/mcp

# 添加本地 stdio 服务器
claude mcp add --transport stdio database -- npx @ company/db-server

# 列出所有 MCP 服务器
claude mcp list

# 获取特定服务器详情
claude mcp get github

# 移除 MCP 服务器
claude mcp删除github

# 重置项目特定的批准选择
claude mcp reset-project-choices

# 从 Claude Desktop 导入
claude mcp add-from-claude-desktop
```

## 可用 MCP 服务器表

| MCP 服务器 | 用途 | 常用工具 | 认证 | 实时 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 文件操作 | read, write, delete | OS permissions | ✅ 是 |
| **GitHub** | 仓库管理 | list_prs, create_issue, push | OAuth | ✅ 是 |
| **Slack** | 团队沟通 | send_message, list_channels | Token | ✅ 是 |
| **Database** | SQL 查询 | query, insert, update | Credentials | ✅ 是 |
| **Google Docs** | 文档访问 | read, write, share | OAuth | ✅ 是 |
| **Asana** | 项目 management | create_task, update_status | API Key | ✅ 是 |
| **Stripe** | 支付数据 | list_charges, create_invoice | API Key | ✅ 是 |
| **Memory** | 持久内存 | store, retrieve, delete | 本地 | ❌ 否 |

## 实用示例

### 示例 1：GitHub MCP 配置

* *文件： * * `.mcp.json` （项目根目录）

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

* *可用的GitHub MCP工具： * *

#### Pull Request 管理
- `list_prs` -列出存储库中的所有PR
- `get_pr` -获取PR详细信息，包括diff
- `create_pr` -创建新的公关
- `update_pr` -更新公关描述/标题
- `merge_pr` -将PR合并到主分支
- `review_pr` -添加审阅评论

* *请求示例： * *
```
/mcp __ github __ get_pr 456

# 返回：
标题：添加深色模式支持
作者： @ alice
描述：使用CSS变量实现暗色主题
状态：打开
审核人： @ bob、@ charlie
```

#### 问题管理
- `list_issues` -列出所有问题
- `get_issue` -获取问题详细信息
- `create_issue` -创建新问题
- `close_issue` -关闭问题
- `add_comment` -向问题添加评论

#### 仓库信息
- `get_repo_info` -存储库详细信息
- `list_files` -文件树结构
- `get_file_content` -读取文件内容
- “SEARCH_CODE” -跨代码库搜索

#### 提交操作
- `list_commits` -提交历史记录
- `get_commit` -特定提交详细信息
- `create_commit` -创建新提交

设置
```bash
export GITHUB_TOKEN="your_github_token"
# 或使用 CLI 直接添加：
claude mcp add --transport stdio github -- npx @ modelcontextprotocol/server-github
```

### 配置中的env扩展

MCP配置支持使用回退默认值进行env扩展。`${VAR}'和`$ {VAR: -默认}`语法适用于以下字段： `command`、`args`、`env`、`URL`和`头`。

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "URL": "${API_BASE_URL:-https://api.example.com}/mcp",
      "头": {
        "认证orization": "Bearer ${API_KEY}",
        "X-Custom-Header": "${CUSTOM_HEADER:-默认-value}"
      }
    },
    "local-server": {
      "command": "${MCP_BIN_PATH:-npx}",
      "args": ["${MCP_PACKAGE:-@company/mcp-server}"],
      "env": {
        "DB_URL": "${DATABASE_URL:-postgresql://localhost/dev}"
      }
    }
  }
}
```

变量在运行时展开：
- “${VAR}” -使用env，未设置时出错
- “$ {VAR: -默认}” -使用env，如果未设置，则回退到默认值

### 示例 2：数据库 MCP 设置

配置

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

示例用法

```markdown
用户：获取超过10个订单的所有用户

克劳德：我会查询你的数据库来找到这些信息。

# 使用 MCP 数据库工具：
选择u. *, COUNT (o.id)作为order_count
哪些用户隐藏
左联接订单o ON u.id = o.user_id
GROUP BY u.id
计数(o.id) > 10
ORDER BY ORDER_COUNT DESC

# 结果：
- Alice ： 15个订单
- BOB ： 12个订单
- CHARLIE ： 11个订单
```

设置
```bash
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
# 或使用 CLI 直接添加：
claude mcp add --transport stdio database -- npx @ modelcontextprotocol/server-database
```

### 示例 3：多 MCP 工作流

* *场景：每日报告生成* *

```markdown
# 使用多个 MCP 的每日报告工作流

## 设置
1. GitHub MCP -获取公关指标
2.数据库MCP -查询销售数据
3. Slack MCP - POST报告
4.文件系统MCP -保存报告

## 工作流

### 步骤 1：获取 GitHub 数据
/mcp __ github __ list_prs completed: true last: 7days

输出：
- PR总数： 42
-平均合并时间： 2.3小时
-审核周转时间： 1.1小时

### 步骤 2：查询数据库
选择COUNT (*)作为销售额， SUM (AMOUNT)作为收入
来自订单
WHERE created_at > NOW () -间隔“1天”

输出：
-销售额： 247
-收入： $ 12,450

### 步骤 3：生成报告
将数据合并到HTML报表中

### 步骤 4：保存到文件系统
将report.html写入/reports/

### 步骤 5：发布到 Slack
将摘要发送到#daily-reports频道

最终输出
✅ 报告已生成并发布
📊 本周合并了 47 个 PR
💰 每日销售额为 $12,450
```

设置
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN = "your_slack_token"
# Add each MCP server via the CLI or configure them in .mcp.json
```

### 示例 4：文件系统 MCP 操作

配置

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**可用操作：**

| 操作 | command | 用途 |
|-----------|---------|---------|
| 列出文件 | `ls ~/projects` | 显示目录内容 |
| 读取文件 | `cat src/main.ts` | 读取文件内容 |
| 写入文件 | `create docs/api.md` | 创建新文件 |
| 编辑文件 | `edit src/app.ts` | 修改文件 |
| 搜索 | `grep "async function"` | 在文件中搜索 |
| 删除 | `rm old-file.js` | 删除文件 |

设置
```bash
# Use the CLI to add directly:
claude mcp add --transport stdio filesystem -- npx @ modelcontextprotocol/server-filesystem/home/user/projects
```

## MCP 与 Memory：决策矩阵

```mermaid
graph TD
    A ["需要外部数据？"]
    A -- > |否| B ["使用内存"]
    A -- > | Yes | C [“它经常变化吗？”]
    C -- > |否/很少| B
    C -- > | Yes/Often | D ["使用 MCP"]

    B -->| 存储 | E ["首选项<br/>上下文<br/>历史记录"]
    D -->| 访问 | F ["实时 API<br/>数据库<br/>服务"]

    style A fill: # fff3e0, stroke: # 333, color: # 333
    样式B填充： # e1f5fe ，笔画： # 333 ，颜色： # 333
    样式C填充： # fff3e0 ，笔画： # 333 ，颜色： # 333
        style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill: # e8f5e9, stroke: # 333, color: # 333
    style F fill: # e8f5e9, stroke: # 333, color: # 333
```

## 请求/响应模式

```mermaid
sequenceDiagram
    participant App扮演Claude
    参与者MCP作为MCP服务器
    参与者数据库作为数据库

    App- > > MCP: Request: "SELECT * FROM users WHERE id = 1"
    MCP- > > DB ：执行查询
    DB-- > > MCP ：结果集
    MCP-- > >应用：返回解析数据
    APP- > > APP ：处理结果
    APP- > > APP ：继续任务

    MCP、DB注意事项：实时访问<br/>无缓存
```

## env

在env中存储敏感凭据：

```bash
# ~/.bashrc or ~/.zshrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN = "xoxb-xxxxxxxxxxxxx"
```

然后在MCP配置中引用它们：

```json
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

## Claude as MCP 服务器 (`claude mcp serve`)

Claude Code本身可以充当其他应用程序的MCP服务器。这使外部工具、编辑器和自动化系统能够通过标准MCP协议利用Claude的能力。

```bash
# 在 stdio 上启动 Claude Code 作为 MCP 服务器
claude mcp发球
```

然后，其他应用程序可以像连接任何基于stdio的MCP服务器一样连接到此服务器。例如，要在另一个Claude Code实例中添加Claude Code作为MCP服务器：

```bash
claude mcp add --transport stdio claude-agent -- claude mcp serve
```

这对于构建一个Claude实例协调另一个Claude实例的多Agent工作流程非常有用。

## Managed MCP 配置 (Enterprise)

对于企业部署， IT管理员可以通过“managed-mcp.json”配置文件强制执行MCP服务器策略。此文件提供对组织范围内允许或阻止的MCP服务器的独占控制。

位置位置
- macOS ： `/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux ： `~/.config/ClaudeCode/managed-mcp.json`
- Windows ： `% APPDATA %\ ClaudeCode\ managed-mcp.json`

功能特点
- `allowedMcpServers` --允许的服务器白名单
- `deniedMcpServers` --禁止的服务器阻止列表
-支持按服务器名称、command和URL模式进行匹配
-在用户配置之前实施组织范围的MCP策略
-防止未经授权的服务器连接

配置示例：

```json
{
  "allowedMcpServers": [
    {
      "serverName": "github",
      "serverUrl": "https://api.github.com/mcp"
    },
    {
      "serverName": "company-internal",
      "servercommand": "company-mcp-server"
    }
  ],
  "deniedMcpServers": [
    {
      "serverName": "untrusted-*"
    },
    {
      "serverUrl": "http://*"
    }
  ]
}
```

> * *注意： * *当`allowedMcpServers'和`deniedMcpServers`与服务器匹配时，拒绝规则优先。

## Plugin-Provided MCP 服务器s

插件可以捆绑自己的MCP服务器，使它们在安装插件时自动可用。插件提供的MCP服务器可以通过两种方式定义：

1. * * Standalone `.mcp.json` * * --将`.mcp.json`文件放在插件根目录中
2. * * 'plugin.json`中的内联* * --直接在插件清单中定义MCP服务器

使用“${CLAUDE_PLUGIN_ROOT}”变量引用相对于插件安装目录的路径：

```json
{
  "mcpServers": {
    "plugin-tools": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/dist/mcp-server.js"],
      "env": {
        "CONFIG_PATH": "${CLAUDE_PLUGIN_ROOT}/config.json"
      }
    }
  }
}
```

## Subagent-范围d MCP

MCP服务器可以使用'mcpServers:`键在代理frontmatter内联定义，将它们作用于特定的子代理，而不是整个项目。当代理需要访问工作流中的其他代理不需要的特定MCP服务器时，这非常有用。

```yaml
----
mcpServers ：
  my-tool:
    type: http
    URL: https://my-tool.example.com/mcp
----

您是代理，可以访问my-tool进行专门操作。
```

子代理范围的MCP服务器仅在该代理的执行上下文中可用，不会与父代理或同级代理共享。

## MCP 输出限制

Claude Code对MCP工具输出实施限制，以防止上下文溢出：

| 限制 | 阈值 | 行为 |
|-------|-----------|----------|
| **警告** | 10,000 tokens | 显示输出很大的警告 |
| **默认最大值** | 25,000 tokens | 输出在此限制后被截断 |
| **磁盘持久化** | 50,000 characters | 超过 50K 字符的工具结果持久化到磁盘 |

最大输出限制可通过`MAX_MCP_OUTPUT_TOKENS`env配置：

```bash
# 将最大输出增加到 50,000 tokens
export MAX_MCP_OUTPUT_TOKENS = 50000
```

## Solving 上下文 Bloat with Code Execution

随着MCP采用规模的扩大，使用数百或数千种工具连接到数十台服务器会带来重大挑战： * *上下文膨胀* *。这可以说是大规模MCP的最大问题， Anthropic的工程团队提出了一个优雅的解决方案—使用代码执行而不是直接工具调用。

> **Source**: [通过 MCP 进行代码执行: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic Engineering Blog

### 问题：Token 浪费的两个来源

* * 1.工具定义过载上下文窗口* *

大多数MCP客户端预先加载所有工具定义。当模型连接到数千个工具时，它必须在读取用户的请求之前处理数十万个令牌。

* * 2.中间结果消耗额外的代币* *

每个中间工具结果都通过模型的上下文。考虑将会议成绩单从Google云端硬盘传输到Salesforce —完整的成绩单在上下文中流动* *两次* * ：一次在阅读时，另一次在写入目的地时。2小时的会议记录可能意味着50,000多个额外的代币。

```mermaid
graph LR
    A ["Model"] -- > | "工具调用： getDocument" | B ["MCP 服务器"]
    B -- > | “完整成绩单（ 50K令牌）” | A
    A -->| "工具调用： updateRecord （<br/>重新发送完整成绩单）" | B
    B -- > | "确认" | A

    style A fill: # ffcdd2, stroke: # 333, color: # 333
        style B fill:#f3e5f5,stroke:#333,color:#333
```

### 解决方案：将 MCP 工具作为代码 API

代理* *编写调用MCP工具作为API的代码* * ，而不是通过上下文窗口传递工具定义和结果。代码在沙盒执行环境中运行，只有最终结果返回到模型。

```mermaid
graph LR
    A ["Model"] -->| "Writes code" | B ["Code Execution<br/>Environment"]
    B -- > | “直接调用工具” | C [“MCP服务器”]
    C -->| "数据保留在执行环境中<br/>" | B
    B -->| "仅最终结果<br/>（最小令牌）" | A

    style A fill: # c8e6c9, stroke: # 333, color: # 333
    样式B填充： # e1f5fe ，笔画： # 333 ，颜色： # 333
    样式C填充： # f3e5f5 ，笔画： # 333 ，颜色： # 333
```

#### 工作原理

MCP工具呈现为类型化函数的文件树：

```
服务器
谷歌云盘
│   ├── getDocument.ts
│   └── index.ts
Salesforce
│   ├── updateRecord.ts
│   └── index.ts
└── ...
```

每个工具文件都包含一个类型化的包装器：

```typescript
//./servers/google-drive/getDocument.ts
从"../../../client.js"导入{callMCPTool};

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

导出异步函数getDocument (
  input: GetDocumentInput
应许
  return callMCPTool<GetDocumentResponse> (
    'google_drive __ get_document' ，输入
  )。
}
```

然后，代理编写代码来编排工具：

```typescript
import * as gdrive from './servers/google-drive';
将*作为salesforce从'./servers/salesforce'导入;

//数据直接在工具之间流动—从不通过模型
const transcript = (
  等待gdrive.getDocument ({documentId: 'abc123'})
CONTENT

等待salesforce.updateRecord ({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  数据： {否tes: transcript}
});
```

**结果：Token 使用从约 150,000 减少到约 2,000 — 减少了 98.7%。**

### 关键好处

| 好处 | 描述 |
|---------|-------------|
| **渐进式展示** | 代理浏览文件系统以仅加载其需要的工具定义, 而不是预先加载所有工具 |
| **上下文-Efficient Results** | Data is filtered/transformed in the execution environment 然后返回给模型 |
| **强大的控制流** | 循环、条件语句和错误处理在代码中运行 无需通过模型往返 |
| **隐私保护** | Intermediate data (PII, sensitive records) stays in the execution environment; 永不进入模型上下文 |
| **状态持久化** | 代理可以将中间结果保存到文件 并构建可重用的技能函数 |

#### 示例：筛选大型数据集

```typescript
// Without code execution — all 10,000 rows flow through context
// 工具调用：gdrive.getSheet(sheetId: 'abc123')
//   -> 返回上下文中的 10,000 行

// With code execution — filter in the execution environment
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(
  row => row["Status"] === 'pending'
);
console.log(`Found ${pendingOrders.length} pending orders`);
console.log(pendingOrders.slice(0, 5)); // 只有 5 行到达模型
```

#### 示例：无需往返的循环

```typescript
// Poll for a deployment notification — runs entirely in code
let found = false;
while (!found) {
  const messages = await slack.getChannel历史({
    channel: 'C123456'
  });
  found = messages.some(
    m => m.text.includes('deployment complete')
  );
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('收到部署通知');
```

### 需要考虑的权衡

代码执行引入其自身的复杂性。 运行代理生成的代码需要：

- A **secure sandboxed execution environment** with appropriate resource limits
- **监控 and logging** of executed code
- Additional **infrastructure overhead** compared to direct tool calls

The benefits — reduced token costs, lower latency, improved tool composition — should be weighed against these implementation costs. 对于只有几个 MCP 服务器的代理，直接调用工具可能更简单。 对于大规模代理（数十个服务器、数百个工具）, 代码执行是一个重大改进。

### MCPorter：MCP 工具组合的运行时

[MCPorter](https://github.com/steipete/mcporter) is a TypeScript runtime and CLI toolkit 使调用 MCP 服务器变得实用而无需样板代码 — 并通过以下方式帮助减少上下文膨胀 选择性工具暴露和类型化包装器。

**解决的问题：** MCPorter 允许您按需发现、检查和调用特定工具, 而不是预先从所有 MCP 服务器加载所有工具定义 — 保持您的上下文精简。

**关键特性：**

| 特性 | 描述 |
|---------|-------------|
| **零配置发现** | 从 Cursor、Claude、Codex 或本地配置自动发现 MCP 服务器 |
| **类型化工具客户端** | `mcporter emit-ts` generates `.d.ts` interfaces 和可直接运行的包装器 |
| **可组合 API** | `createServerProxy()` exposes tools as camelCase methods with `.text()`, `.json()`, `.markdown()` helpers |
| **CLI 生成** | `mcporter generate-cli` converts any MCP server 为独立的 CLI，带有 `--include-tools` / `--exclude-tools` filtering |
| **参数隐藏** | Optional parameters stay hidden by 默认, 减少模式冗长 |

**安装：**

```bash
npx mcporter list          # 否 install required — discover servers instantly
pnpm add mcporter          # 添加到项目
brew install steipete/tap/mcporter  # macOS 通过 Homebrew
```

**Example — composing tools in TypeScript:**

```typescript
import { createRuntime, createServerProxy } from "mcporter";

const runtime = await createRuntime();
const gdrive = createServerProxy(runtime, "google-drive");
const salesforce = createServerProxy(runtime, "salesforce");

// 数据在工具之间流动，而不通过模型上下文
const doc = await gdrive.getDocument({ documentId: "abc123" });
await salesforce.updateRecord({
  objectType: "SalesMeeting",
  recordId: "00Q5f000001abcXYZ",
  data: { 否tes: doc.text() }
});
```

**Example — CLI tool call:**

```bash
# 直接调用特定工具
npx mcporter call linear.create_comment issueId:ENG-123 body:'Looks good!'

# 列出可用服务器和工具
npx mcporter list
```

MCPorter 补充了上述代码执行方法 described above 通过提供运行时基础设施 用于调用 MCP 工具作为类型化 API — 使将中间数据保持在模型上下文之外变得简单。

## 最佳实践

### 安全考虑

#### 应该做 ✅
- Use environment variables for all credentials
- Rotate tokens and API keys regularly (monthly recommended)
- Use read-only tokens when possible
- 限制 MCP server access scope to minimum required
- Monitor MCP server usage and access logs
- Use OAuth for external services when available
- Implement rate limiting on MCP requests
- Test MCP connections before production use
- Document all active MCP connections
- Keep MCP server packages updated

#### 不应该做 ❌
- Don't hardcode credentials in config files
- Don't commit tokens or secrets to git
- Don't share tokens in team chats or emails
- Don't use personal tokens for team projects
- Don't grant unnecessary permissions
- Don't ignore authentication errors
- Don't expose MCP endpoints publicly
- Don't run MCP servers with root/admin privileges
- Don't cache sensitive data in logs
- Don't disable authentication mechanisms

### 配置最佳实践

1. **版本控制**: Keep `.mcp.json` in git but use environment variables for secrets
2. **最小权限**: 授予每个 MCP 服务器所需的最小权限
3. **隔离**: 尽可能在单独的进程中运行不同的 MCP 服务器
4. **监控**: 记录所有 MCP 请求和错误以进行审计跟踪
5. **测试**: 在部署到生产环境之前测试所有 MCP 配置

### 性能提示

- 在应用程序级别缓存频繁访问的数据
- 使用特定的 MCP 查询以减少数据传输
- 监控 MCP 操作的响应时间
- 考虑对外部 API 实施速率限制
- 执行多个操作时使用批处理

## 安装说明

### 前提条件
- Node.js and npm installed
- Claude Code CLI installed
- API tokens/credentials for external services

### Step-by-Step 设置

1. **添加您的第一个 MCP 服务器** 使用 CLI（示例：GitHub）：
```bash
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

   或创建 `.mcp.json` 文件 in your 项目根目录:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

2. **设置环境变量：**
```bash
export GITHUB_TOKEN="your_github_personal_access_token"
```

3. **测试连接：**
```bash
claude /mcp
```

4. **使用 MCP tools:**
```bash
/mcp__github__list_prs
/mcp__github__create_issue "Title" "描述"
```

### Installation for Specific 服务

**GitHub MCP：**
```bash
npm install -g @modelcontextprotocol/server-github
```

**Database MCP：**
```bash
npm install -g @modelcontextprotocol/server-database
```

**Filesystem MCP：**
```bash
npm install -g @modelcontextprotocol/server-filesystem
```

**Slack MCP：**
```bash
npm install -g @modelcontextprotocol/server-slack
```

## 故障排除

### MCP 服务器 否t Found
```bash
# 验证 MCP 服务器已安装
npm list -g @modelcontextprotocol/server-github

# 如果缺失则安装
npm install -g @modelcontextprotocol/server-github
```

### 认证entication Failed
```bash
# 验证环境变量已设置
echo $GITHUB_TOKEN

# 如需要则重新导出
export GITHUB_TOKEN="your_token"

# 验证令牌具有正确的权限
# 检查 GitHub 令牌范围： https://github.com/settings/tokens
```

### 连接超时
- 检查网络连接： `ping api.github.com`
- 验证 API 端点可访问
- 检查 API 速率限制
- 尝试在配置中增加超时
- 检查防火墙或代理问题

### MCP 服务器 Crashes
- 检查 MCP 服务器日志： `~/.claude/logs/`
- 验证所有环境变量已设置
- 确保正确的文件权限
- 尝试重新安装 MCP 服务器包
- 检查同一端口上的冲突进程

## 相关概念

### Memory vs MCP
- **Memory**: 存储持久的、不变的数据 (preferences, context, history)
- **MCP**: 访问实时的、变化的数据 (APIs, databases, real-time services)

### 何时使用
- **使用 Memory** 用于：用户偏好设置、对话历史、学习的上下文
- **使用 MCP** 用于：当前的 GitHub 问题、实时数据库查询、实时数据

### Integration 与其他 Claude 功能
- 将 MCP 与 Memory 结合以获得丰富的上下文
- 使用 在提示中使用 MCP 工具以获得更好的推理
- 利用多个 MCP 进行复杂工作流

## 其他资源

- [官方 MCP 文档](https://code.claude.com/docs/en/mcp)
- [MCP 协议规范](https://modelcontextprotocol.io/specification)
- [MCP GitHub 仓库](https://github.com/modelcontextprotocol/servers)
- [可用的 MCP 服务器](https://github.com/modelcontextprotocol/servers)
- [MCPorter](https://github.com/steipete/mcporter) — TypeScript runtime & CLI for calling MCP servers without boilerplate
- [通过 MCP 进行代码执行](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic 关于解决上下文膨胀的工程博客
- [Claude Code CLI 参考](https://code.claude.com/docs/en/cli-reference)
- [Claude API 文档](https://docs.anthropic.com)
