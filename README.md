# NTU Assistant

你的 NTU AI 助手。它會操作 Chrome 幫你從 COOL、ePortfolio、Mail 抓資料回來，查課程、下載教材、整理作業截止日期、看成績都行。

An AI assistant for NTU students. It drives Chrome to fetch data from COOL, ePortfolio, and Mail — courses, materials, deadlines, grades, whatever you need.

## 能做什麼

- 同步 COOL 上的課程、公告、作業、成績
- 下載講義和簡報
- 從教務系統抓課表
- 讀 NTU Mail
- 查學分進度
- 整理完的資料可以做成一頁式網頁

## 裝之前

1. [Node.js](https://nodejs.org/) v20+
2. 一個支援 [skills](https://skills.sh) 的 AI 工具（Claude Code、Codex、Cursor 都行）
3. 讓 AI 可以操作 Chrome：

```bash
claude mcp add chrome-devtools -- npx chrome-devtools-mcp@latest
```

用的時候它會自己開 Chrome，不用手動。

<details>
<summary>其他工具的設定</summary>

在 MCP 設定檔加：

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

</details>

## 安裝

```bash
npx skills add YouMingYeh/ntu
```

過程中問的問題，**全部按 Enter** 就好。

更新：`npx skills update`

<details>
<summary>手動裝</summary>

```bash
git clone https://github.com/YouMingYeh/ntu.git ~/.claude/skills/ntu
```

</details>

## 用法

輸入 `/ntu`，它會列出能做的事讓你選。也可以直接講：

```
/ntu
/ntu 幫我下載所有課程教材
/ntu 整理課表和近期作業
/ntu 這週有什麼作業？
/ntu 幫我看成績
/ntu Download all course materials
/ntu What's due this week?
```

第一次用會開 Chrome 處理登入。你可以給帳密讓它填，或自己在 Chrome 登好再告訴它。登一次就夠了，NTU 各系統共用 SSO。

## 安全

帳密只用來當場填入登入表單，不存。資料都在你電腦上。

## 搭配推薦

- [anthropics/skills](https://github.com/anthropics/skills) — PDF、Word、PPT、Excel 處理
- [blader/humanizer](https://github.com/blader/humanizer) / [kevintsai1202/Humanizer-zh-TW](https://github.com/kevintsai1202/Humanizer-zh-TW) — 寫報告去 AI 味

```bash
npx skills add anthropics/skills
npx skills add blader/humanizer
npx skills add kevintsai1202/Humanizer-zh-TW
```

## License

MIT
