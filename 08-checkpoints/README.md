<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Checkpoints and Rewind

检查点允许你保存对话状态并回退到 Claude Code 会话中的之前时刻。这对于探索不同的方法、从错误中恢复或比较替代方案非常宝贵。

## Overview

检查点允许你保存对话状态并回退到之前的点，实现安全实验和多种方法的探索。它们是对话状态的快照，包括：
- 所有已交换的消息
- 文件修改
- 工具使用历史
- 会话上下文

检查点在探索不同的方法、从错误中恢复或比较替代方案时非常宝贵。

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Checkpoint** | 对话状态的快照，包括消息、文件和上下文 |
| **Rewind** | 返回到之前的检查点，丢弃后续的更改 |
| **Branch Point** | 从中探索多种方法的检查点 |

## Accessing Checkpoints

你可以通过两种主要方式访问和管理检查点：

### Using Keyboard Shortcut
按两次 `Esc`（`Esc` + `Esc`）打开检查点界面并浏览已保存的检查点。

### Using Slash Command
使用 `/rewind` 命令（别名：`/checkpoint`）快速访问：

```bash
# 打开回退界面
/rewind

# 或使用别名
/checkpoint
```

## Rewind Options

当你回退时，会看到五个选项的菜单：

1. **Restore code and conversation** -- 将文件和消息都回退到该检查点
2. **Restore conversation** -- 仅回退消息，保持当前代码不变
3. **Restore code** -- 仅恢复文件更改，保持完整对话历史
4. **Summarize from here** -- 将此点之后的对话压缩成 AI 生成的摘要，而不是丢弃它。原始消息保留在记录中。你可以选择提供 instructions 来专注于特定主题。
5. **Never mind** -- 取消并返回当前状态

## Automatic Checkpoints

Claude Code 会自动为你创建检查点：

- **Every user prompt** - 每次用户输入都会创建新检查点
- **Persistent** - 检查点跨会话持久化
- **Auto-cleaned** - 检查点在 30 天后自动清理

这意味着你可以随时回退到对话中的任何之前时刻，从几分钟前到几天前。

## Use Cases

| Scenario | Workflow |
|----------|----------|
| **Exploring Approaches** | 保存 → 尝试 A → 保存 → 回退 → 尝试 B → 比较 |
| **Safe Refactoring** | 保存 → 重构 → 测试 → 如果失败：回退 |
| **A/B Testing** | 保存 → 设计 A → 保存 → 回退 → 设计 B → 比较 |
| **Mistake Recovery** | 发现问题 → 回退到上一个良好状态 |

## Using Checkpoints

### Viewing and Rewinding

按两次 `Esc` 或使用 `/rewind` 打开检查点浏览器。你将看到所有可用检查点的列表及其时间戳。选择任何检查点即可回退到该状态。

### Checkpoint Details

每个检查点显示：
- 创建时的时间戳
- 修改过的文件
- 对话中的消息数量
- 使用过的工具

## Practical Examples

### Example 1: Exploring Different Approaches

```
User: Let's add a caching layer to the API

Claude: I'll add Redis caching to your API endpoints...
[Makes changes at checkpoint A]

User: Actually, let's try in-memory caching instead

Claude: I'll rewind to explore a different approach...
[User presses Esc+Esc and rewinds to checkpoint A]
[Implements in-memory caching at checkpoint B]

User: Now I can compare both approaches
```

### Example 2: Recovering from Mistakes

```
User: Refactor the authentication module to use JWT

Claude: I'll refactor the authentication module...
[Makes extensive changes]

User: Wait, that broke the OAuth integration. Let's go back.

Claude: I'll help you rewind to before the refactoring...
[User presses Esc+Esc and selects the checkpoint before the refactor]

User: Let's try a more conservative approach this time
```

### Example 3: Safe Experimentation

```
User: Let's try rewriting this in a functional style
[Creates checkpoint before experiment]

Claude: [Makes experimental changes]

User: The tests are failing. Let's rewind.
[User presses Esc+Esc and rewinds to the checkpoint]

Claude: I've rewound the changes. Let's try a different approach.
```

### Example 4: Branching Approaches

```
User: I want to compare two database designs
[Takes note of checkpoint - call it "Start"]

Claude: I'll create the first design...
[Implements Schema A]

User: Now let me go back and try the second approach
[User presses Esc+Esc and rewinds to "Start"]

Claude: Now I'll implement Schema B...
[Implements Schema B]

User: Great! Now I have both schemas to choose from
```

## Checkpoint Retention

Claude Code 自动管理你的检查点：

- 每次用户提示都会自动创建检查点
- 旧检查点保留最多 30 天
- 检查点自动清理以防止无限存储增长

## Workflow Patterns

### Branching Strategy for Exploration

探索多种方法时：

