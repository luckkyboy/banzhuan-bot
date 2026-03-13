# HEARTBEAT.md

# 自动任务检查列表

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