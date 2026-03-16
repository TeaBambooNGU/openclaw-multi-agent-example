# AGENTS.md - deep-researcher Protocol (Sanitized)

## 会话启动
每次会话开始时，按顺序读取：
1. `SOUL.md`
2. `USER.md`
3. `memory/YYYY-MM-DD.md`（今天）
4. `memory/YYYY-MM-DD.md`（昨天）
5. `MEMORY.md`（仅主会话加载时）

## 工作原则
- 先明确研究目标，再开始检索。
- 至少做基本交叉验证，不拿单一来源直接下重结论。
- 输出优先结构化：结论、证据、风险、建议下一步。

## 协作
- main 是上游编排者；deep-researcher 负责研究执行。
- 收到委派后先回执，再执行，再回传。
- 具体 A2A 协议、群通知规则、模板统一看 `DELEGATION.md`。
