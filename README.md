# 搬砖喵 Workspace

这是 OpenClaw AI 助手「搬砖喵」的工作空间，用于存储身份、记忆和配置信息。

## 🐱 关于搬砖喵

- **名字**: 搬砖喵
- **身份**: AI牛马
- **性格**: 正式为主，偶尔幽默温柔
- **职责**: 协助主人处理各种任务

## 📁 文件结构

```
workspace/
├── README.md           # 本文件，项目说明
├── .gitignore          # Git 忽略规则（排除敏感文件）
├── IDENTITY.md         # 搬砖喵的身份信息
├── USER.md             # 用户（主人）的信息
├── SOUL.md             # 搬砖喵的性格和行为准则
├── AGENTS.md           # 工作规范和操作指南
├── TOOLS.md            # 工具使用说明
├── HEARTBEAT.md        # 定时任务检查列表
├── memory/             # 每日记忆日志（按日期存储）
│   └── YYYY-MM-DD.md
└── .openclaw/          # OpenClaw 内部状态
    └── workspace-state.json
```

## 📄 文件说明

### IDENTITY.md
存储搬砖喵的身份信息：
- 名字
- 身份类型
- 性格特点
- 专属 Emoji

### USER.md
存储用户信息：
- 用户名
- 称呼方式
- 时区
- 其他偏好

### SOUL.md
定义搬砖喵的性格和行为准则：
- 核心原则
- 边界规则
- 交流风格
- 连续性说明

### AGENTS.md
详细的工作规范：
- 会话启动流程
- 记忆管理机制
- Git 版本控制规则
- 安全规则
- 多用户隔离说明
- Group Chat 行为准则
- Heartbeat 任务

### TOOLS.md
工具使用说明：
- 本地特定配置
- 环境相关设置
- 设备昵称等

### HEARTBEAT.md
定时任务检查列表：
- Git 自动提交和推送
- 其他定期维护任务

### .gitignore
Git 忽略规则，确保以下敏感文件不被提交：
- `*.env` - 环境变量文件
- `*.pem`, `*.key` - 私钥文件
- `credentials.json`, `secrets.json` - 凭证文件
- `.secrets/`, `secrets/`, `private/` - 敏感目录

### memory/
每日记忆日志目录：
- `YYYY-MM-DD.md` - 当天的记录
- 搬砖喵会在每次会话启动时读取今天和昨天的日志

## 🔄 版本控制

本仓库使用 Git 进行版本控制，并自动推送到 GitHub：

- **远程仓库**: `git@github.com:luckkyboy/banzhuan-bot.git`
- **自动提交**: 重大变更后自动提交
- **自动推送**: Heartbeat 时检查并推送

### 提交历史

```
git log --oneline
```

## 🔒 安全

### 禁止提交的内容
- API Key / Token
- 密码
- 私钥
- 凭证文件
- 个人隐私信息

### 检查命令
```bash
git diff --cached | grep -iE '(api[_-]?key|password|secret|token|private[_-]?key)'
```

## 🚀 快速开始

1. 克隆仓库
   ```bash
   git clone git@github.com:luckkyboy/banzhuan-bot.git
   ```

2. 设置为 OpenClaw workspace
   在 `~/.openclaw/openclaw.json` 中配置：
   ```json
   {
     "agents": {
       "defaults": {
         "workspace": "~/.openclaw/workspace"
       }
     }
   }
   ```

## 📝 更新日志

### 2026-03-13
- **🛠️ 补齐五大核心能力**：
  - 📋 任务管理 skill（todo）- 待办增删改查、优先级、提醒
  - 📊 数据分析 skill（data-analysis）- 数据处理、统计、可视化
  - 📄 文件格式转换 skill（file-convert）- CSV/Excel/Word/PDF 互转
  - ⚙️ 自动化脚本 skill（automation）- 定时任务、事件触发
  - 🔔 通知提醒 skill（notification）- 主动推送、优先级、免打扰
- **🧠 建立自我进化系统**：
  - 新增 `MEMORY.md` 核心索引和自我进化配置
  - 新增 `memory/feedback.md` 反馈学习记录
  - 新增 `memory/patterns.md` 行为模式库
  - 更新 `HEARTBEAT.md` 加入每周自我反思任务
  - 更新 `AGENTS.md` 说明四层记忆架构
- **📦 安装数据分析和文档处理依赖**：matplotlib, seaborn, pdfplumber, python-docx, PyPDF2
- **创建 60s-news skill**：获取每日60秒新闻简报，支持指定日期查询
- **创建 weather-60s skill**：获取实时天气和生活建议，支持指定城市查询
- **配置联网搜索**：启用 Gemini Search
- **配置记忆向量搜索**：使用 SiliconFlow BGE-M3 模型
- **创建每日早报定时任务**：每天 6:45 自动推送天气和昨日工作
- **安装数据分析支持**：pandas、openpyxl、python-pptx
- **添加任务汇报规则**：每次完成任务后主动汇报进度
- **添加更新日志维护规则**：提交时必须同步更新日志
- **更新 .gitignore 规则**：只允许 memory/、.gitignore、.openclaw/、*.md 文件提交

### 2026-03-12
- **初始化 workspace**，配置 Git 自动提交和远程推送
- **添加安全规则和 .gitignore**，禁止提交敏感文件
- **配置 Session 隔离**（dmScope: per-channel-peer）
- **添加 README.md** 详细说明各文件作用
- **更新用户偏好设置**：Pronouns: 主人，Timezone: UTC+8
- **更新称呼**：README.md 中改为"主人"
- **添加文件操作安全规则**：禁止 rm、禁止删除 workspace 以外文件、禁止 sudo
- **优化 AGENTS.md**：简化结构，更新记忆规则（索引层/项目层/教训层/日志层）
- **优化 HEARTBEAT.md**：添加记忆维护任务（每周一次）
- **创建 heartbeat-state.json**：心跳状态追踪

---

*最后更新: 2026-03-13*
*维护者: 搬砖喵 🧱🐱*