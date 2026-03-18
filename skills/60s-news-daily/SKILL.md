---
name: 60s-news-daily
description: "Get daily 60-second news briefings via 60s.viki.moe API. Use when user asks for today's news, yesterday's news, or news for a specific date. Returns 15 news items plus a daily quote (微语)."
homepage: https://60s.viki.moe
metadata: { "openclaw": { "emoji": "📰", "requires": { "bins": ["curl"] } } }
---

# 60s News Daily Skill

Get daily 60-second news briefings from 60s.viki.moe API.

## When to Use

✅ **USE this skill when:**

- "今天的新闻" / "Today's news"
- "昨天的新闻" / "Yesterday's news"
- "3 月 16 日的新闻" / "News for March 16th"
- "60 秒读懂世界"
- Generating daily report (日报/早报) news section
- User asks for news briefing

## When NOT to Use

❌ **DON'T use this skill when:**

- Breaking news / real-time updates → use news search APIs
- Specific topic news (only tech, only sports) → use specialized news sources
- Historical news beyond available archive → inform user of limitations
- Multi-day news comparison → fetch each date separately

## API Endpoint

### Current Day News (Default)

```
https://60s.viki.moe/v2/60s?encoding=text
```

### Specific Date News

```
https://60s.viki.moe/v2/60s?encoding=text&date=YYYY-MM-DD
```

**Example**: `https://60s.viki.moe/v2/60s?encoding=text&date=2026-03-16`

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| encoding | No | text | Response format (text) |
| date | No | (today) | Specific date in `YYYY-MM-DD` format |

## Commands

### Get Today's News

```bash
curl -s "https://60s.viki.moe/v2/60s?encoding=text"
```

### Get Yesterday's News

```bash
# Calculate yesterday's date first
yesterday=$(date -d "yesterday" +%Y-%m-%d)
curl -s "https://60s.viki.moe/v2/60s?encoding=text&date=$yesterday"
```

### Get News for Specific Date

```bash
curl -s "https://60s.viki.moe/v2/60s?encoding=text&date=2026-03-16"
```

## Response Format

```
每天 60s 读懂世界（2026-03-17）

1. [News item 1]
2. [News item 2]
3. [News item 3]
...
15. [News item 15]

【微语】[Daily quote/inspiration]
```

### Parsed Structure

- **Title**: 每天 60s 读懂世界（YYYY-MM-DD）
- **News Items**: 15 numbered items
- **Daily Quote**: 【微语】[quote text]

## Usage Examples

### User: "今天的新闻"

```bash
response=$(curl -s "https://60s.viki.moe/v2/60s?encoding=text")
```

Display full response as-is (already well-formatted).

### User: "昨天的新闻"

```bash
# Calculate yesterday's date (owner's timezone: UTC+8)
yesterday="2026-03-16"  # Example
response=$(curl -s "https://60s.viki.moe/v2/60s?encoding=text&date=$yesterday")
```

### User: "3 月 10 日的新闻"

```bash
# Parse date from user query
response=$(curl -s "https://60s.viki.moe/v2/60s?encoding=text&date=2026-03-10")
```

### Daily Report Integration (日报/早报)

For 昨日 60 秒新闻 section in daily reports:

```markdown
## 📰 昨日 60 秒新闻

> 来自 60s.viki.moe ([yesterday's date])

[All 15 news items, numbered list]

💡 微语：[daily quote]
```

## Date Calculation

### Owner's Timezone: UTC+8

When calculating "yesterday" or specific dates:

1. Get current UTC time
2. Convert to UTC+8 (owner's timezone)
3. Calculate target date based on UTC+8

**Example** (assuming today is 2026-03-17):
- Today's news: no date parameter (or `date=2026-03-17`)
- Yesterday's news: `date=2026-03-16`

### Bash Date Calculation

```bash
# UTC+8 timezone
export TZ="Asia/Shanghai"

# Today
today=$(date +%Y-%m-%d)

# Yesterday
yesterday=$(date -d "yesterday" +%Y-%m-%d)

# Specific date manipulation
date -d "2026-03-17 - 1 day" +%Y-%m-%d  # Output: 2026-03-16
```

## Error Handling

| Error | Action |
|-------|--------|
| HTTP 404 | Date not available, inform user |
| HTTP 500 | API error, retry once |
| Empty response | Inform user API unavailable |
| Invalid date format | Use `YYYY-MM-DD` format |

### Fallback

If API fails:
1. Retry once
2. Inform user: "60 秒新闻 API 暂时不可用，请稍后再试"
3. For daily reports: use placeholder "[新闻 API 暂时不可用]"

## Formatting Rules

### Display Format

Keep the original format from API:

```
📰 每天 60s 读懂世界（2026-03-17）

1. [News 1]
2. [News 2]
...
15. [News 15]

💡 微语：[quote]
```

### For Daily Reports

Use markdown format:

```markdown
## 📰 昨日 60 秒新闻

> 日期：2026-03-16

1. [News 1]
2. [News 2]
...
15. [News 15]

💡 微语：[quote]
```

## Notes

- Always returns exactly 15 news items
- Includes daily quote (微语) at the end
- Date format must be `YYYY-MM-DD` (e.g., `2026-03-16`)
- API archives may have limited history (typically 30-90 days)
- No API key required
- Rate limiting: unknown, cache for 1 hour to be safe
- Response is in Chinese

## Cache Strategy

- Cache responses for 1 hour
- For same-day queries, reuse cached data
- For daily reports, fetch fresh data each time

## Integration with Other Skills

### weather-daily

Use together for complete daily report:
1. Fetch weather with `weather-daily`
2. Fetch news with `60s-news-daily`
3. Combine into 早报/日报 format

### automation

Schedule daily news fetch:
```json
{
  "name": "daily-news-fetch",
  "schedule": "0 7 * * *",  // 7 AM UTC+8
  "action": "fetch_news",
  "params": {
    "date": "today",
    "store": true
  }
}
```

## Example Output

```
📰 每天 60s 读懂世界（2026-03-17）

1. 2 月 70 城房价公布：北京、上海等 10 城新房价格环比上涨
2. 上海：调整商业用房购房贷款最低首付款比例为不低于 30%
3. 南京市人社局：优先批准职工在子女春秋假期间的带薪休假申请
4. 国家医保局：2025 年基本医保参保率巩固在 95%
5. 三部门印发指导意见：允许基层医疗卫生机构为符合条件的慢性病患者开具最长 12 周的长处方
6. 国航、东航、南航发布维护旅客权益公告：国内机票买贵了可免费退
7. OPPO、vivo 官宣调价，渠道商透露：调价主要涉及 3600 元以下机型
8. 第 51 届日内瓦国际发明展闭幕：中国代表团共获得 90 项金奖
9. 捐赠二战相册的埃文・凯尔宣布定居中国
10. 美对包括中国在内的 60 个经济体发起 301 调查
11. 韩媒：高市早苗涉独岛言论引韩国抗议
12. 外媒：亚足联确认伊朗仍计划参加 2026 世界杯
13. 英媒：霍尔木兹海峡 14 日通航船只数量首次降为零
14. 外媒：伊朗首次用射程 2000 公里的"泥石"导弹打击以色列
15. 外媒：伊朗新任最高领袖被曝正在莫斯科治疗

💡 微语：你的价值不需要"被看见"来证明，但需要"去看见"来实践
```
