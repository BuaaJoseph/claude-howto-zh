---
name: 设置 CI/CD 流水线
description: 实现 pre-commit hooks 和 GitHub Actions 以确保质量
tags: ci-cd, devops, 自动化
---

# 设置 CI/CD 流水线

根据项目类型实现全面的 DevOps 质量门：

1. **分析项目**：检测语言、框架、构建系统和现有工具
2. **配置 pre-commit hooks** 使用语言特定工具：
   - 格式化：Prettier/Black/gofmt/rustfmt 等
   - 代码检查：ESLint/Ruff/golangci-lint/Clippy 等
   - 安全：Bandit/gosec/cargo-audit/npm audit 等
   - 类型检查：TypeScript/mypy/flow（如果适用）
   - 测试：运行相关测试套件
3. **创建 GitHub Actions 工作流**（.github/workflows/）：
   - 在 push/PR 时镜像 pre-commit 检查
   - 多版本/平台矩阵（如果适用）