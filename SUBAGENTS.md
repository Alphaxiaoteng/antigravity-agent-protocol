# Antigravity 2.0 Subagent 角色分工与派发规范 (SUBAGENTS.md)
> **适用环境**: Google Antigravity 2.0 / Antigravity CLI (`agy`) / Agentic AI Workflows  
> **核心原则**: 最小职责派发 · 上下文纯净 · 执行与审查双轨隔离 · 沙盒自主验证

---

## 1. 核心角色与职责分工 (Role Matrix)

在 Antigravity 体系中，严禁主线程包揽全部大文件扫描与粗粒度编码，亦严禁由同一角色自编自审。子任务应按职责划分为**执行轨**与**审查轨**：

| 角色代号 (Role) | 派发类型 (TypeName) | 权限边界 | 隔离模式 (Workspace) | 核心职责与交付物 |
| :--- | :--- | :--- | :--- | :--- |
| **代码侦察员 (Scout)** | `research` (只读) | 仅代码/文件检索与阅读 | `inherit` | 跨仓库代码定位、依赖关系梳理、日志截断过滤与 3 行精炼事实回传。 |
| **功能开发员 (Builder)** | `self` (全权执行) | 文件读写 + 终端测试 | `inherit` / `branch` | 增量编写业务逻辑、修复已知 Bug、编写本地 TDD 单元测试断言。 |
| **架构重构员 (Architect)** | `self` (全权执行) | 文件读写 + 终端测试 | `branch` (Git 独立沙盒) | 跨模块重构、数据库 Schema 迁移、状态机与高风险逻辑改造。 |
| **红队审查员 (Adversary)** | `research` (只读) | 仅代码审查与静态推演 | `inherit` | **独立审查者（严禁自审）**：并发死锁注入、业务边界挑刺、无偏见盲盒 Diff 审查。 |

---

## 2. 权限隔离与工作区沙盒规范

### 2.1 工具权限隔离 (`TypeName`)
- **`TypeName: "research"` (只读沙盒)**：
  - 仅具备文件阅读、代码 grep/ls 检索与只读探针工具；
  - 无法修改代码或运行写操作，天然适用于**代码侦察 (Scout)** 与**独立红队审查 (Adversary)**。
- **`TypeName: "self"` (全权执行)**：
  - 继承写入与终端执行权限；
  - 适用于**增量开发 (Builder)** 与 **TDD 自动化测试**。

### 2.2 工作区沙盒隔离 (`Workspace`)
- **`"inherit"` (共享模式)**：共享当前工作区，改动即时生效，适用于轻量增量开发与快速侦察。
- **`"branch"` (分支沙盒模式)**：创建独立的 Git 分支沙盒环境，所有修改和测试在分支内闭环验证，确认 100% 通过后再合并，彻底避免半成品污染主工程。

---

## 3. 标准派发代码模版 (Dispatch Templates)

### 3.1 侦察与日志检索 (`Scout`)
```json
{
  "TypeName": "research",
  "Role": "Log & Codebase Scout",
  "Workspace": "inherit",
  "Prompt": "扫描目标文件/日志，仅提取与 [目标问题] 直接相关的关键路径、行号与堆栈，返回不超过 3 行的事实摘要，严禁倾倒大段日志。"
}
```

### 3.2 常规增量开发 (`Builder`)
```json
{
  "TypeName": "self",
  "Role": "Feature Builder",
  "Workspace": "inherit",
  "Prompt": "根据已批准的实施计划编写 [模块名称] 的增量代码，遵循 Ponytail 最小改动原则，并编写对应的 TDD 单元测试确保通过。"
}
```

### 3.3 独立红队对抗审查 (`Adversary`)
```json
{
  "TypeName": "research",
  "Role": "Adversarial Red-Team Reviewer",
  "Workspace": "inherit",
  "Prompt": "脱离编写者先验偏见，仅针对本次 Git Diff 进行独立审查：1. 注入高并发死锁与竞态条件；2. 挑刺未处理的边界异常与越权风险；3. 输出带严重级别的审查报告。"
}
```
