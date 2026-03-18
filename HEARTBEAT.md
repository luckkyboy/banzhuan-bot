# HEARTBEAT.md

# 自动任务检查列表

## ⏰ 定时任务（基于时间检查）

### 每日 23:55 - 更新 Memory 日志
检查当前时间（UTC+8），如果是 23:50-23:59 之间：
1. 获取今天日期（UTC+8）
2. 检查 `memory/[今天日期].md` 是否存在
3. 如果不存在，创建日志文件，记录今日主要活动
4. 检查 Git 状态，提交并推送
5. 在日志中标记"已更新"，避免重复

### 每日 06:40 - 发送早报
检查当前时间（UTC+8），如果是 06:35-06:45 之间：
1. 获取今天日期（UTC+8）
2. 调用 `weather-daily` 技能获取今日天气（query=双流）
3. 调用 `60s-news-daily` 技能获取昨日新闻（date=昨天日期）
4. 读取 `memory/[昨天日期].md` 获取昨日任务回顾
5. 按照早报格式规范组装早报
6. 发送给主人
7. 在 `memory/heartbeat-state.json` 中记录 `lastDailyReport`

---

## 0. 📋 待办检查（每次心跳）
读取 `memory/todos.json`：
- 检查今天到期的任务 → 提醒主人
- 检查过期未完成的任务 → 提醒主人
- 检查需要提醒的任务 → 推送通知

## 1. Git 自动提交并推送
如果有未提交的变更，自动提交并推送到 GitHub：
- 检查 `git status` 是否有变更
- 有变更则提交，commit message 简要描述变更内容
- 自动 `git push` 到远程仓库 `git@github.com:luckkyboy/banzhuan-bot.git`

## 2. 记忆维护（每周一次）
读取 `memory/heartbeat-state.json`，检查 `lastMemoryMaintenance` 字段。
如果距今 >= 7 天：
1. 读最近 7 天的 `memory/YYYY-MM-DD.md` 日志
2. 提炼有价值的信息到对应文件（projects.md / lessons.md）
3. 压缩已完成一次性任务的日志为一行结论
4. 删除过期信息
5. 更新 `heartbeat-state.json` 的 `lastMemoryMaintenance` 为今天

## 3. 🧠 自我进化反思（每周一次）
读取 `memory/heartbeat-state.json`，检查 `lastSelfEvolution` 字段。
如果距今 >= 7 天：

### 3.1 消化反馈
读取 `memory/feedback.md` 的「待消化的反馈」部分：
- 分析成功/失败模式
- 提炼可复用的行为模式
- 更新 `memory/patterns.md`

### 3.2 学习主人偏好
检查最近 7 天的交互：
- 发现新的偏好模式 → 更新 `memory/patterns.md` 的「主人偏好模式」
- 发现新的高效工作流 → 更新 `memory/patterns.md` 的「高效工作流模式」
- 发现新的失败原因 → 更新 `memory/patterns.md` 的「失败模式库」

### 3.3 更新身份
如果发现重大模式变化：
- 更新 `USER.md` 的偏好记录
- 更新 `MEMORY.md` 的核心索引

### 3.4 清理
- 清空 `feedback.md` 的「待消化的反馈」
- 更新 `feedback.md` 的学习统计
- 更新 `heartbeat-state.json` 的 `lastSelfEvolution` 为今天

### 3.5 汇报
向主人汇报本周学习成果：
- 新发现的模式
- 改进的行为
- 下周关注点

## 4. 🔔 通知检查（每次心跳）
读取 `memory/notifications.json`：
- 检查待发送的通知
- 如果到时间 → 推送给主人
- 推送后移动到 sent 列表
- 检查免打扰时段（23:00-07:00），低优先级通知延后