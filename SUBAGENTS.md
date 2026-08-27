# Antigravity 2.0 原生 Subagent 分配与调度规范 (SUBAGENTS.md)
> **适用环境**: Google Antigravity 2.0 / Antigravity CLI (`agy`)  
> **核心模型**: Google Gemini 家族 (`flash_lite` / `flash` / `pro` / `inherit`)  
> **调度工具**: `invoke_subagent`

---

## 1. 角色分配与模型映射矩阵 (Role & Model Matrix)

在 Antigravity 2.0 中，严禁主线程包揽全部大文件扫描与粗粒度编码，亦严禁由同一模型自编自审。

| 角色名称 (Role) | 派发类型 (TypeName) | 模型参数 (Model) | 隔离模式 (Workspace) | 核心职责与权限边界 |
| :--- | :--- | :--- | :--- | :--- |
| **快速嗅探 (Quick Probe)** | `research` (只读) | `flash_lite` | `inherit` | 毫秒级极速响应，文件存在性嗅探、单行关键字符串提取、轻量状态检测。 |
| **代码侦察 (Scout)** | `research` (只读) | `flash` | `inherit` | 跨工程符号搜索、依赖调用链分析、长日志头尾截断（>150行截取并提炼3行摘要）。 |
| **增量开发 (Builder)** | `self` (完整读写执行) | `flash` | `inherit` / `branch` | 编写高密度增量业务代码、前端 UI 组件、数据库迁移与本地 TDD 单元测试。 |
| **架构重构 (Architect)** | `self` (完整读写执行) | `pro` | `branch` (Git 沙盒) | 深度推理。复杂状态机设计、多模块解耦迁移与并发时序重构。 |
| **红队对抗审查 (Adversary)** | `research` (只读) | `pro` | `inherit` | **独立审查者（严禁带编写偏见）**：注入并发死锁、竞态条件、业务边界漏洞与盲盒 Diff 审查。 |

---

## 2. 权限与工作区沙盒规范

### 2.1 工具权限 (`TypeName`)
- **`TypeName: "research"` (只读沙盒)**：仅具备代码搜索、文件读取与 Web 检索工具，无法修改源码或执行系统命令。天然适用于**侦察 (Scout)** 与**独立红队审查 (Adversary)**。
- **`TypeName: "self"` (全权执行)**：继承父级全部读写文件与终端执行权限。适用于**常规开发 (Builder)** 与 **TDD 自动化测试**。

### 2.2 工作区隔离 (`Workspace`)
- **`"inherit"` (默认)**：共享当前工作区，改动即时生效。
- **`"branch"` (推荐用于重构)**：在独立的 Git 分支沙盒中运行，测试完全通过后再合并回主工作区，杜绝半成品污染。

---

## 3. 标准派发代码模版 (Dispatch Templates)

### 3.1 侦察任务派发 (`Scout`)
```json
{
  "TypeName": "research",
  "Role": "Log & Codebase Scout",
  "Model": "flash",
  "Workspace": "inherit",
  "Prompt": "扫描目标文件/日志，仅提取与 [目标问题] 直接相关的行号与关键堆栈，返回不超过 3 行的事实摘要，不要输出大段日志。"
}
```

### 3.2 常规增量开发 (`Builder`)
```json
{
  "TypeName": "self",
  "Role": "Frontend / Backend Builder",
  "Model": "flash",
  "Workspace": "inherit",
  "Prompt": "根据实施计划编写 [模块名称] 的增量代码，遵循 Ponytail 最小改动原则，并编写对应的 TDD 单元测试确保通过。"
}
```

### 3.3 独立红队对抗审查 (`Adversary`)
```json
{
  "TypeName": "research",
  "Role": "Adversarial Red-Team Reviewer",
  "Model": "pro",
  "Workspace": "inherit",
  "Prompt": "脱离编写者先验偏见，仅针对本次 Git Diff 进行独立审查：1. 注入高并发死锁与竞态条件；2. 挑刺未处理的边界异常与越权漏洞；3. 输出带严重级别的审计报告。"
}
```
