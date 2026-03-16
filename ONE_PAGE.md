# OpenClaw 多 Agent 协作：一页讲清版

## 这是什么

这是一套 **主控 + 专业执行位** 的多 agent 协作方式。

它不是让多个 bot 平级乱聊，而是让：
- 一个 **main agent** 负责理解需求、分派任务、整合结果
- 多个 **specialist agent** 各自在擅长的领域执行

一句话：

> **main 负责调度与把关，专用 agent 负责把具体事情做出来。**

---

## 角色分工

### 1. main
负责：
- 接用户需求
- 判断是否需要委派
- 选择委派对象
- 同步进度
- 整合结果
- 最终对人输出

### 2. deep-researcher
负责：
- 调研
- 查证
- 交叉验证
- 证据整理
- 对比分析

适合：最新信息、研究问题、多来源结论。

### 3. code-dev
负责：
- 中大型开发
- 复杂 bug 修复
- 多文件改动
- 工程化整理
- 测试补全

适合：main 不该手搓完成的开发任务。

### 4. daily-ops-agent
负责：
- cron 驱动任务
- 日常例行任务
- 批处理 / 汇总 / 巡检

适合：输入明确、输出稳定的 daily task。

---

## 怎么委派

不是简单说一句“你去做”，而是尽量发一个 **结构化任务包**。

任务包通常包含：
- `taskId`：任务 ID
- `fromAgent` / `toAgent`：谁委派谁
- `callbackSessionKey`：结果回传到哪里
- `notifyTarget`：要不要同步给群聊/外部会话
- `objective`：任务目标
- `scope`：范围和边界
- `deliverables`：交付物要求

这样做的意义是：
- 任务可追踪
- 结果可回收
- 进度可同步
- 协作可复用

---

## 实际工作流

### Step 1：用户把需求交给 main
用户通常只需要和 main 说话。

### Step 2：main 判断要不要委派
- 调研类 → `deep-researcher`
- 开发类 → `code-dev`
- 日常任务类 → `daily-ops-agent`
- 简单任务 → main 自己做

### Step 3：main 发协议化委派包
main 把目标、范围、交付物、回传地址一起交给执行方。

### Step 4：执行方回执并推进
执行 agent 一般会回：
- `accepted`：已接手
- `blocked`：遇到阻塞
- `done`：已完成
- `failed`：执行失败

### Step 5：main 做最终整合
main 不是转发机，而是最后一道质量关：
- 看结果是否完整
- 必要时继续追问
- 如果与已验证事实冲突，先纠偏
- 再用人类易读的话讲出来

### Step 6：任务收尾后清理委派 session
任务结束后，任务级专用会话会被清理，避免上下文长期堆积。

---

## 为什么这样设计

### 1. 分工清楚
main 不需要自己做所有事，专用 agent 也不需要直接面对全部上下文。

### 2. 更容易扩展
只要协议一致，就可以继续增加新的 specialist agent。

### 3. 结果更稳
main 负责最终把关，避免执行结果直接未经检查地暴露给用户。

### 4. 群协作更自然
通过 `notifyTarget`，接手、阻塞、完成这些状态可以同步回群里，人能看见进度。

### 5. 会话不容易串味
每次委派默认用专用 session key，做完再清理。

---

## 这套系统的关键，不是“多开几个 bot”

它真正的核心是三件事：

1. **角色分层**：main 编排，specialist 执行
2. **协议委派**：不是随口转发，而是结构化任务包
3. **结果回收**：执行结果要回到 main，由 main 整合后再对外输出

所以更准确地说，它是：

> **一个轻量的 agent orchestration + A2A protocol + session routing + human-visible notification 方案。**

---


## 相关文件

- 详细说明：`README.md`
- 委派协议：`delegation/DELEGATION.md`
- 脱敏配置：`config/openclaw.sanitized.json`
- 各 agent 提示语：`prompts/`
