# Google Antigravity Agent Guidelines (`AGENTS.md`) 🚀
### 工业级 Google Antigravity & Agentic AI 工程规范与 Subagent 协同准则 (v3.0.21)

[![Antigravity Version](https://img.shields.io/badge/Antigravity-v3.0.21%20Ready-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-guidelines)
[![Specification](https://img.shields.io/badge/Specification-AGENTS.md%20v3.0.21-orange.svg?style=flat-square)](https://github.com/Alphaxiaoteng/antigravity-agent-guidelines)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

[核心规范 (AGENTS.md)](AGENTS.md) | [Subagent 分工指南 (SUBAGENTS.md)](SUBAGENTS.md) | [快速上手](#-极速接入-quick-start)

---

## 🌟 为什么 Antigravity 项目需要 `AGENTS.md`？

在 **Google Antigravity** 与 **Antigravity CLI (`agy`)** 体系中，`AGENTS.md` 是官方最高优先级的**工作区规则配置标准 (Workspace Rules)**。

本规范（v3.0.21）专为解决自主 AI 编码智能体的核心顽疾而生：
- 🛡️ **彻底杜绝代码破坏**：严禁 `// ... existing code ...` 等占位破坏性重写。
- ⚡ **两击熔断机制 (Two-Strike Circuit Breaker)**：同类报错连续尝试 2 次失败强制中止，输出带证据链的根因诊断报告，拒绝无意义死循环。
- 🎯 **Ponytail 最小正确改动**：非当前必需不写，严格控制修改范围，禁止顺手重构与过度设计。
- 🧪 **强制闭环交付与 TDD 验证**：写完逻辑紧接着执行终端测试与断言探针，拒绝假完成与空城计。
- 👥 **Subagent 双轨对抗审查**：执行轨（Builder）与审查轨（Adversary）权限隔离，杜绝自编自审的作者先验偏见。

---

## 📁 核心文档导航

| 核心文件 | 作用与定位 |
| :--- | :--- |
| **📄 [AGENTS.md](AGENTS.md)** | **工作区核心工程纪律规范 (v3.0.21)**：涵盖 0~9 大硬核纪律（产品思维、防幻觉、两击熔断、Ponytail 最小改动、双重服务核验、强制 TDD 验证、Harness 自优化）。 |
| **🤖 [SUBAGENTS.md](SUBAGENTS.md)** | **Subagent 角色分工与派发手册 (v3.0.21)**：定义了侦察员 (Scout)、开发员 (Builder)、架构师 (Architect) 与审查员 (Adversary) 的权限沙盒与标准派发 JSON 模版。 |

---

## ⚡ 极速接入 (Quick Start)

在任何工程根目录下直接拉取最新 v3.0.21 规范：

```bash
# 1. 下载核心工程规范 AGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/AGENTS.md -o AGENTS.md

# 2. 下载子 Agent 分工手册 SUBAGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/SUBAGENTS.md -o SUBAGENTS.md
```

---

## 📄 License

本项目采用 [MIT License](LICENSE) 许可证开源，欢迎自由引用、Fork 与社区共建！
