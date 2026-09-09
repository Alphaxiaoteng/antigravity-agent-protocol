# Antigravity Agent Protocol
> 现代轻量级 Google Antigravity 智能体工程协议 (AGENTS.md)

[![Antigravity](https://img.shields.io/badge/Antigravity-Ready-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Standard](https://img.shields.io/badge/Standard-Minimal%20%26%20Surgical-brightgreen.svg?style=flat-square)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

一个文件，彻底解决 AI 编程时的 **上下文爆炸**、**死循环盲目重试**、**占位符吞代码** 与 **注意力漂移**。

---

## 极速接入 (Quick Start)

在项目根目录执行一行命令即可生效：

```bash
curl -sSL https://raw.githubusercontent.com/alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o AGENTS.md
```

*或者配置为全局规则（对本机所有项目生效）：*
```bash
mkdir -p ~/.gemini/rules
curl -sSL https://raw.githubusercontent.com/alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o ~/.gemini/rules/antigravity.md
```

---

## 核心约束机制

| 核心防线 | 规则精髓 | 解决痛点 |
| :--- | :--- | :--- |
| **上下文低水位** | 脏日志后台消化，长输出窗口截断，三元状态锚定 | 避免 Prompt 前缀缓存击穿与长程注意力漂移 |
| **精准局部手术** | 严格 Ponytail 最小改动，严禁 `// ... existing code ...` | 杜绝顺手重构引入回归 Bug，保护已有代码无损 |
| **两击强制熔断** | 同类错误连续失败 2 次立即停机交排查报告 | 彻底打断 AI 死循环盲试与 Token 浪费 |
| **确定性闭环交付** | 代码写完强制跑测试/探针，服务双重验证 (监听+200) | 拒绝口头假完成与不可用半成品 |

---

## 完整协议内容预览

```markdown
# Antigravity Agent Protocol (AGENTS.md)

> 现代轻量化 Google Antigravity 工作区工程协议 · 极简 · 防漂移 · 闭环交付

## 1. 上下文低水位与防漂移 (Context Purity)
- **低水位原则**：主会话严禁倾倒未过滤的长日志、堆栈或大段源码；单次输出 >100 行强制截取头尾并提炼 3 行事实摘要。
- **状态锚定**：长任务思考中仅维护极简三元状态 `[已完成] -> [当前执行] -> [阻断风险]`，严禁复述历史细节。
- **子沙盒隔离**：大范围源码检索、依赖查证、压力审查交由只读 Subagent（`research`）独立消化，仅回传结论与行号。

## 2. 精准改动与防破坏 (Surgical Diffs)
- **Ponytail 准则**：只改当前目标必需的最少代码；严禁顺手重构未相关模块、预建多余抽象或修改无关注释。
- **零占位符铁律**：严禁使用 `// ... existing code ...` 或 `// TODO` 破坏已有逻辑。
- **思维制动**：识别出最简有效路径立即停止推演并输出行动，不构想极端罕见边缘分支。

## 3. 防幻觉与两击熔断 (Two-Strike Rule)
- **查源码不脑补**：改动前查证真实文件与依赖定义，严禁凭空臆造 API。
- **两击强制熔断**：同类报错连续修复失败 2 次，强制停止盲试，输出带真实证据链的根因诊断报告。

## 4. 闭环验证与确定性交付 (Deterministic Delivery)
- **写完强制自测**：代码改动完成后严禁直接结束 Turn，必须在终端执行测试断言或最小探针跑绿。
- **服务双重核验**：Web 服务交付必须确认进程正常监听且 HTTP 200 核心资源可渲染。
- **事实证据交付**：系统状态与结论以终端真实退出码与客观日志为唯一依据，无证据不宣称完成。
```

---

## License

[MIT License](LICENSE)
