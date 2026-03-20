# OpenClaw Multi-Agent Orchestration Template

一个基于 OpenClaw 的多 agent 协作模板，用来搭建 **主控 + specialist agents** 的任务编排系统。\
它的目标不是“多开几个 bot”，而是让一个 `main` agent 负责任务理解、委派、回收和整合，让不同 specialist agent 在各自擅长的领域执行任务。

> 简单说：**main 负责 orchestration，specialists 负责 execution。**

B站讲解视频link: https://www.bilibili.com/video/BV1nDw2zQEww/
---

## Why this exists

很多“多 agent”方案最后都会退化成两种情况：

1. 多个 bot 平级说话，职责边界不清
2. 所谓委派其实只是自然语言转发，无法稳定复用

这个模板想解决的是：

- 如何把 `main` 和 specialist agent 真正分层
- 如何让 agent-to-agent（A2A）委派可追踪、可回传、可约束
- 如何让协作结果最终回流到 `main`，而不是散落在各个会话里
- 如何让这套系统可以被别人复现，而不是只在一个人的环境里勉强可用

---

## What you get

这个模板包含一套可复用的最小设计：

- **角色分层**：`main`、`deep-researcher`、`code-dev`、`daily-ops-agent`
- **Prompt 分层**：每个 agent 独立 workspace / prompt 文件
- **A2A 协议**：统一的 delegation request / response 结构
- **OpenClaw 配置样例**：包含 agent、bindings、A2A、session、channel 等关键配置
- **教学文档**：解释如何从 0 搭建一套类似系统
- **Bootstrap prompt**：可直接发给另一个 AI 生成骨架

---

## Core idea

这套系统依赖四层结构：

### 1. Role layer
定义谁做什么：
- `main`: orchestration / routing / result integration
- `deep-researcher`: research / verification / evidence gathering
- `code-dev`: implementation / bug fixing / code changes / validation
- `daily-ops-agent`: cron-driven or structured recurring tasks

### 2. Prompt layer
不同 agent 必须有不同的：
- role definition
- behavior constraints
- output expectations
- workspace context

### 3. Config layer
在 `openclaw.json` 中配置：
- `agents.list`
- `bindings`
- `tools.agentToAgent`
- `tools.sessions.visibility`
- `session.agentToAgent.maxPingPongTurns`
- `session.maintenance`

### 4. Protocol layer
定义统一任务包，而不是只靠自然语言：
- `taskId`
- `fromAgent` / `toAgent`
- `callbackSessionKey`
- `notifyTarget`
- `objective`
- `scope`
- `deliverables`

---

## Repository structure

```text
multi-agent-share-pack/
├── README.md
├── README_OPEN_SOURCE.md
├── ONE_PAGE.md
├── TEMPLATE_TREE.md
├── BOOTSTRAP_PROMPT.md
├── delegation/
│   └── DELEGATION.md
├── config/
│   └── openclaw.sanitized.json
└── prompts/
    ├── main/
    ├── deep-researcher/
    ├── code-dev/
    └── daily-ops-agent/
```

---

## Start here

如果你第一次接触这套模板，建议按这个顺序看：

1. `ONE_PAGE.md` — 快速理解这套系统是什么
2. `config/openclaw.sanitized.json` — 看关键配置长什么样
3. `delegation/DELEGATION.md` — 看 A2A 协议如何定义
4. `prompts/` — 看各 agent 如何分角色与边界
5. `BOOTSTRAP_PROMPT.md` — 需要时直接喂给另一个 AI 生成项目骨架

---

## Minimum requirements for a real multi-agent setup

如果你想要的不是“看起来像多 agent”，而是真的能稳定跑，至少需要：

- separate agents with separate workspaces
- channel/account bindings
- A2A enabled at tool level
- session-level ping-pong limits
- a reusable delegation protocol
- result integration by `main`
- cleanup strategy for task-scoped sessions

缺任何一个，都可能让系统退化成：
- 单 agent 假装多 agent
- 多 bot 并列发言
- 委派不可追踪
- agent 间来回打转
- 结果无法统一回收

---

## Design principles

### Main is not a relay bot
`main` 不应该只是转发 specialist 的原始输出。\
它应该负责：
- deciding when to delegate
- checking whether results are complete
- correcting conflicts against verified facts
- producing the final human-facing answer

### Specialists should stay specialized
研究 agent 不该像主控一样聊天，开发 agent 不该接过多研究任务，ops agent 不该随意扩展任务范围。

### A2A needs both config and protocol
只打开 `tools.agentToAgent.enabled` 不够。\
你还需要：
- session visibility
- session ping-pong limits
- maintenance policy
- callback / notify conventions
- reusable request/response schema

### Session hygiene matters
如果每次委派都堆成永久上下文，系统很快会混乱。\
任务级 session + 完成后清理，是比较稳的做法。

---

## Included files

### `ONE_PAGE.md`
一页讲清架构和流程。

### `BOOTSTRAP_PROMPT.md`
让AI帮你创建的提示语。

### `delegation/DELEGATION.md`
统一委派协议模板。

### `config/openclaw.sanitized.json`
脱敏配置示例，重点展示：
- agent definitions
- bindings
- A2A-related settings
- session limits
- channel layout

### `prompts/`
脱敏后的 agent prompt 示例。

---

## Typical use cases

- main delegates market/tech/product research to a research specialist
- main delegates medium/large coding tasks to a code specialist
- main delegates cron-triggered routines to an ops specialist
- main integrates results and answers the user with one coherent output

---

## What this repo is not

这不是：
- 一个可直接运行的完整产品仓库
- 一个通用万能 prompt 包
- 一个“只开几个 bot 账号就行”的速成方案

这是一个：
- **架构模板**
- **配置参考**
- **协议示例**
- **Prompt 分层样例**
- **教学型复现包**

---

## If you want to adapt this template

你可以很容易替换 specialist 角色，比如：
- `sales-agent`
- `qa-agent`
- `designer-agent`
- `support-agent`
- `content-agent`

但建议保留这些核心结构：
- `main` 负责 orchestration
- specialist 独立 workspace
- 明确的 A2A 协议
- session 限制与清理策略
- 最终结果回流给 `main`

---