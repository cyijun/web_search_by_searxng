# SearXNG Search Skill

[English](README.md) | 中文

一个用于 [Claude Code](https://github.com/anthropics/anthropic-coder) 的 Skill，通过 SearXNG 搜索引擎实现隐私友好的网络搜索功能。

## 功能特性

- 🔍 支持自定义 SearXNG 实例
- 🌐 多类别搜索（网页、图片、视频、新闻、科学文献等）
- 🎯 多引擎聚合（Google、DuckDuckGo、Bing、Wikipedia 等）
- 📊 支持 JSON/CSV/RSS 多种输出格式
- 🌍 多语言支持
- ⏰ 时间范围过滤（天、月、年）
- 🔒 安全搜索级别设置
- 📄 分页支持

## 安装

### 1. 克隆仓库

```bash
git clone https://github.com/cyijun/searxng-search-skill.git
cd searxng-search-skill
```

### 2. 安装到 Claude Code

```bash
# 将 .skill 文件复制到 Claude Code 的 skills 目录
cp searxng-search.skill ~/.claude/skills/

# 或者直接使用 skill 命令安装
claude skill install ./searxng-search.skill
```

## 使用方法

### 配置 SearXNG 实例

使用前需要配置 SearXNG 实例 URL。有以下几种方式：

1. **环境变量**
   ```bash
   export SEARXNG_URL=https://searx.example.org
   ```

2. **在对话中直接指定**
   告诉 Claude 你的 SearXNG 实例地址即可

3. **使用公共实例**
   - https://searx.be
   - https://search.sapti.me
   - https://search.bus-hit.me
   - [更多公共实例](https://searx.space/)

### 命令行工具

```bash
# 基本搜索
python scripts/searxng_search.py -u https://searx.example.org -q "Python 教程"

# JSON 输出
python scripts/searxng_search.py -u https://searx.example.org -q "Python 教程" --format json

# 指定语言和类别
python scripts/searxng_search.py -u https://searx.example.org -q "新闻" --lang zh --categories news

# 时间范围过滤
python scripts/searxng_search.py -u https://searx.example.org -q "AI" --time-range day

# 指定搜索引擎
python scripts/searxng_search.py -u https://searx.example.org -q "Python" --engines google,stackoverflow

# 使用环境变量
export SEARXNG_URL=https://searx.example.org
python scripts/searxng_search.py -q "搜索内容"
```

### 在 Claude Code 中使用

安装 skill 后，Claude 会自动在需要搜索时使用 SearXNG：

```
用户: 帮我搜索最新的 Python 3.12 新特性
Claude: 我将使用 SearXNG 为你搜索...
```

## 支持的搜索类别

| 类别 | 说明 | 示例引擎 |
|------|------|----------|
| `general` | 通用网页搜索 | Google, DuckDuckGo, Bing |
| `images` | 图片搜索 | Google Images, Unsplash |
| `videos` | 视频搜索 | YouTube, Vimeo |
| `news` | 新闻搜索 | Google News, Reuters |
| `science` | 学术文献 | Google Scholar, arXiv |
| `it` | IT/技术资源 | GitHub, Stack Overflow |
| `music` | 音乐搜索 | SoundCloud, Bandcamp |
| `files` | 文件搜索 | - |
| `map` | 地图搜索 | OpenStreetMap |
| `social_media` | 社交媒体 | Reddit |

## Bang 语法

使用 `!` 前缀快速指定搜索引擎：

- `!go Python` - 搜索 Google
- `!wp 人工智能` - 搜索 Wikipedia
- `!gh machine learning` - 搜索 GitHub
- `!yt 音乐视频` - 搜索 YouTube
- `!gos 深度学习论文` - 搜索 Google Scholar

## 项目结构

```
searxng-search/
├── searxng-search.skill      # Claude Code skill 文件
├── SKILL.md                  # Skill 详细文档
├── scripts/
│   └── searxng_search.py     # 命令行搜索工具
└── references/
    └── engines_and_categories.md  # 引擎和类别参考文档
```

## API 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| `q` | string | 搜索查询（必填） |
| `format` | string | 输出格式: `json`, `csv`, `rss` |
| `language` | string | 语言代码，如 `en`, `zh`, `de` |
| `pageno` | int | 页码，默认 1 |
| `time_range` | string | 时间范围: `day`, `month`, `year` |
| `categories` | string | 类别列表，逗号分隔 |
| `engines` | string | 引擎列表，逗号分隔 |
| `safesearch` | int | 安全搜索: `0`=关闭, `1`= moderate, `2`=严格 |

## 自行部署 SearXNG

如需更稳定的服务，建议自行部署：

```bash
# 使用 Docker 部署
docker run -d --name searxng \
  -p 8080:8080 \
  -v searxng-data:/etc/searxng \
  searxng/searxng
```

更多部署方式请参考 [SearXNG 官方文档](https://docs.searxng.org/)。

## 注意事项

1. **格式支持**：JSON/CSV/RSS 格式需要在 SearXNG 实例的 `settings.yml` 中启用，部分公共实例可能禁用这些格式
2. **速率限制**：请合理使用，避免频繁请求
3. **引擎可用性**：不同实例启用的搜索引擎不同，可通过实例的偏好设置页面查看

## 许可证

MIT License

## 相关链接

- [SearXNG 官网](https://docs.searxng.org/)
- [SearXNG 公共实例列表](https://searx.space/)
- [Claude Code 文档](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
