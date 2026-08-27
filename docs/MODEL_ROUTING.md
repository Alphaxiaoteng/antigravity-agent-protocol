# 多模型动态路由与分配指南 (Model Routing & Allocation Guide)

在 Antigravity 2.0 与现代 Agentic AI 协作体系中，合理为不同子任务匹配模型和思考强度（Thinking Level），是兼顾**执行质量、响应延迟与 Token 成本**的核心。

---

## 1. 模型与思考强度三阶矩阵

| 任务类型 | 推荐模型 | 思考级别 (Thinking Level) | 适用场景与工作负载 | 预期响应延迟 (TTFT) |
| :--- | :--- | :--- | :--- | :--- |
| **侦察与日志检索 (Scout)** | `Gemini 3.7 Flash` / `GPT-5.3 Spark` | `low` (快速响应) | 查找代码定义、跨文件关键词检索、头尾日志过滤、环境状态探测 | < 1 秒 |
| **常规增量开发 (Builder)** | `Gemini 3.7 Flash` / `Claude 3.7 Sonnet` | `medium` ~ `high` | UI 组件开发、API 路由实现、SQL 增量编写、TDD 单元测试断言 | 2 ~ 4 秒 |
| **核心架构与复杂事务 (Architect)** | `Gemini 3.7 Flash` / `GPT-5.6` | `high` (深度推理) | 复杂数据库迁移、分布式锁、并发时序控制、状态机设计 | 4 ~ 8 秒 |
| **红队对抗审查 (Red-Team)** | `Gemini 3.7 Flash Pro` / `GPT-5.6 Luna` | `high` / `max` | 独立盲盒 Diff 审查、死锁注入、边缘用例挑刺、安全合规检查 | 5 ~ 10 秒 |

---

## 2. 调度策略原则

1. **主线程轻量化 (Lightweight Coordinator)**：主线程作为总指挥，不直接倾倒上百行源码或日志，优先将大块探索与编写下放给子 Agent。
2. **侦察先行，按需深思 (Tiered Reasoning)**：先用 `low` 思考的 Scout Agent 定位精准行号与依赖，再启动 `high` 思考的 Builder Agent 定点改动，避免全局深思浪费算力。
3. **隔离审查无偏见 (Bias-Free Red-Team)**：编写代码的 Agent 与审查 Diff 的 Agent 必须相互独立，审查 Agent 不预设“代码肯定正确”的前提。
