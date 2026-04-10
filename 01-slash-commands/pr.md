---
description: 清理代码、暂存更改并准备拉取请求
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# 拉取请求准备清单

创建 PR 前，执行以下步骤：

1. 运行格式化：`prettier --write .`
2. 运行测试：`npm test`
3. 查看 git 差异：`git diff HEAD`
4. 暂存更改：`git add .`
5. 按 conventional commits 创建提交消息：
   - `fix:` 用于错误修复
   - `feat:` 用于新功能
   - `docs:` 用于文档
   - `refactor:` 用于代码重构
   - `test:` 用于测试添加
   - `chore:` 用于维护