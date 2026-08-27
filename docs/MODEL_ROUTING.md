# Antigravity 原生 Gemini Subagent 调度与参数指南

在 Google Antigravity 2.0 中，Subagent 原生基于 **Gemini 基础模型家族** 构建，通过 `invoke_subagent` 工具提供开箱即用的多智能体协同能力。

---

## 1. 原生 Model 参数枚举与适用场景

`invoke_subagent` 工具的 `Model` 参数支持以下枚举：

| Model 参数值 | 底层 Gemini 模型 | 特性与优势 | 推荐使用场景 |
| :--- | :--- | :--- | :--- |
| **`"flash_lite"`** | Gemini Flash-Lite | 极低延迟（<1s），超低 Token 开销 | 文件存在性嗅探、单行字符串提取、环境状态轻量心跳检测 |
| **`"flash"`** | Gemini 3.7 Flash | 极高吞吐、强推理能力、低延迟 | 跨仓库检索（`research`）、增量代码编写（`self`）、TDD 单元测试 |
| **`"pro"`** | Gemini 3.7 Pro | 极深推理能力、高复杂度长程规划 | 架构重构设计、红队对抗审查（死锁/竞态检测）、多模块复杂迁移 |
| **`"inherit"`** (默认) | 继承当前 Agent 模型 | 保持与主调用者一致的思考上下文 | 复杂长任务的无缝接力子任务 |

---

## 2. TypeName 工具权限隔离

- **`"research"`**：只读工具集（代码搜索、文件读取、Web 搜索），无法修改项目代码或执行终端命令，天然适合**侦察 (Scout)** 与**独立红队审查 (Adversary Audit)**。
- **`"self"`**：完整工具集（继承父级的所有写入、终端执行与测试工具），适合**增量开发 (Builder)** 与**TDD 自动化验证 (Test Engineer)**。

---

## 3. Workspace 隔离模式

- **`"inherit"`**：共享主工作区，改动即时生效。
- **`"branch"`**：创建隔离的 Git 分支工作区，改动在沙盒内进行，验证通过后再合并，适合高风险重构与大版本迁移。
- **`"share"`**：基于工作树（Worktree）的共享空间，兼顾隔离与存储效率。
