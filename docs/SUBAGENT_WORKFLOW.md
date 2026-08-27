# 子 Agent 双轨协同与对抗审查工作流 (Subagent Workflow)

本指南阐述主线程总控（Sol）与执行轨（Executive Track）、独立审查轨（Audit Track）之间的标准化协作协议。

---

## 1. 双轨协同架构

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

## 2. 红队对抗审查三要素 (Red-Team Audit)

1. **并发与时序注入**：检查高并发请求下的竞态条件、死锁与连接池泄露。
2. **业务边界挑刺**：检查未处理的异常输入、权限越权（IDOR/BOLA）与边界溢出。
3. **盲盒 Diff 独立核验**：脱离编写者的先验偏见，仅针对 Git Diff 进行静态推演与压力注入。
