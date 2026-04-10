---
description: 暂存所有更改，创建提交并推送到远程（谨慎使用）
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---

# 提交并推送所有更改

⚠️ **谨慎**：暂存所有更改、提交并推送到远程。仅在确信所有更改属于同一逻辑时使用。

## 工作流

### 1. 分析更改
并行运行：
- `git status` - 显示修改/添加/删除/未跟踪的文件
- `git diff --stat` - 显示更改统计
- `git log -1 --oneline` - 显示最近的提交以参考消息风格

### 2. 安全检查

**❌ 如果检测到以下内容，停止并警告：**
- 敏感文件：`.env*`、`*.key`、`*.pem`、`credentials.json`、`secrets.yaml`、`id_rsa`、`*.p12`、`*.pfx`、`*.cer`
- API 密钥：任何带有真实值的 `*_API_KEY`、`*_SECRET`、`*_TOKEN` 变量（不是占位符如 `your-api-key`、`xxx`、`placeholder`）
- 大文件：`>10MB` 且未使用 Git LFS
- 构建产物：`node_modules/`、`dist/`、`build/`、`__pycache__/`、`*.pyc`、`.venv/`
- 临时文件：`.DS_Store`、`thumbs.db`、`*.swp`、`*.tmp`

**API 密钥验证：**
检查修改的文件是否包含以下模式：
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 检测到真实密钥！
AWS_SECRET_KEY=AKIA...         # ❌ 检测到真实密钥！
STRIPE_API_KEY=sk_live_...    # ❌ 检测到真实密钥！

# ✅ 可接受的占位符：
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 验证：**
- `.gitignore` 配置正确
- 无合并冲突
- 分支正确（如果是 main/master 则警告）
- API 密钥仅为占位符

### 3. 请求确认

展示摘要：
```
📊 更改摘要：
- X 个文件已修改，Y 个已添加，Z 个已删除
- 总计：+AAA 行插入，-BBB 行删除

🔒 安全：✅ 无敏感文件 | ✅ 无大文件 | ⚠️ [警告]
🌿 分支：[name] → origin/[name]

我将执行：git add . → commit → push

输入 'yes' 继续或 'no' 取消。
```

**等待明确的 "yes" 后再执行。**

### 4. 执行（确认后）

依次运行：
```bash
git add .
git status  # 验证暂存
```

### 5. 生成提交消息

分析更改并创建 conventional commit 格式的提交：

**格式：**
```
[类型]: 简要摘要（最多 72 个字符）

- 关键更改 1
- 关键更改 2
- 关键更改 3
```

**类型：** `feat`、`fix`、`docs`、`style`、`refactor`、`test`、`chore`、`perf`、`build`、`ci`

**示例：**
```
docs: 更新概念 README 文件，添加全面文档

- 添加架构图和表格
- 包含实际示例
- 扩展最佳实践部分
```

### 6. 提交并推送

```bash
git commit -m "$(cat <<'EOF'
[生成的提交消息]
EOF
)"
git push  # 如果失败：git pull --rebase && git push
git log -1 --oneline --decorate  # 验证
```

### 7. 确认成功

```
✅ 成功推送到远程！

提交：[hash] [消息]
分支：[branch] → origin/[branch]
文件更改：X（+insertions，-deletions）
```

## 错误处理

- **git add 失败**：检查权限、锁定文件、验证仓库是否已初始化
- **git commit 失败**：修复 pre-commit hook、检查 git 配置（user.name/email）
- **git push 失败**：
  - 非快进：`git pull --rebase && git push`
  - 无远程分支：`git push -u origin [branch]`
  - 受保护分支：改用 PR 工作流

## 使用场景

✅ **适合：**
- 多文件文档更新
- 带测试和文档的功能
- 跨文件的错误修复
- 项目范围的格式化/重构
- 配置更改

❌ **避免：**
- 不确定正在提交什么
- 包含敏感/隐私数据
- 受保护分支且无审查
- 存在合并冲突
- 想要细粒度的提交历史
- pre-commit hook 失败

## 替代方案

如果用户想要控制，建议：
1. **选择性暂存**：审查并暂存特定文件
2. **交互式暂存**：`git add -p` 选择补丁
3. **PR 工作流**：创建分支 → 推送 → PR（使用 `/pr` 命令）

**⚠️ 记住**：推送前始终审查更改。不确定时，使用单独的 git 命令以获得更多控制。