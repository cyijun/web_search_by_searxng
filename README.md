# SearXNG Search Skill

English | [中文](README.zh.md)

A [Claude Code](https://github.com/anthropics/anthropic-coder) skill for privacy-friendly web search using the SearXNG metasearch engine.

## Features

- 🔍 Custom SearXNG instance support
- 🌐 Multi-category search (web, images, videos, news, scientific papers, etc.)
- 🎯 Multi-engine aggregation (Google, DuckDuckGo, Bing, Wikipedia, etc.)
- 📊 Multiple output formats (JSON/CSV/RSS)
- 🌍 Multi-language support
- ⏰ Time range filtering (day, month, year)
- 🔒 Safe search level settings
- 📄 Pagination support

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/cyijun/searxng-search-skill.git
cd searxng-search-skill
```

### 2. Install to Claude Code

```bash
# Copy the .skill file to Claude Code's skills directory
cp searxng-search.skill ~/.claude/skills/

# Or install using the skill command
claude skill install ./searxng-search.skill
```

## Usage

### Configure SearXNG Instance

You need to configure a SearXNG instance URL before use. There are several ways:

1. **Environment Variable**
   ```bash
   export SEARXNG_URL=https://searx.example.org
   ```

2. **Specify in Conversation**
   Simply tell Claude your SearXNG instance address

3. **Use Public Instances**
   - https://searx.be
   - https://search.sapti.me
   - https://search.bus-hit.me
   - [More public instances](https://searx.space/)

### Command Line Tool

```bash
# Basic search
python scripts/searxng_search.py -u https://searx.example.org -q "Python tutorial"

# JSON output
python scripts/searxng_search.py -u https://searx.example.org -q "Python tutorial" --format json

# Specify language and category
python scripts/searxng_search.py -u https://searx.example.org -q "news" --lang en --categories news

# Time range filter
python scripts/searxng_search.py -u https://searx.example.org -q "AI" --time-range day

# Specify search engines
python scripts/searxng_search.py -u https://searx.example.org -q "Python" --engines google,stackoverflow

# Use environment variable
export SEARXNG_URL=https://searx.example.org
python scripts/searxng_search.py -q "search query"
```

### Use in Claude Code

After installing the skill, Claude will automatically use SearXNG when search is needed:

```
User: Help me search for the latest Python 3.12 features
Claude: I'll search for you using SearXNG...
```

## Supported Search Categories

| Category | Description | Example Engines |
|----------|-------------|-----------------|
| `general` | General web search | Google, DuckDuckGo, Bing |
| `images` | Image search | Google Images, Unsplash |
| `videos` | Video search | YouTube, Vimeo |
| `news` | News articles | Google News, Reuters |
| `science` | Scientific papers | Google Scholar, arXiv |
| `it` | IT/Tech resources | GitHub, Stack Overflow |
| `music` | Music search | SoundCloud, Bandcamp |
| `files` | File search | - |
| `map` | Map search | OpenStreetMap |
| `social_media` | Social media | Reddit |

## Bang Syntax

Use `!` prefix to quickly specify search engines:

- `!go Python` - Search Google
- `!wp artificial intelligence` - Search Wikipedia
- `!gh machine learning` - Search GitHub
- `!yt music video` - Search YouTube
- `!gos deep learning paper` - Search Google Scholar

## Project Structure

```
searxng-search/
├── searxng-search.skill      # Claude Code skill file
├── SKILL.md                  # Detailed skill documentation
├── scripts/
│   └── searxng_search.py     # CLI search tool
└── references/
    └── engines_and_categories.md  # Engine and category reference
```

## API Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `q` | string | Search query (required) |
| `format` | string | Output format: `json`, `csv`, `rss` |
| `language` | string | Language code, e.g., `en`, `zh`, `de` |
| `pageno` | int | Page number, default 1 |
| `time_range` | string | Time range: `day`, `month`, `year` |
| `categories` | string | Category list, comma-separated |
| `engines` | string | Engine list, comma-separated |
| `safesearch` | int | Safe search: `0`=off, `1`=moderate, `2`=strict |

## Self-Host SearXNG

For more stable service, consider self-hosting:

```bash
# Deploy using Docker
docker run -d --name searxng \
  -p 8080:8080 \
  -v searxng-data:/etc/searxng \
  searxng/searxng
```

For more deployment options, refer to the [SearXNG official documentation](https://docs.searxng.org/).

## Important Notes

1. **Format Support**: JSON/CSV/RSS formats need to be enabled in the SearXNG instance's `settings.yml`. Some public instances may disable these formats.
2. **Rate Limiting**: Please use responsibly and avoid frequent requests.
3. **Engine Availability**: Different instances have different engines enabled. Check the instance's preferences page.

## License

MIT License

## Related Links

- [SearXNG Official Site](https://docs.searxng.org/)
- [SearXNG Public Instances](https://searx.space/)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
