---
name: 扩展单元测试
description: 通过针对未测试的分支和边缘情况来增加测试覆盖率
tags: 测试, 覆盖率, 单元测试
---

# 扩展单元测试

根据项目的测试框架扩展现有单元测试：

1. **分析覆盖率**：运行覆盖率报告以识别未测试的分支、边缘情况和低覆盖率区域
2. **识别差距**：审查代码的逻辑分支、错误路径、边界条件、null/空输入
3. **使用项目框架编写测试**：
   - Jest/Vitest/Mocha（JavaScript/TypeScript）
   - pytest/unittest（Python）
   - Go testing/testify（Go）
   - Rust 测试框架（Rust）
4. **针对特定场景**：
   - 错误处理和异常
   - 边界值（最小/最大、空、null）