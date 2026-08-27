# Antigravity 原生 Subagent 双轨协同与对抗审查工作流

本指南阐述 Antigravity 主线程（Main Agent / Coordinator）与执行轨（Executive Track）、独立审查轨（Audit Track）之间的标准化协作协议。

---

## 1. 双轨协同架构

```mermaid
sequenceDiagram
    participant User as 用户 (User)
    participant Main as Antigravity 主线程 (Coordinator)
    participant Scout as 侦察 Subagent (research: flash)
    participant Builder as 执行 Subagent (self: flash)
    participant Adversary as 红队审查 Subagent (research: pro)

    User->>Main: 需求输入
    Main->>Main: 0. 产品思维评估 (JTBD)
    Main->>Scout: invoke_subagent: 快速源码/日志探测 (窗口化截断)
    Scout-->>Main: 回传关键行号与 3 行事实摘要
    Main->>User: 提交实施计划 (Plan Gate)
    User-->>Main: 授权执行
    Main->>Builder: invoke_subagent: 增量编码与本地 TDD 断言编写
    Builder-->>Main: 回传测试证据与 Diff
    Main->>Adversary: invoke_subagent: 盲盒红队对抗审查 (死锁/边界/注入)
    Adversary-->>Main: 回传独立审查审计报告
    Main->>Main: 双重真实核验 (端口监听 + HTTP 200)
    Main->>User: 交付闭环系统与客观证据链
```

---

## 2. 红队对抗审查三要素 (Red-Team Audit)

1. **并发与时序注入**：检查高并发请求下的竞态条件、死锁与连接池泄露。
2. **业务边界挑刺**：检查未处理的异常输入、权限越权（IDOR/BOLA）与边界溢出。
3. **盲盒 Diff 独立核验**：脱离编写者的先验偏见，由配置为 `Model: "pro"` 的独立只读 Subagent（`TypeName: "research"`）仅针对 Git Diff 进行静态推演与压力注入。
