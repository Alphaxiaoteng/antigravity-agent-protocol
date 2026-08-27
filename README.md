# Google Antigravity Agent Protocol & Best Practices (AGENTS.md)
> 工业级 Google Antigravity 最佳实践、防幻觉、红队对抗审查与闭环交付规范 (v3.0.21)

[![Antigravity Version](https://img.shields.io/badge/Antigravity-v3.0.21%20Ready-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Best Practices](https://img.shields.io/badge/Best%20Practices-Production%20Standard-brightgreen.svg?style=flat-square)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Red-Team Audit](https://img.shields.io/badge/Audit-Adversarial%20Red--Team-red.svg?style=flat-square)](SUBAGENTS.md#2-核心机制红队对抗审查三要素-adversarial-red-team-audit)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

[核心工程协议与最佳实践 (AGENTS.md)](AGENTS.md) | [Subagent 对抗审查手册 (SUBAGENTS.md)](SUBAGENTS.md) | [极速接入](#极速接入-quick-start)

---

## 为什么这是 Antigravity 的最佳实践？

在大规模生产研发环境中，未经规范约束的 AI 往往会产生代码破坏、盲目死循环、上下文爆炸与自审偏见。

Antigravity Agent Protocol (v3.0.21) 沉淀了 5 大工业级最佳实践守则：

### 五大最佳实践核心守则

1. **零占位符破坏最佳实践**：严格拦截 `// ... existing code ...` 等破坏性占位重写，无损保留已有业务逻辑与注释。
2. **两击熔断最佳实践 (Two-Strike Rule)**：同类修复连续失败 2 次强制停止并输出带证据链根因报告，杜绝无意义 Token 消耗与死循环。
3. **Ponytail 最小正确改动最佳实践**：非当前必需不写，严格约束 Diff 范围，禁止顺手重构与过度设计。
4. **红队对抗审查最佳实践 (Adversarial Audit)**：执行轨（Builder）与审查轨（Adversary）权限物理隔离，审查者通过并发死锁注入、越权攻击与盲盒 Diff 推演消除作者先验偏见。
5. **强制闭环交付与 TDD 验证最佳实践**：写完逻辑紧接着执行终端测试与断言探针，严禁未经验证宣称完成。

---

## 核心文档导航

| 核心文件 | 作用与定位 |
| :--- | :--- |
| **[AGENTS.md](AGENTS.md)** | **工作区核心工程协议与最佳实践 (v3.0.21)**：涵盖 0~9 大硬核纪律（产品思维、防幻觉、两击熔断、Ponytail 最小修改、双重服务核验、强制 TDD 验证、Harness 自优化）。 |
| **[SUBAGENTS.md](SUBAGENTS.md)** | **Subagent 角色分工与红队对抗审查手册 (v3.0.21)**：定义了侦察员 (Scout)、开发员 (Builder)、架构师 (Architect) 与红队审查员 (Adversary) 的双轨隔离机制、死锁注入方法论与标准派发 JSON 模版。 |

---

## 极速接入 (Quick Start)

在任何工程根目录下直接拉取最新 v3.0.21 最佳实践协议：

```bash
# 1. 下载核心工程协议与最佳实践 AGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o AGENTS.md

# 2. 下载子 Agent 对抗审查手册 SUBAGENTS.md
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-protocol/main/SUBAGENTS.md -o SUBAGENTS.md
```

---

## License

本项目采用 [MIT License](LICENSE) 许可证开源，欢迎自由引用、Fork 与社区共建。
