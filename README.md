# Antigravity Agent Guidelines 🚀
### Google Antigravity 2.0 / CLI (`agy`) 官方工程规范与 Subagent 调度标准 (v1.0)

[![Antigravity 2.0 Native](https://img.shields.io/badge/Antigravity-2.0%2B%20Config-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-guidelines)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Gemini Native Subagents](https://img.shields.io/badge/Subagents-Gemini%20Flash%20%2F%20Pro-orange.svg?style=flat-square)](https://deepmind.google/technologies/gemini/)

[核心规范 (AGENTS.md)](AGENTS.md) | [Subagent 分配指南 (SUBAGENTS.md)](SUBAGENTS.md) | [llms.txt](llms.txt)

---

## 🌟 核心文档导航

本项目专为 **Google Antigravity 2.0** 与 **Antigravity CLI (`agy`)** 打造，核心由以下两份 Markdown 文件构成：

1. **📄 [AGENTS.md](AGENTS.md)**：工作区核心工程纪律规范（产品思维、防幻觉、两击熔断、Ponytail 最小正确改动、TDD 强制验证等 0~9 大纪律）。
2. **🤖 [SUBAGENTS.md](SUBAGENTS.md)**：独立 Subagent 分配与调度手册（Gemini `flash_lite` / `flash` / `pro` 角色矩阵、权限沙盒与派发模版）。

---

## ⚡ 快速接入

在任何工程根目录下直接拉取：

```bash
# 下载核心工程规范 AGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/AGENTS.md -o AGENTS.md

# 下载子 Agent 分配手册 SUBAGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-guidelines/main/SUBAGENTS.md -o SUBAGENTS.md
```

---

## 📄 License

[MIT License](LICENSE)
