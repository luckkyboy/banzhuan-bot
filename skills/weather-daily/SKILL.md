---
name: weather-daily
description: "Get real-time weather and daily suggestions via 60s.viki.moe API. Use when user asks about current weather, today's weather, or needs weather-based suggestions (umbrella, sunscreen, etc.). Also used for daily report weather data."
homepage: https://60s.viki.moe
metadata: { "openclaw": { "emoji": "🌤️", "requires": { "bins": ["curl"] } } }
---

# Weather Daily Skill

Get current weather conditions and AI-powered daily suggestions from 60s.viki.moe API.

## When to Use

✅ **USE this skill when:**

- "现在天气如何？" / "What's the weather now?"
- "今天天气怎么样？" / "How's the weather today?"
- "需要带伞吗？" / "Do I need an umbrella?"
- "今天需要防晒吗？" / "Do I need sunscreen today?"
- User asks about weather-based suggestions
- Generating daily report (日报/早报) weather section

## When NOT to Use

❌ **DON'T use this skill when:**

- Multi-day forecast beyond today → use weather skill with forecast
- Historical weather data → use weather archives
- Weather for multiple cities in one query → query one city at a time

## API Endpoint

```
https://60s.viki.moe/v2/weather?query={city}&encoding=json
```

**Example**: `https://60s.viki.moe/v2/weather?query=双流&encoding=json`

### Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| query | No | 双流 | City name (Chinese or English). Default is 双流 (owner's location). Examples: `query=北京`, `query=深圳`, `query=上海` |
| encoding | No | json | Response format (json) |

### Response Format

```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "city": "上海市",
    "weather": "晴",
    "temp": "25°C",
    "temp_min": "20°C",
    "temp_max": "28°C",
    "humidity": "60%",
    "wind": "东南风 3 级",
    "aqi": "50",
    "aqi_level": "优",
    "tips": {
      "clothing": "建议穿薄衬衫",
      "umbrella": "无需带伞",
      "sunscreen": "需要防晒",
      "exercise": "适宜户外运动"
    },
    "notice": "今天天气晴朗，紫外线较强，注意防晒"
  }
}
```

## Commands

### Get Weather for Default City (双流)

```bash
curl -s "https://60s.viki.moe/v2/weather?query=双流&encoding=json"
```

### Get Weather for Specific City

```bash
# Default city (双流)
curl -s "https://60s.viki.moe/v2/weather?query=双流&encoding=json"

# Other cities
curl -s "https://60s.viki.moe/v2/weather?query=北京&encoding=json"
curl -s "https://60s.viki.moe/v2/weather?query=深圳&encoding=json"
curl -s "https://60s.viki.moe/v2/weather?query=上海&encoding=json"
```

### Parse and Display Weather

```bash
response=$(curl -s "https://60s.viki.moe/v2/weather?query=双流&encoding=json")
city=$(echo "$response" | jq -r '.data.city')
weather=$(echo "$response" | jq -r '.data.weather')
temp=$(echo "$response" | jq -r '.data.temp')
notice=$(echo "$response" | jq -r '.data.notice')

echo "🌤️ $city 天气：$weather $temp"
echo "💡 $notice"
```

## Response Handling

### Success (code: 200)

Extract and display:
- City name
- Weather condition
- Temperature (current, min, max)
- Humidity
- Wind
- AQI and level
- Tips (clothing, umbrella, sunscreen, exercise)
- Notice (important daily reminder)

### Error Handling

| Error | Action |
|-------|--------|
| code: 500 | API error, fallback to wttr.in or inform user |
| Network error | Retry once, then fallback |
| Empty response | Inform user API unavailable |

### Fallback

If API fails:
1. Retry once with `--insecure` flag
2. Fallback to `wttr.in` API
3. Inform user if all sources fail

## Usage Examples

**City Detection Rule:**
- If user mentions a specific city (北京，深圳，上海，etc.) → use `query={city}`
- If no city mentioned → use default `query=双流`

### User: "现在天气如何？" / "双流天气"

```bash
response=$(curl -s "https://60s.viki.moe/v2/weather?query=双流&encoding=json")
```

Response format:
```
🌤️ 双流区 | 多云 14°C
🌡️ 体感：15°C | 湿度：72%
💨 风力：西南风 2 级
📊 空气质量：良 (AQI: 80)

💡 今日提示：
• 👕 穿衣：早晚温差大，建议薄外套
• ☂️ 雨伞：多云天气，可不带伞
• 🧴 防晒：紫外线中等，建议防晒
• 🏃 运动：适宜户外运动

⚠️ 注意：今天云量较多，气温适中，适合户外活动
```

### User: "北京天气如何？" / "今天深圳天气"

```bash
# User specifies a city - use that city in query
response=$(curl -s "https://60s.viki.moe/v2/weather?query=北京&encoding=json")
response=$(curl -s "https://60s.viki.moe/v2/weather?query=深圳&encoding=json")
```

Extract city name from user's query and pass it to the API.

### User: "今天需要带伞吗？"

Extract umbrella tip from response and answer directly:
```
☂️ 今天不需要带伞，多云天气无降水。
```

### Daily Report Integration

For 日报/早报 weather section:

```markdown
## 🌤️ 今日天气 - [city]

| 项目 | 信息 |
|------|------|
| 天气 | [weather] [temp] |
| 湿度 | [humidity] |
| 风力 | [wind] |
| 空气质量 | [aqi_level] (AQI: [aqi]) |

### 📋 生活建议

| 指数 | 建议 |
|------|------|
| 👕 穿衣 | [clothing] |
| ☂️ 雨伞 | [umbrella] |
| 🧴 防晒 | [sunscreen] |
| 🏃 运动 | [exercise] |

⚠️ **注意事项**: [notice]
```

## Notes

- Default city is 双流 (owner's location) if not specified
- API returns suggestions in Chinese
- Use owner's timezone (UTC+8) for daily reports
- Cache response for 30 minutes to avoid rate limiting
- Always include the "notice" field in daily reports - it contains important weather alerts

## Rate Limiting

- No documented rate limit
- Cache responses for 30 minutes
- For multiple queries in same session, reuse cached data

## Error Recovery

If API returns error:
1. Log the error
2. Try fallback: `curl -s "wttr.in/{city}?format=j1"`
3. If fallback succeeds, parse and format similarly
4. If all fails, inform user: "天气 API 暂时不可用，请稍后再试"
