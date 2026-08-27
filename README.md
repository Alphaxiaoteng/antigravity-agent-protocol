# Google Antigravity Agent Protocol & Best Practices (AGENTS.md)
> 工业级 Google Antigravity 最佳实践、防幻觉、红队对抗审查与闭环交付规范 (v3.0.21)

[![Antigravity Version](https://img.shields.io/badge/Antigravity-v3.0.21%20Ready-blue.svg?style=flat-square&logo=google)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Best Practices](https://img.shields.io/badge/Best%20Practices-Production%20Standard-brightgreen.svg?style=flat-square)](https://github.com/Alphaxiaoteng/antigravity-agent-protocol)
[![Red-Team Audit](https://img.shields.io/badge/Audit-Adversarial%20Red--Team-red.svg?style=flat-square)](SUBAGENTS.md#2-核心机制红队对抗审查三要素-adversarial-red-team-audit)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)

[核心工程协议 (AGENTS.md)](AGENTS.md) | [Subagent 对抗审查手册 (SUBAGENTS.md)](SUBAGENTS.md)

---

## 极速接入与使用 (Quick Start)

无需复杂安装，在终端执行以下命令即可立即将协议挂载至您的项目中：

### 选项 1：项目级规则（推荐，放入当前工程根目录）
```bash
# 1. 下载核心工程协议 AGENTS.md (v3.0.21)
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o AGENTS.md

# 2. 下载子 Agent 对抗审查手册 SUBAGENTS.md (v3.0.21)
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-protocol/main/SUBAGENTS.md -o SUBAGENTS.md
```

### 选项 2：Antigravity 全局规则（对本机所有项目全局自动生效）
```bash
mkdir -p ~/.gemini/rules
curl -sSL https://raw.githubusercontent.com/Alphaxiaoteng/antigravity-agent-protocol/main/AGENTS.md -o ~/.gemini/rules/antigravity_protocol.md
```

### 运行机制说明
- 当启动 Antigravity IDE 或 CLI (`agy`) 时，系统会自动从当前工作目录向上遍历至 Git 根目录，自动发现并加载 `AGENTS.md`。
- 协议条款会被直接编译注入为 `<user_rules>`，其优先级高于模型默认偏好与通用系统提示。

---

## 真实痛点：未经约束的 AI 编码助手如何毁掉工程？

在大规模生产研发环境中，直接使用原生 AI 编写复杂业务代码时，往往会遭遇以下六大致命问题：

1. **占位符破坏已存代码**：AI 在重写大文件时为了省事，随手写下 `// ... existing code ...` 或 `// TODO`，导致数百行核心业务逻辑和注释被直接抹除。
2. **盲目死循环重试**：遇到环境依赖或逻辑报错时，AI 不查根因，连续盲目重试 5 次以上，不仅毫无进展，还白白耗费巨额 Token。
3. **自编自审偏见**：由编写代码的同一个 Agent 负责检查自身逻辑，由于“作者认知偏见”，极易对并发死锁、时序竞态、权限越权等隐蔽缺陷视而不见。
4. **上下文爆炸与注意力漂移**：一次性读取几百行终端日志或大文件，将主会话上下文填满脏数据，导致前缀缓存（Prompt Caching）命中率归零，AI 开始“胡言乱语”。
5. **假完成与空城计**：未运行真实测试断言，未核验服务监听端口，仅仅因为代码写完就自信宣称“已成功部署上线”。
6. **顺手重构与过度设计**：用户要求修改一个参数，AI 顺手把上下游数据结构和未相关函数全部重构，引入海量不可控回归 Bug。

---

## 协议解决方案与核心防御体系

Antigravity Agent Protocol (v3.0.21) 针对上述痛点建立了严密的工程防御标准：

| 痛点场景 | 传统 AI 表现 | Antigravity 协议治理方案 |
| :--- | :--- | :--- |
| **代码写入** | 随手写占位符，抹掉已有代码 | **零占位符铁律**：严禁 `// ... existing code ...`，保护现有注释与逻辑无损。 |
| **错误修复** | 盲目重试 5+ 次陷入死循环 | **两击熔断机制**：同类错误失败 2 次强制中止，输出带证据链根因排查报告。 |
| **代码改动** | 顺手重构上下游，过度工程 | **Ponytail 规范**：非当前必需不写，识别最简路径立即停止推演。 |
| **代码审查** | 自编自审，忽视严重死锁 | **双轨对抗审查**：执行轨 (Builder) 与审查轨 (Adversary) 物理隔离，强制死锁与越权注入。 |
| **上下文管理** | 倾倒上百行日志，缓存失效 | **窗口化截断**：超过 150 行日志实施头尾截断，仅提炼 3 行事实摘要。 |
| **质量验收** | 口头宣称完成，不跑测试 | **强制 TDD 闭环**：代码编写后必须紧接真实测试断言与服务双重核验（监听+HTTP 200）。 |

---

## 核心工程纪律 (0-9 大原则解析)

### 0. 第一性原理 (First Principles)
- **产品思维先行**：以用户真实场景与核心痛点（JTBD）为第一评估标准，拒绝纯技术自嗨。
- **事实高于推测**：以终端退出码与客观日志为唯一判定标准，未亲眼见证证据前严禁宣称完成。
- **最小作用量 (Ponytail)**：如无必要勿增实体，以最简架构与最小代码改动解决问题。
- **确定性闭环交付**：交付前完成全链路可达性验证，严禁静态假 Demo 冒充真实系统。

### 1. 诚信验证与防幻觉 (Integrity & Anti-Hallucination)
- **状态锚定**：长任务维护 `[已完成] -> [当前执行] -> [阻断风险]` 极简状态，防止注意力漂移。
- **工具输出窗口化**：单次超过 150 行的输出头尾截断，提炼 3 行事实摘要，维持低上下文水位。
- **查源码不臆造**：改动前优先检索真实依赖定义，严禁凭空虚构不存在的 API。
- **两击熔断机制**：连续失败 2 次立即暂停，输出带证据链排查报告。

### 2. 最小正确改动 (Ponytail Standard)
- 非当前必需不写，优先局部增量修改，禁止静默删减已有逻辑。
- 思维停止制动：识别出最简有效路径后立即停止推演，禁止构想极端边缘假设。

### 3. 需求前置对齐 (Alignment Gate)
- 遇到需求模糊或架构分叉，强制使用 `ask_question` 发起 1~3 个精简结构化选项对齐。

### 4. 计划与确认边界 (Plan Boundaries)
- 涉及多文件修改先出计划停顿待确认，明确授权后开始写操作。

### 5. 本地服务交付规范 (Service Delivery)
- Web 服务必须完成双重真实核验：(1) 进程正常监听；(2) HTTP 200 核心资源完整渲染。

### 6. Subagent 双轨对抗审查 (Adversarial Audit)
- 详见 [SUBAGENTS.md](SUBAGENTS.md)。主线程负责总控，执行轨负责生产代码，审查轨负责盲盒审计与异常死锁注入。

### 7. 安全与费用底线 (Security Guardrails)
- 永不读取/记录密钥凭据，严禁执行破坏性命令（`git reset --hard` / `rm -rf`），费用调用需审批。

### 8. 强制自动检验与 TDD (Mandatory TDD)
- 代码编写完成后严禁直接结束 Turn，必须紧接着在终端执行测试断言并获得真实通过证据。

### 9. Harness 运行时自优化 (Self-Optimization)
- 两击故障自熔断、长程会话前缀缓存自压缩、批量任务完成后自回收闲置进程、规则行数严格控制在 80 行以内。

---

## Subagent 角色分工与红队对抗审查

在 [SUBAGENTS.md](SUBAGENTS.md) 中，智能体被物理隔离为两大阵营：

### 执行轨 (Executive Track)
- **代码侦察员 (Scout)**：只读沙盒（`TypeName: "research"`），负责跨仓库定位与大日志过滤。
- **功能开发员 (Builder)**：全权沙盒（`TypeName: "self"`），负责增量业务代码编写与本地 TDD 测试。
- **架构重构员 (Architect)**：独立 Git 分支沙盒（`Workspace: "branch"`），负责复杂重构与时序改造。

### 审查轨 (Audit Track - 红队对抗)
- **红队审查员 (Adversary)**：只读独立审查（`TypeName: "research"`），严禁带编写偏见，专职执行三项对抗测试：
  1. **并发死锁注入**：推演高并发场景下的锁顺序倒置与时序竞态；
  2. **业务边界挑刺**：注入恶意异常输入、测试水平/垂直越权漏洞；
  3. **盲盒 Diff 审查**：脱离编写者上下文，逐行推演 Git Diff 变动风险。

---

## 核心文档导航

| 核心文件 | 版本 | 作用与定位 |
| :--- | :--- | :--- |
| **[AGENTS.md](AGENTS.md)** | `v3.0.21` | 工作区核心工程协议与最佳实践（0~9 大硬核纪律）。 |
| **[SUBAGENTS.md](SUBAGENTS.md)** | `v3.0.21` | Subagent 角色分工与红队对抗审查手册（双轨隔离与派发模版）。 |

---

## 常见问题解答 (FAQ)

### Q1: 为什么要求 Agent 在修复失败 2 次后强制熔断（Two-Strike Rule）？
根据工程统计，当 AI 在相同错误上连续尝试 2 次失败后，盲目进行第 3 次重试的成功率低于 8%，且极易产生幻觉与代码破坏。两击熔断机制强制 AI 停止无意义的重试循环，回退并输出带完整证据链的根因诊断报告。

### Q2: 什么是 Ponytail 最小正确改动规范？
Ponytail 规范要求 AI 每次只改动满足当前目标的最少代码行数，严禁在未经授权的情况下顺手重构上下游代码、修改无关注释或预建过度抽象架构。

### Q3: 为什么必须对工具输出实施头尾窗口截断？
大段原始日志（如构建错误、海量搜索结果）会迅速填满主会话的 Context 窗口，导致 Google Antigravity 的前缀缓存（Prompt Caching）失效并产生注意力漂移。头尾窗口截断仅提炼 3 行事实摘要，能大幅提升后续推理的精准度。

---

## License

本项目采用 [MIT License](LICENSE) 许可证开源，欢迎自由引用、Fork 与社区共建。
