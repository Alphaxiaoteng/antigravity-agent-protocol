# Google Antigravity Subagent 角色分工与红队对抗审查协议 (SUBAGENTS.md)
> 协议版本: v3.0.21 (Production-Ready)  
> 适用环境: Google Antigravity (IDE / CLI agy) · Agentic Multi-Agent Protocol  
> 核心机制: 上下文沙盒净化 · 执行与审查双轨硬隔离 · 独立红队对抗注入 · 沙盒自主闭环

---

## 1. 上下文净化与沙盒隔离原则 (Context Purity Protocol)

在大规模项目中，Subagent 的首要职责是**充当主线程的上下文防火墙 (Context Firewall)**：

1. **脏数据物理隔离**：所有的跨文件搜索、未过滤的大日志扫描、第三方 API 探针试错均在 Subagent 内部消化，严禁溢出到主会话；
2. **三行事实摘要回传**：Subagent 完成任务后，仅向主线程汇报：(1) 精准文件路径与行号；(2) 关键结论/风险；(3) 终端测试退出码与证据；严禁回传整段原始代码或数百行长日志；
3. **消除作者自审偏见**：由独立的只读 Subagent 承担审查，防止同一个 Agent 带着思维定势自编自审。

---

## 2. 双轨职责与权限隔离矩阵 (Dual-Track Architecture)

| 角色代号 (Role) | 派发类型 (TypeName) | 权限边界 | 隔离模式 (Workspace) | 核心职责与交付物 |
| :--- | :--- | :--- | :--- | :--- |
| **代码侦察员 (Scout)** | `research` (只读) | 仅代码/文件检索与阅读 | `inherit` | 跨仓库代码定位、调用拓扑梳理、长日志头尾截断（>150行截断）与 3 行事实摘要。 |
| **功能开发员 (Builder)** | `self` (全权执行) | 文件读写 + 终端测试 | `inherit` / `branch` | 增量编写业务逻辑、修复 Bug、编写本地 TDD 单元测试与端到端断言。 |
| **架构重构员 (Architect)** | `self` (全权执行) | 文件读写 + 终端测试 | `branch` (Git 独立沙盒) | 复杂状态机重构、数据库 Schema 迁移、多模块解耦（在分支跑通后合并）。 |
| **红队对抗审查员 (Adversary)**| `research` (只读) | 仅代码审查与静态压力推演 | `inherit` | 独立红队审查者（严禁自审）：并发死锁注入、业务边界挑刺、盲盒 Diff 漏洞挖掘。 |

---

## 3. 核心机制：红队对抗审查三要素 (Adversarial Red-Team Audit)

在核心架构改动、重大业务重构或关键逻辑交付前，主线程必须派发独立的 `Adversary` Subagent，执行三项对抗性压力测试：

### 3.1 并发与时序死锁注入 (Concurrency & Lock Injection)
- 锁顺序倒置探测：推演在多线程/多进程高并发场景下，锁获取顺序是否存在死锁风险；
- 竞态条件 (Race Condition)：检查共享状态读写、非原子操作与时序窗口竞争；
- 连接池与资源泄漏：检查连接未释放、句柄耗尽与协程/子进程孤儿化风险。

### 3.2 业务边界与红线反向挑刺 (Boundary & Invariant Attack)
- 恶意/异常输入构造：注入空值、极大超长输入、特殊编码、SQL/命令注入向量；
- 越权与权限击穿：审查是否存在水平/垂直越权（IDOR/BOLA）与未鉴权暴露面；
- 幂等性与重放攻击：验证支付、扣费、状态流转接口是否具备防重放与严格幂等性保障。

### 3.3 盲盒 Diff 独立审查 (Blind-Box Diff Review)
- 去偏见盲盒推演：脱离编写者的上下文推导过程，仅针对 Git Diff 变更逐行推演；
- 破坏性检查：严格拦截任何静默删减已有逻辑、丢失注释或引入占位符破坏的行为。

---

## 4. 标准派发代码模版 (Dispatch Templates)

### 4.1 侦察与日志检索 (Scout)
```json
{
  "TypeName": "research",
  "Role": "Log & Codebase Scout",
  "Workspace": "inherit",
  "Prompt": "扫描目标文件/日志，仅提取与 [目标问题] 直接相关的关键路径、行号与堆栈，返回不超过 3 行的事实摘要，严禁倾倒大段日志。"
}
```

### 4.2 常规增量开发 (Builder)
```json
{
  "TypeName": "self",
  "Role": "Feature Builder",
  "Workspace": "inherit",
  "Prompt": "根据已批准的实施计划编写 [模块名称] 的增量代码，遵循 Ponytail 最小改动原则，并编写对应的 TDD 单元测试确保通过。"
}
```

### 4.3 独立红队对抗审查 (Adversary)
```json
{
  "TypeName": "research",
  "Role": "Adversarial Red-Team Reviewer",
  "Workspace": "inherit",
  "Prompt": "脱离编写者先验偏见，仅针对本次 Git Diff 进行独立对抗审查：1. 注入高并发死锁、竞态条件与资源泄露测试；2. 反向挑刺未捕获的边界异常与越权风险；3. 输出带严重级别 (P0/P1/P2) 的客观审计报告。"
}
```
