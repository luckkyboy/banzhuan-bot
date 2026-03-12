# 踩坑教训

记录使用过程中遇到的问题和解决方案，按严重程度分级。

---

## 🔴 严重

*暂无*

---

## 🟡 中等

### OpenClaw Hook 不支持动态 workspace 创建
- **时间**: 2026-03-12
- **问题**: 尝试用 Hook 为新用户自动创建独立 workspace，但 OpenClaw 没有原生支持
- **解决**: 放弃该方案，保持 Session 隔离 + Workspace 共享模式
- **标签**: #openclaw #hook #workspace

### OpenClaw 配置 denyCommands 无效
- **时间**: 2026-03-12
- **问题**: 主人在远端配置了 `tools.exec.denyCommands`，但该配置项不被 OpenClaw 识别
- **解决**: 运行 `openclaw doctor --fix` 移除无效配置
- **标签**: #openclaw #config

---

## 🟢 轻微

*暂无*