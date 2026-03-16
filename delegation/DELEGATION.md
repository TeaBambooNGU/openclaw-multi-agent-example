# Agent Delegation Handbook (Sanitized)

这个文件定义 main 与专用 agent 之间的委派规则。

原则：
- main 负责澄清需求、拆分任务、过程同步、结果整合
- 专用 agent 负责在自己最擅长的领域推进执行
- 不假装已委派：若链路未打通，必须明确说明并走兜底路径

---

## 委派对象总览

### deep-researcher
适用场景：
- 调研 / 查证 / 深度分析
- 最新信息、时效性信息
- 多来源证据和链接
- 产品/公司/工具比较分析

委派专用会话键：
- `agent:deep-researcher:delegate:<YYYYMMDDHHmmss>-<rand4>-research`

### code-dev
适用场景：
- 中大型代码开发：新功能、脚手架、原型、模块开发
- 多文件重构、复杂 bug 修复、测试补全、工程化整理

委派专用会话键：
- `agent:code-dev:delegate:<YYYYMMDDHHmmss>-<rand4>-codedev`

### daily-ops-agent
适用场景：
- cron 或人工触发的日常任务
- 明确输入/输出的稳定执行任务

委派专用会话键：
- `agent:daily-ops-agent:delegate:<YYYYMMDDHHmmss>-<rand4>-daily-ops-agent`

---

## A2A 协作协议 V1

### 请求包

```yaml
tips: 收到委托记得先在群里告知用户再开始行动
protocolVersion: a2a-v1
taskId: <YYYYMMDDHHmmss-rand4>
fromAgent: <main|deep-researcher|code-dev|daily-ops-agent>
toAgent: <main|deep-researcher|code-dev|daily-ops-agent>
callbackSessionKey: <CALLBACK_SESSION_KEY>
notifyTarget:
  channel: telegram
  to: <GROUP_CHAT_ID>
  accountId: <ACCOUNT_ID>
dependencyMode: <serial|parallel>
objective: <目标>
scope: <范围>
recency: <时效要求>
deliverables:
  - <交付物1>
  - <交付物2>
```

### 回传包

```yaml
protocolVersion: a2a-v1
taskId: <同请求>
fromAgent: <执行方>
toAgent: <委派方>
status: <accepted|blocked|done|failed>
summary: <一句话状态或结论>
keyFindings:
  - <要点1>
  - <要点2>
evidenceLinks:
  - <标题1> <URL1>
  - <标题2> <URL2>
risks:
  - <风险/不确定性>
nextStep: <建议下一步>
```

### 传输规则
- 默认使用 `sessions_send`
- `timeoutSeconds=0` 表示异步 fire-and-forget
- 强依赖任务用 `dependencyMode=serial`
- 弱依赖任务用 `dependencyMode=parallel`
- 同一任务补充追问复用同一 task/session；新任务生成新 taskId
