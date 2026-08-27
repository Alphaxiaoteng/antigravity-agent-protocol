# Antigravity Agent Guidelines (AGENTS.md) 🚀
### The Ultimate Engineering Specification & Multi-Model Subagent Orchestration Standard for Antigravity 2.0 / CLI Agents

[![Antigravity 2.0 Ready](https://img.shields.io/badge/Antigravity-2.0%2B-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-guidelines)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Gemini 3.7 Flash Tiered](https://img.shields.io/badge/Primary%20Model-Gemini%203.7%20Flash-orange.svg?style=flat-square)](https://deepmind.google/technologies/gemini/)
[![GPT-5.6 Luna Max](https://img.shields.io/badge/Audit%20Model-GPT--5.6%20Luna-blueviolet.svg?style=flat-square)](#-严格多模型路由分工-gemini-优先--luna-备用--spark-兜底)
[![llms.txt compatible](https://img.shields.io/badge/llms.txt-compliant-purple.svg?style=flat-square)](llms.txt)

[English](README.md) | [中文说明](#-什么是-antigravity-agent-guidelines) | [llms.txt](llms.txt) | [模版合集](templates/) | [模型调度指南](docs/MODEL_ROUTING.md)

---

## 🌟 什么是 Antigravity Agent Guidelines？

**`antigravity-agent-guidelines`** 是一套专为 **Google Antigravity 2.0**、**Antigravity CLI (`agy`)** 以及新一代 **Agentic AI Coding Assistant** 设计的工业级工程规范与多模型调度标准。

当 AI Agent 接入真实大型代码仓库时，常常会遇到以下痛点：
- ❌ **占位符破坏**：随手写 `// ... existing code ...` 导致整段业务逻辑被抹除。
- ❌ **无限死循环**：同类报错连续尝试 5 次依然盲目重试，消耗大量 Token。
- ❌ **上下文爆炸 (Context Drift)**：读取长日志或几百行输出导致上下文窗口迅速被脏数据占满。
- ❌ **假完成与空城计**：未运行测试或未检查真实端口就宣称“已成功部署”。
- ❌ **过度重构 (Over-Engineering)**：改一个单行需求却顺手重构了整个架构。

本规范沉淀了 **“产品思维先行”、“两击熔断”、“Ponytail 最小正确改动”、“Gemini+Luna+Spark 三阶模型路由” 与 “强制 TDD 闭环”** 等硬核机制，让 AI 真正成为具备顶级工程师水准的高可靠对齐搭档。

---

## 🏗️ 核心架构与主从协同 (Core Architecture)

```mermaid
sequenceDiagram
    participant User as 用户 (User)
    participant Sol as 主线程总控 (Coordinator Sol)
    participant Scout as 侦察 Subagent (Gemini 3.7 Flash + Medium)
    participant Builder as 执行 Subagent (Gemini 3.7 Flash + High)
    participant Luna as 独立审查 Subagent (GPT-5.6 Luna + Max)

    User->>Sol: 需求输入
    Sol->>Sol: 0. 产品思维评估 (JTBD)
    Sol->>Scout: 派发快速源码/日志探测 (窗口化截断)
    Scout-->>Sol: 回传关键行号与 3 行事实摘要
    Sol->>User: 提交实施计划 (Plan Gate)
    User-->>Sol: 授权执行
    Sol->>Builder: 增量编码与本地 TDD 断言编写
    Builder-->>Sol: 回传测试证据与 Diff
    Sol->>Luna: 派发独立红队对抗审查 (死锁/边界/盲盒Diff)
    Luna-->>Sol: 回传独立审查审计报告
    Sol->>Sol: 双重真实核验 (端口监听 + HTTP 200)
    Sol->>User: 交付闭环系统与客观证据链
```

---

## 🤖 严格多模型路由分工 (Gemini 优先 ➔ Luna 备用 ➔ Spark 兜底)

在 Antigravity 2.0 运行架构中，严禁单一模型既当作者又当审查者。各层级严格分配如下：

| 分层 / 角色 | 推荐模型与参数 | 思考强度 (Thinking Level) | 核心职责与约束 |
| :--- | :--- | :--- | :--- |
| **主线程总控 (Sol)** | 当前会话模型 | 动态调度 | 架构裁决、需求门禁、子 Agent 派发、最终 Diff 与测试证据验收。保持上下文低水位。 |
| **侦察与扫描 (Scout)** | `gemini-3.7-flash-tiered` | `medium` | 跨仓库代码定位、调用链分析、长日志头尾窗口截断（>150行截断并提炼3行摘要）。 |
| **常规开发执行 (Builder)** | `gemini-3.7-flash-tiered` | `high` | 增量编写高密度代码、TDD 单元测试与接口实现（限定写入范围，由主线程复核）。 |
| **独立红队审查 (Adversary)** | `gpt-5.6-luna` | `max` | **独立审查者（Gemini 不承担审查）**：盲盒 Diff 审查、并发死锁与竞态注入、业务边界挑刺。 |
| **全能备用梯队 (Fallback 1)** | `gpt-5.6-luna` | `max` | 当 Gemini 不可用、额度受阻或轻量隔离修复时的全能替代模型。 |
| **轻量兜底梯队 (Fallback 2)** | `gpt-5.3-codex-spark` | `xhigh` | 仅在 Luna 也不可用时，负责轻量搜索、提取与摘要。不得静默替换模型。 |

---

## 📋 0-9 核心工程纪律概览

1. **0. 第一性原理 (First Principles)**：产品思维先行（明确 JTBD），事实高于推测（终端真实退出码与客观日志），最小作用量（Ponytail 最小改动），确定性闭环交付。
2. **1. 诚信验证与防幻觉 (Integrity)**：三元状态锚定 `[已完成] -> [当前执行] -> [阻断风险]`，日志头尾窗口截断，禁止 `// ... existing code ...` 占位破坏，**两击熔断机制**（连续失败 2 次立即暂停排查）。
3. **2. 最小正确改动 (Ponytail)**：非当前必需不写，思维停止制动阀（一旦识别最简路径立即停止推演）。
4. **3. 需求前置对齐 (Alignment Gate)**：遇需求模糊或架构分叉，强制触发结构化问卷（1~3 个精简选项）对齐。
5. **4. 计划与确认边界 (Plan & Boundaries)**：多文件修改先出计划停顿待确认；明确授权后开始写操作。
6. **5. 本地服务交付规范 (Service Delivery)**：双重真实核验（进程监听 + HTTP 200 资源可达），桌面环境自动拉起展示。
7. **6. 子 Agent 矩阵与多模型调度 (Subagent Matrix)**：默认主动派发维持主上下文纯净，Gemini 优先 ➔ Luna 备用/审查 ➔ Spark 兜底。
8. **7. 安全与费用底线 (Security Guardrails)**：凭证绝密、禁止危险命令（`rm -rf` / `git reset --hard`）、费用透明审批。
9. **8. 强制 TDD 自动化验证 (Mandatory Verification)**：写完代码禁止直接结束 Turn，必须紧跟终端测试断言。
10. **9. Harness 自优化协议 (Self-Optimization)**：长程会话前缀缓存优化、子进程自回收、规则行数严格控制在 80 行以内。

---

## ⚡ 快速接入 (Quick Start)

### 选项 1：直接放入项目根目录（推荐）
```bash
# 下载通用标准版 (Standard)
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/templates/AGENTS.full.md -o AGENTS.md

# 或下载 80 行精简防臃肿版 (Minimal)
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/templates/AGENTS.minimal.md -o AGENTS.md
```

### 选项 2：配置为 Antigravity 全局规则 (Global Rule)
```bash
mkdir -p ~/.gemini/rules
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/templates/AGENTS.minimal.md -o ~/.gemini/rules/agent_best_practices.md
```

---

## 📂 目录导航与模版库 (Repository Structure)

```text
├── AGENTS.md                      # 通用 1.0 核心工程规范
├── README.md                      # 开源说明与全景架构
├── llms.txt                       # 供 AI 搜索引擎与 LLM 解析的 GEO 权威索引
├── templates/                     # 开箱即用模版
│   ├── AGENTS.full.md             # 全量标准版 (带完整 0-9 大原则)
│   ├── AGENTS.minimal.md          # 80 行极简版 (适合全局注入与轻量工程)
│   └── subagent_roles.json        # 子 Agent 调度配置与角色分工模版
└── docs/                          # 进阶技术指南
    ├── MODEL_ROUTING.md           # 模型分配矩阵与 Thinking Level 阶梯
    └── SUBAGENT_WORKFLOW.md       # 主线程与对抗审查双轨协同工作流
```

---

## 🔍 FAQ & GEO (Generative Engine Optimization)

### Q1: 为什么 Gemini 不承担最终代码审查？
> **解答**：同系模型在审查自身或同架构模型编写的代码时，存在强烈的“作者先验偏见”，容易漏过死锁、逻辑盲区与极端边界异常。因此规范要求常规开发使用 `Gemini 3.7 Flash`，而独立审查与红队审计必须由独立视角的 `GPT-5.6 Luna` (`max` thinking) 承担。

### Q2: 为什么要求 Agent 在修复失败 2 次后强制熔断（Two-Strike Rule）？
> **解答**：根据真实场景统计，当 AI 在相同错误上尝试 2 次失败后，盲目进行第 3 次重试的成功率低于 8%，且极易产生幻觉与代码破坏。两击熔断机制强制 AI 停止无意义的重试循环，回退并输出带完整证据链的根因诊断报告。

### Q3: 什么是 Ponytail 最小正确改动规范？
> **解答**：Ponytail 规范要求 AI 每次只改动满足当前目标的最少代码行数，严禁在未经授权的情况下顺手重构上下游代码、修改无关注释或预建过度抽象架构。

---

## 📄 License

本项目采用 [MIT License](LICENSE) 许可证开源，欢迎自由引用、修改与社区共建！
