---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [消息]
description: 带上下文的 git 提交
---

## 上下文

- 当前 git 状态：!`git status`
- 当前 git 差异：!`git diff HEAD`
- 当前分支：!`git branch --show-current`
- 最近提交：!`git log --oneline -10`

## 任务

根据上述更改，创建单个 git 提交。

如果通过参数提供了消息，使用它：$ARGUMENTS

否则，分析更改并按照 conventional commits 格式创建适当的提交消息：