```
1. Start with initial implementation → Checkpoint A
2. Try Approach 1 → Checkpoint B
3. Rewind to Checkpoint A
4. Try Approach 2 → Checkpoint C
5. Compare results from B and C
6. Choose best approach and continue
```

### Safe Refactoring Pattern

进行重大更改时：

```
1. Current state → Checkpoint (auto)
2. Start refactoring
3. Run tests
4. If tests pass → Continue working
5. If tests fail → Rewind and try different approach
```

## Best Practices

由于检查点是自动创建的，你可以专注于工作而不必担心手动保存状态。但是，请记住以下实践：

### Using Checkpoints Effectively

✅ **Do:**
- 回退前查看可用的检查点
- 想探索不同方向时使用回退
- 保留检查点以比较不同方法
- 了解每个回退选项的作用（恢复代码和对话、恢复对话、恢复代码或总结）

❌ **Don't:**
- 仅依赖检查点来保存代码
- 期望检查点跟踪外部文件系统更改
- 使用检查点作为 git 提交的替代品

## Configuration

你可以在设置中切换自动检查点：

```json
{
  "autoCheckpoint": true
}
```

- `autoCheckpoint`: 启用或禁用每次用户提示时自动创建检查点（默认：`true`）

## Limitations

检查点有以下限制：

- **Bash command changes NOT tracked** - 文件系统上的 `rm`、`mv`、`cp` 等操作不会被捕获到检查点中
- **External changes NOT tracked** - 在 Claude Code 之外（在你的编辑器、终端等中）进行的更改不会被捕获
- **Not a replacement for version control** - 使用 git 对代码库进行永久的、可审计的更改

## Troubleshooting

### Missing Checkpoints

**Problem**: 找不到预期的检查点

**Solution**:
- 检查检查点是否被清除
- 验证设置中 `autoCheckpoint` 是否启用
- 检查磁盘空间

### Rewind Failed

**Problem**: 无法回退到检查点

**Solution**:
- 确保没有未提交的更改冲突
- 检查检查点是否损坏
- 尝试回退到其他检查点

## Integration with Git

检查点补充（但不能替代）git：

| Feature | Git | Checkpoints |
|---------|-----|-------------|
| Scope | File system | Conversation + files |
| Persistence | Permanent | Session-based |
| Granularity | Commits | Any point |
| Speed | Slower | Instant |
| Sharing | Yes | Limited |

一起使用两者：
1. 使用检查点进行快速实验
2. 使用 git 提交最终更改
3. 在 git 操作之��创建检查点
4. 将成功的检查点状态提交到 git

## Quick Start Guide

### Basic Workflow

1. **Work normally** - Claude Code 自动创建检查点
2. **Want to go back?** - 按两次 `Esc` 或使用 `/rewind`
3. **Choose checkpoint** - 从列表中选择回退
4. **Select what to restore** - 选择恢复代码和对话、恢复对话、恢复代码、从这里总结或取消
5. **Continue working** - 你回到了那个点

### Keyboard Shortcuts

- **`Esc` + `Esc`** - 打开检查点浏览器
- **`/rewind`** - 访问检查点的替代方式
- **`/checkpoint`** - `/rewind` 的别名

## Knowing When to Rewind: Context Monitoring

检查点让你可以回去 —— 但你如何知道 *什么时候* 应该回去？随着对话增长，Claude 的上下文窗口会填满，模型质量会悄悄下降。你可能在不知不觉中使用了半盲模型发送代码。

**[cc-context-stats](https://github.com/luongnv89/cc-context-stats)** 通过向你的 Claude Code 状态栏添加实时**上下文区域**来解决这个问题。它跟踪你在上下文窗口中的位置 —— 从**Plan**（绿色，安全进行计划和编码）到**Code**（黄色，避免开始新计划）再到**Dump**（橙色，完成并回退）。当看到区域切换时，你就知道是时候创建检查点并重新开始，而不是在下降的输出中继续推进。

## Related Concepts

- **[Advanced Features](../09-advanced-features/)** - 计划模式和其他高级功能
- **[Memory Management](../02-memory/)** - 管理对话历史和上下文
- **[Slash Commands](../01-slash-commands/)** - 用户调用的快捷方式
- **[Hooks](../06-hooks/)** - 事件驱动的自动化
- **[Plugins](../07-plugins/)** - 捆绑的扩展包

## Additional Resources

- [Official Checkpointing Documentation](https://code.claude.com/docs/en/checkpointing)
- [Advanced Features Guide](../09-advanced-features/) - 扩展思考和其他功能

## Summary

检查点是 Claude Code 中的自动功能，让你安全地探索不同的方法而不必担心丢失工作。每次用户提示都会自动创建新检查点，因此你可以回退到会话中的任何之前时刻。

主要好处：
- 无畏地尝试多种方法
- 快速从错误中恢复
- 并排比较不同的解决方案
- 与版本控制系统安全集成

记住：检查点不能替代 git。将检查点用于快速实验，将 git 用于永久的代码更改。