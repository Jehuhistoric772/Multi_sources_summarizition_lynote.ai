# AI Summary Skill for Claude Code

A Claude Code skill that generates illustrated, structured summary reports from PDFs, web pages, YouTube videos, and audio/video files — with Mermaid diagrams, anti-hallucination verification, and quality self-assessment.

## Features

- **Multi-source input**: PDF, DOCX, TXT, web URLs, YouTube, audio/video
- **AI-powered summarization**: Claude generates structured summaries with fact verification
- **Illustrated output**: Every chapter includes a Mermaid diagram (pie charts, flowcharts, graphs)
- **Anti-hallucination**: 4-layer defense — fact pre-extraction, outline validation, per-chapter source checking, quality self-assessment
- **Zero server deployment**: No backend needed — Claude does the AI work directly
- **Progressive capability**: Works with zero configuration, unlocks more features as you add API keys

## Quick Start

### 1. Install

**方式 A — 项目级安装（推荐）：**

```bash
# Clone 仓库
git clone https://github.com/YOUR_USERNAME/ai-summary-skill.git

# 拷贝 Skill 到你项目的 Claude Code skills 目录
# ⚠️ 注意：是 .claude/skills/ 相对于你的 Claude Code 项目根目录
cp -r ai-summary-skill/.claude/skills/summarize YOUR_PROJECT/.claude/skills/summarize
```

> **如何确认项目根目录？** 打开 Claude Code，看底部状态栏显示的路径。
> 例如状态栏显示 `C:\Users\you\myproject`，则拷贝到 `C:\Users\you\myproject\.claude\skills\summarize\`

**方式 B — 全局安装（所有项目可用）：**

```bash
# macOS / Linux
cp -r ai-summary-skill/.claude/skills/summarize ~/.claude/skills/summarize

# Windows (Git Bash)
cp -r ai-summary-skill/.claude/skills/summarize "$USERPROFILE/.claude/skills/summarize"
```

**方式 C — 验证安装成功：**

```bash
# 启动 Claude Code，输入 /summarize
# 如果能识别（自动补全出现），说明安装成功
```

### 2. Use

```bash
# Start Claude Code in your project
claude

# Summarize a PDF
/summarize report.pdf

# Summarize a web page
/summarize https://example.com/article

# Summarize multiple sources
/summarize paper.pdf https://example.com https://youtube.com/watch?v=xxx
```

## Capability Levels

### Level 1: Zero Config (no API keys needed)

| Input | How it works |
|-------|-------------|
| PDF (≤5 pages) | Claude reads directly — text + visual understanding of charts/tables |
| DOCX / TXT | Claude reads directly |
| Web URLs | WebFetch extracts article content |

**What you get**: Structured Markdown summary + Mermaid diagrams + quality score

### Level 2: Large Documents

**Requires**: `AZURE_DI_ENDPOINT` + `AZURE_DI_KEY`

Unlocks: PDF files with more than 5 pages, processed via Azure Document Intelligence for high-speed, high-accuracy extraction with table/layout recognition.

### Level 3: YouTube Videos

**Requires**: `SUPADATA_API_KEY`

Unlocks: YouTube video transcript extraction and summarization.

### Level 4: Audio & Video Files

**Requires**: `DEEPGRAM_API_KEY`

Unlocks: Local MP3, MP4, WAV file transcription via Deepgram Nova-3.

## Configuration

Copy the example env file and fill in the keys you need:

```bash
cp .env.example .env
```

Then set the environment variables before starting Claude Code:

```bash
# Option A: Export in your shell
export AZURE_DI_ENDPOINT="https://your-instance.cognitiveservices.azure.com"
export AZURE_DI_KEY="your_key_here"
export SUPADATA_API_KEY="your_key_here"
export DEEPGRAM_API_KEY="your_key_here"

# Option B: Add to your shell profile (~/.bashrc, ~/.zshrc)
# Option C: Use direnv with .envrc
```

### Getting API Keys

| Service | Free Tier | Sign Up |
|---------|-----------|---------|
| Azure Document Intelligence | 500 pages/month free | [Azure Portal](https://portal.azure.com/) → Create "Document Intelligence" resource |
| Supadata (YouTube transcripts) | 50 requests/month free | [supadata.ai](https://supadata.ai/) |
| Deepgram (Audio transcription) | $200 free credit | [deepgram.com](https://deepgram.com/) |

## Output

Every summary generates **two files**:

| File | Purpose | How to view |
|------|---------|-------------|
| `./output/summary-{timestamp}.md` | Source Markdown | VSCode, GitHub, Typora |
| `./output/summary-{timestamp}.html` | Visual report | **Double-click → opens in browser** |

The HTML file is a standalone page with embedded Mermaid JS rendering — no server or plugins needed. Just double-click to see the fully illustrated report.

Both files contain:

- Title and source attribution
- Executive summary
- 3-6 structured chapters, each with:
  - Sub-sections with key data points
  - Mermaid diagram (pie chart, flowchart, or graph)
- Conclusions and recommendations
- Quality self-assessment score (D1-D4)

### Example Output Structure

```markdown
# Core Insights: AI Market Trends 2026

**Source documents:**
- ai-trends-report.pdf
- https://techcrunch.com/2026/03/ai-funding

**Summary**
The AI market is projected to reach $500B by 2027...

## 1. Market Overview

### 1.1 Growth Trajectory
Key findings from the report show **47% YoY growth**...

\`\`\`mermaid
pie title AI Market Share 2026
    "Enterprise AI" : 45
    "Consumer AI" : 30
    "AI Infrastructure" : 25
\`\`\`

### 1.2 Regional Distribution
...

## 2. Investment Landscape
...

## Conclusions and Recommendations
1. ...
2. ...
3. ...
```

## Security

- **Zero hardcoded secrets**: No API keys or server URLs in the skill code
- **Direct API calls**: Your keys go straight to Azure/Supadata/Deepgram — no intermediary servers
- **Local processing**: Claude does AI work locally in your session — no data leaves your machine except to the APIs you configure
- **No backend required**: Nothing to deploy, no server to maintain

## How It Works

```
/summarize input1 input2 ...
         │
         ▼
  Phase 0: Parse inputs → classify as PDF/URL/YouTube/Audio
         │
         ▼
  Phase 1: Extract content
         │  PDF ≤5pg → Claude Read (free)
         │  PDF >5pg → Azure DI (or fallback to Claude Read)
         │  URL → WebFetch
         │  YouTube → Supadata API
         │  Audio → Deepgram API
         │
         ▼
  Phase 2: AI Summary (Claude does this directly)
         │  Step A: Extract facts from source text
         │  Step B: Generate structured outline
         │  Step C: Validate outline against facts
         │  Step D: Write chapters + Mermaid diagrams
         │
         ▼
  Phase 3: Output
         │  Assemble Markdown
         │  Save to ./output/
         │  Quality self-assessment
         │  Display preview
         ▼
  Done!
```

## Coming Soon: LyNote.ai

> **Want a one-click experience without any setup?**
>
> [**LyNote.ai**](https://lynote.ai) is building a multimodal AI summarization platform — upload any content and get beautifully illustrated reports instantly.
>
> **Supported formats:**
> PDF | Word | PPT | TXT | YouTube | URL | Audio | Video
>
> No API keys. No configuration. Just drag, drop, and read.
>
> [Join the waitlist → lynote.ai](https://lynote.ai)

## License

MIT
