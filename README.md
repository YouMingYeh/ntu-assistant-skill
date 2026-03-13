# NTU Assistant — Claude Code Skill

> Sync your NTU academic data in one command.
>
> 一行指令同步你的 NTU 學術資料。

NTU students juggle 4+ fragmented systems — COOL, myNTU, Mail, ePortfolio — with no unified view. This [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) automates academic data extraction using Chrome DevTools MCP.

NTU 學生得在 COOL、myNTU、Mail、ePortfolio 之間來回切換。這個 Claude Code skill 透過 Chrome DevTools MCP 自動擷取學術資料，幫你一次搞定。

## What it does / 功能

- **Courses & Announcements** — extract all active courses, announcements, materials from NTU COOL (Canvas API)
- **Schedule** — pull your weekly timetable from myNTU
- **Assignments & Deadlines** — consolidated deadline view sorted by date
- **Materials Download** — optionally download PDFs from COOL
- **Mail Summary** — inbox overview from NTU Mail (metadata only, privacy-first)
- **ePortfolio** — course/credit tracking

Output: organized Markdown files (`COURSE_SUMMARY.md`, `schedule.md`, `deadlines.md`) + per-course folders.

## Prerequisites / 前置需求

1. [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
2. Chrome DevTools MCP server configured ([setup guide](https://github.com/anthropics/anthropic-quickstarts/tree/main/chrome-devtools-mcp))
3. Chrome launched with `--remote-debugging-port=9222`

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# Windows
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

## Install / 安裝

### Option A: Clone to skills directory

```bash
git clone https://github.com/YouMingYeh/ntu-assistant-skill.git ~/.claude/skills/ntu-assistant
```

### Option B: Manual

Download this repo and place it at `~/.claude/skills/ntu-assistant/`.

## Usage / 使用

Start a Claude Code conversation and say:

```
幫我同步 NTU COOL 的課程資料
```

or in English:

```
Sync my NTU COOL courses
```

The skill will:
1. Check Chrome MCP connection
2. Verify you're logged in (you log in manually — the skill never touches passwords)
3. Extract data via Canvas REST API
4. Generate organized Markdown files in your current directory

### Trigger phrases

The skill activates on NTU-related prompts: `NTU COOL`, `同步課程`, `課表`, `公告`, `成績`, `作業截止日期`, `下載教材`, `新學期`, `ePortfolio`, `NTU mail`, etc.

## Output / 輸出結構

```
your-project/
├── COURSE_SUMMARY.md      # Master course summary
├── schedule.md            # Weekly timetable
├── deadlines.md           # All deadlines sorted by date
├── Course_Name/
│   ├── Week1/             # Downloaded materials
│   ├── Week2/
│   └── Homework/
```

## Security / 安全

- **Never automates login** — you log in manually in Chrome
- **API-first** — uses Canvas REST API with your browser session cookies
- **Privacy** — mail extraction is metadata-only (subject, sender, date)
- **Local only** — all data stays on your machine

## License

MIT
