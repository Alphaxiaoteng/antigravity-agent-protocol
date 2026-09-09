# Antigravity Agent Protocol
> 现代自适应 Google Antigravity 智能体工程协议 (AGENTS.md)

[![Antigravity](https://img.shields.io/badge/Antigravity-Ready-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Standard](https://img.shields.io/badge/Standard-Adaptive%20%26%20Practical-brightgreen.svg?style=flat-square)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

通用、轻量、非硬编码的 Antigravity 智能体工作约定，自适应多语言与复杂工程场景，解决 AI 编码时的 **上下文膨胀**、**顺手过度重构**、**破坏性覆盖** 与 **口头假完成**。

---

## 极速接入 (Quick Start)

在项目根目录下执行一行命令：

```bash
curl -sSL https://raw.githubusercontent.com/alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o AGENTS.md
```

*全局生效配置（对本机所有项目生效）：*
```bash
mkdir -p ~/.gemini/rules
curl -sSL https://raw.githubusercontent.com/alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o ~/.gemini/rules/antigravity.md
```

---

## 核心约定一览

| 维度 | 核心原则 | 说明 |
| :--- | :--- | :--- |
| **目标与执行** | 实用优先、通用抽象 | 解决真实使用问题，查证真实代码不臆造接口，大日志走 Subagent 沙盒。 |
| **需求对齐** | 模糊对齐、明确自主 | 需求含糊或架构分叉时对齐意图；明确任务按 Ponytail 最小改动自主闭环。 |
| **改动规范** | Ponytail 最小正确改动 | 优先复用与标准库，修共同根因，严禁占位符破坏代码，两击失败查根因。 |
| **闭环交付** | 匹配风险的真实验证 | 代码改完附测试/探针证据，Web 服务核验监听与 HTTP 200，无假完成。 |

---

## 协议完整预览

```markdown
# Antigravity Agent Protocol (AGENTS.md)

> 通用自适应工作区工程协议 · 解决真实问题 · 最小改动 · 需求对齐 · 闭环交付

---

## 1. 目标与执行原则
- **实用优先**：先解决真实使用问题，再考虑技术形式；追求最小、直接、可维护，不做无关重构。
- **查证不臆造**：修改前先读取真实文件、调用关系与依赖定义，不虚构接口，不凭记忆断言。
- **通用抽象自适应**：规则与逻辑保持通用抽象，自适应多语言与多场景，避免硬编码特例污染上下文。
- **保持上下文纯净**：大范围源码阅读、依赖检索与重日志分析交由 Subagent 沙盒独立消化；回传提炼核心结论与行号，主会话保持低水位。

## 2. 需求对齐门禁 (Alignment Gate)
- **模糊时对齐**：用户需求表述不明确、有多重设计分支或可能影响核心架构时，不盲目猜测执行，主动通过结构化选项对齐意图后再行动。
- **明确时自主闭环**：范围明确的任务直接自主推进闭环；客观事实自行查证，不就可自查事项向用户反问。
- **计划与边界**：多文件架构改造先给出简明计划并待确认；单点或局部修复直接完成。

## 3. 最小正确改动 (Ponytail)
- **选择优先级**：按“无需新增 → 复用现有 → 标准库/原生能力 → 已安装依赖 → 最小代码”的顺序选择方案。
- **修共同根因**：修 Bug 查找共同根因，不在每个调用点盲目打补丁；连续失败两次停止盲试，分析真实阻断原因。
- **保护现有逻辑**：保留用户已有改动，不删除无关内容，严禁使用占位代码（如 `// ... existing code ...`）替换现有业务逻辑。

## 4. 闭环验证与确定性交付
- **风险匹配验证**：修改后运行与风险匹配的测试或最小真实探针，以客观退出码与真实日志作为交付依据。
- **Web 服务交付**：Web 项目交付前至少确认：服务正常监听、HTTP 200、核心页面可渲染。
- **安全与环境守则**：不读取、记录或修改敏感凭据；不执行破坏性重置命令；外部动态以实时检索为准。
```

---

## License

[MIT License](LICENSE)
