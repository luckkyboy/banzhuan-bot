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
