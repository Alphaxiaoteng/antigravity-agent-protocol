# 子 Agent 双轨协同与对抗审查工作流 (Subagent Workflow)

本指南阐述主线程（Coordinator）与执行轨（Executive Track）、审查轨（Audit Track）之间的标准化协作协议。

---

## 1. 双轨协同架构

```mermaid
sequenceDiagram
    participant User as 用户 (User)
    participant Sol as 主线程总控 (Coordinator)
    participant Scout as 侦察 Subagent (Scout)
    participant Builder as 执行 Subagent (Builder)
    participant Adversary as 红队 Subagent (Adversary)

    User->>Sol: 提交需求
    Sol->>Sol: 0. 产品思维评估 (JTBD)
    Sol->>Scout: 派发快速源码/日志探测
    Scout-->>Sol: 回传关键行号与3行精炼事实
    Sol->>User: 提交实施计划 (Plan Gate)
    User-->>Sol: 确认授权
    par 并行或顺序执行
        Sol->>Builder: 执行增量代码编写与 TDD 测试
        Sol->>Adversary: 盲盒审查 Diff 并注入异常/死锁测试
    end
    Builder-->>Sol: 回传测试通过证据
    Adversary-->>Sol: 回传漏洞/边界排查结论
    Sol->>Sol: 双重核验 (监听+HTTP 200)
    Sol->>User: 交付闭环系统与客观证据
```

---

## 2. 红队对抗审查三要素 (Red-Team Audit)

1. **并发与时序注入**：检查高并发请求下的竞态条件、死锁与连接池泄露。
2. **业务边界挑刺**：检查未处理的异常输入、权限越权（IDOR/BOLA）与边界溢出。
3. **盲盒 Diff 独立核验**：脱离上下文偏见，仅针对代码变动（Git Diff）进行静态推演。
