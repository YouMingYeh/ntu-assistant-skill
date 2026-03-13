# NTU Assistant — Agent Skill

每學期開學，你大概都得在 COOL、myNTU、Mail、ePortfolio 四個系統之間反覆跳轉，手動整理課程資訊。這個 skill 幫你用一句話搞定——它透過 Chrome DevTools MCP 直接從各系統抓資料，整理成 Markdown 檔案。

Every semester, NTU students manually hop between COOL, myNTU, Mail, and ePortfolio to piece together their course info. This skill does it for you — it pulls data from each system via [Chrome DevTools MCP](https://github.com/nicolo-nicolo/chrome-devtools-mcp) and organizes everything into Markdown files.

支援 Claude Code、Codex、OpenCode、Cursor、Copilot 等所有 [skills](https://skills.sh) 相容的 AI agent。

## 功能 / What it does

從 NTU COOL 抓課程、公告、教材，從 myNTU 拉課表，把所有作業截止日期整理成一份清單。也可以選擇自動下載 PDF 教材。Mail 只抓信件標題和寄件人，不碰內文。ePortfolio 的學分追蹤也有。

最後產出長這樣：

```
your-project/
├── COURSE_SUMMARY.md      # 課程總覽
├── schedule.md            # 週課表
├── deadlines.md           # 截止日期（按時間排序）
├── Course_Name/
│   ├── Week1/             # 下載的教材
│   ├── Week2/
│   └── Homework/
```

## 前置需求 / Prerequisites

1. [Node.js](https://nodejs.org/) v20 以上
2. 任何支援 skills 的 AI agent（Claude Code、Codex、OpenCode、Cursor 等）
3. Chrome DevTools MCP server

裝 Chrome DevTools MCP 只要一行：

```bash
# Claude Code
claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest

# 或手動加到 MCP 設定
```

<details>
<summary>手動 MCP 設定（JSON）</summary>

在你的 MCP 設定檔加入：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

MCP 啟動後會自動開 Chrome，不需要手動跑 `--remote-debugging-port`。

如果要連到已開啟的 Chrome：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp@latest", "--browser-url=http://127.0.0.1:9222"]
    }
  }
}
```

</details>

## 安裝 / Install

```bash
npx skills add YouMingYeh/ntu
```

或手動 clone：

```bash
git clone https://github.com/YouMingYeh/ntu.git ~/.claude/skills/ntu
```

更新：

```bash
npx skills update
```

## 使用 / Usage

開一個對話，用你自己的話說就好：

```
幫我下載所有課程教材
```
```
幫我整理課表、近期待辦事項、作業和公告
```
```
這週有什麼作業要交？
```
```
新學期開始了，幫我同步所有課程資料
```
```
把 COOL 上面的成績整理給我看
```

英文也行：

```
Download all my course materials from COOL
```
```
What assignments are due this week?
```

它會自己確認 Chrome MCP 有沒有連上、你有沒有登入（登入是你自己在 Chrome 操作，skill 不碰密碼），然後從各系統抓資料，在你的目錄下產生整理好的檔案。

## 安全 / Security

- 登入都是你自己在 Chrome 裡手動操作，skill 不會自動填密碼
- 資料用你瀏覽器的 session 抓取，不另存帳號資訊
- Mail 只讀標題、寄件人、日期，不讀信件內容
- 所有資料都留在你的電腦上

## 相關資源 / Resources

不知道 skills 是什麼的話，可以先看這些：

- [skills.sh](https://skills.sh/) — skills 搜尋引擎，找找有什麼好用的
- [anthropics/skills](https://github.com/anthropics/skills) — Anthropic 官方 skills，PDF、Word、PPT、Excel 文件處理都有

寫報告或文章的時候，AI 味太重？這兩個 skill 可以幫你修：

- [blader/humanizer](https://github.com/blader/humanizer) — 英文去 AI 味
- [kevintsai1202/Humanizer-zh-TW](https://github.com/kevintsai1202/Humanizer-zh-TW) — 中文去 AI 味

裝法都一樣：

```bash
npx skills add anthropics/skills
npx skills add blader/humanizer
npx skills add kevintsai1202/Humanizer-zh-TW
```

## License

MIT
