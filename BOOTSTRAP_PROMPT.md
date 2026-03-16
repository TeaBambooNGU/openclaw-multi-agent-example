# Bootstrap Prompt：让另一个 AI 帮你搭建 OpenClaw 多 Agent 系统

你可以把下面这段提示词直接发给另一个 AI，让它按要求创建一个**实际可运行的 OpenClaw 多 agent 环境**。

---

你要帮我从 0 开始搭建一个基于 OpenClaw 的多 agent 协作系统。

目标：
- 创建一个 `main` agent 作为主控/编排者
- 创建多个 specialist agent，至少包括：
  - `deep-researcher`
  - `code-dev`
  - `daily-ops-agent`
- 为每个 agent 建立独立 workspace / prompt
- 在 `~/.openclaw/openclaw.json` 中正确配置 agents、bindings、A2A、session、channels
- 建立统一的 `DELEGATION.md` 协议文件
- 让 `main` 可以通过 A2A 把任务委派给 specialist agent
- specialist agent 可以通过统一协议回传结果给 `main`

请严格按下面要求执行。

## 一、实际目录结构

不要创建“分享包目录结构”，而是创建**真实 OpenClaw 运行目录结构**。

假设主目录为 `~/.openclaw/`，则至少应包含：

```text
~/.openclaw/
├── openclaw.json
├── workspace/
│   ├── SOUL.md
│   ├── AGENTS.md
│   ├── USER.md
│   ├── TOOLS.md
│   ├── MEMORY.md
│   ├── DELEGATION.md
│   └── memory/
│       └── <daily memory files>
├── workspace-deep-research/
│   ├── SOUL.md
│   ├── AGENTS.md
│   ├── USER.md
│   ├── TOOLS.md
│   └── memory/
├── workspace-code-dev/
│   ├── SOUL.md
│   ├── AGENTS.md
│   ├── USER.md
│   ├── TOOLS.md
│   └── memory/
└── workspace-daily-ops/
    ├── SOUL.md
    ├── AGENTS.md
    └── memory/
```

其中：
- `~/.openclaw/openclaw.json` 是主配置文件
- `~/.openclaw/workspace/` 是 `main` 的 workspace
- `~/.openclaw/workspace-deep-research/` 是 `deep-researcher` 的 workspace
- `~/.openclaw/workspace-code-dev/` 是 `code-dev` 的 workspace
- `~/.openclaw/workspace-daily-ops/` 是 `daily-ops-agent` 的 workspace

如果需要分享文档、教程或模板，可以额外新建一个独立目录；但那不是运行时目录结构本身。

## 二、角色设计要求

### main
职责：
- 接用户需求
- 判断是否委派
- 选择 specialist agent
- 跟进执行进度
- 整合结果
- 最终输出给用户

### deep-researcher
职责：
- 调研
- 查证
- 交叉验证
- 引用来源
- 输出结构化研究结论

### code-dev
职责：
- 实现功能
- 修复 bug
- 多文件改动
- 验证构建/测试结果
- 汇报真实改动与风险

### daily-ops-agent
职责：
- 执行 cron 或日常例行任务
- 接受结构化输入
- 控制副作用
- 输出简洁摘要

## 三、Workspace / Prompt 要求

为每个 agent 创建独立 prompt 文件，不要共用同一套人格与协议。

至少要求：
- 每个 agent 有独立 `SOUL.md`
- 每个 agent 有独立 `AGENTS.md`
- `main` workspace 额外包含：
  - `USER.md`
  - `TOOLS.md`
  - `MEMORY.md`
  - `DELEGATION.md`
- specialist workspace 可按需包含：
  - `USER.md`
  - `TOOLS.md`
  - `memory/`

要求：
- `main` 负责 orchestration，而不是单纯转发
- `deep-researcher` 负责 research / evidence / verification
- `code-dev` 负责 coding / validation
- `daily-ops-agent` 负责 structured recurring tasks

## 四、OpenClaw 配置要求

请在 `~/.openclaw/openclaw.json` 中至少正确包含：

1. `agents.list`
2. `bindings`
3. `tools.sessions.visibility`
4. `tools.agentToAgent.enabled`
5. `tools.agentToAgent.allow`
6. `session.agentToAgent.maxPingPongTurns`
7. `session.maintenance`
8. `channels` 中的示例绑定
9. 必要的 `gateway` / `commands` / `messages` / `hooks` 基础配置

注意：
- 不要只写 `agents.list`
- 不要漏掉 A2A 相关 session 配置
- specialist agent 必须使用独立 workspace
- 所有敏感项使用占位符表示，不写真实 token

## 五、委派协议要求

在 `~/.openclaw/workspace/DELEGATION.md` 中定义统一的 A2A 协议。

请求包必须至少包含：
- `taskId`
- `fromAgent`
- `toAgent`
- `callbackSessionKey`
- `notifyTarget`
- `dependencyMode`
- `objective`
- `scope`
- `deliverables`

回传包必须至少包含：
- `taskId`
- `fromAgent`
- `toAgent`
- `status`
- `summary`
- `keyFindings`
- `risks`
- `nextStep`

状态至少支持：
- `accepted`
- `blocked`
- `done`
- `failed`
