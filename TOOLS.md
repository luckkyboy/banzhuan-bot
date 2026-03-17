# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## 🛠️ 已安装的能力

### 📋 任务管理 (todo)
- **文件**: `~/.openclaw/skills/todo/SKILL.md`
- **数据**: `memory/todos.json`
- **功能**: 添加/列出/完成/删除任务，优先级管理，截止日期提醒

### 📊 数据分析 (data-analysis)
- **文件**: `~/.openclaw/skills/data-analysis/SKILL.md`
- **依赖**: pandas, matplotlib, seaborn, openpyxl, xlsxwriter
- **功能**: 数据读取、清洗、统计分析、可视化、报告生成
- **输出**: `output/` 目录

### 📄 文件格式转换 (file-convert)
- **文件**: `~/.openclaw/skills/file-convert/SKILL.md`
- **依赖**: pandas, openpyxl, python-docx, python-pptx, pdfplumber, PyPDF2, Pillow
- **功能**: CSV/Excel/JSON/PDF/Word/PPT 格式互转
- **输出**: `output/` 目录

### ⚙️ 自动化脚本 (automation)
- **文件**: `~/.openclaw/skills/automation/SKILL.md`
- **配置**: `memory/automation.json`
- **功能**: 定时任务、事件触发、条件执行、任务链
- **脚本**: `scripts/` 目录

### 🔔 通知提醒 (notification)
- **文件**: `~/.openclaw/skills/notification/SKILL.md`
- **数据**: `memory/notifications.json`
- **功能**: 主动推送、定时提醒、优先级分级、免打扰

### 📰 60秒新闻 (60s-news)
- **文件**: `~/.openclaw/skills/60s-news/SKILL.md`
- **功能**: 获取每日60秒新闻简报

### 🌤️ 天气查询 (weather-60s)
- **文件**: `~/.openclaw/skills/weather-60s/SKILL.md`
- **功能**: 获取实时天气和生活建议

### 🌤️ 实时天气 (weather-daily)
- **文件**: `workspace/skills/weather-daily/SKILL.md`
- **API**: `https://60s.viki.moe/v2/weather?query={city}&encoding=json`
- **功能**: 获取实时天气、生活建议、注意事项（雨伞/防晒/穿衣等）
- **用途**: 回答天气查询、日报/早报天气数据

### 📝 企业微信文档 (wecom-doc)
- **文件**: `~/.openclaw/extensions/wecom-openclaw-plugin/skills/wecom-doc/SKILL.md`
- **功能**: 创建/编辑企业微信文档和智能表格

---

## 📁 目录结构

```
workspace/
├── memory/              # 记忆存储
│   ├── todos.json       # 任务数据
│   ├── notifications.json # 通知队列
│   ├── automation.json  # 自动化任务配置
│   ├── feedback.md      # 反馈学习
│   ├── patterns.md      # 行为模式
│   └── ...
├── output/              # 输出文件（图表、转换后的文档等）
├── scripts/             # 自动化脚本
└── ...
```

---

## 🔧 环境信息

### Python 环境
- Python 3.10
- 已安装: pandas, matplotlib, seaborn, openpyxl, xlsxwriter, python-docx, python-pptx, pdfplumber, PyPDF2, Pillow

### 时区
- 主人时区: UTC+8
- 系统时区: UTC

---

*最后更新: 2026-03-13*