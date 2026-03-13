# NTU Assistant — Claude Code Skill

每學期開學，你大概都得在 COOL、myNTU、Mail、ePortfolio 四個系統之間反覆跳轉，手動整理課程資訊。這個 Claude Code skill 幫你用一句話搞定——它透過 Chrome DevTools MCP 直接從各系統抓資料，整理成 Markdown 檔案。

Every semester, NTU students manually hop between COOL, myNTU, Mail, and ePortfolio to piece together their course info. This [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) does it for you — it pulls data from each system via Chrome DevTools MCP and organizes everything into Markdown files.

## 功能 / What it does

從 NTU COOL 抓課程、公告、教材（走 Canvas API），從 myNTU 拉課表，把所有作業截止日期整理成一份清單。也可以選擇自動下載 PDF 教材。Mail 只抓信件標題和寄件人，不碰內文。ePortfolio 的學分追蹤也有。

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

1. 裝好 [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
2. 設定 Chrome DevTools MCP server（[設定教學](https://github.com/anthropics/anthropic-quickstarts/tree/main/chrome-devtools-mcp)）
3. 用以下指令開 Chrome：

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# Windows
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

## 安裝 / Install

```bash
npx skills add YouMingYeh/ntu-assistant-skill
```

或手動 clone：

```bash
git clone https://github.com/YouMingYeh/ntu-assistant-skill.git ~/.claude/skills/ntu-assistant
```

## 使用 / Usage

開一個 Claude Code 對話，直接說：

```
幫我同步 NTU COOL 的課程資料
```

```
Sync my NTU COOL courses
```

它會先確認 Chrome MCP 有連上，然後檢查你有沒有登入（登入永遠是你自己在 Chrome 裡操作，skill 不會碰密碼），接著用 Canvas REST API 抓資料，最後在你的目錄下產生整理好的 Markdown 檔案。

跟 NTU 相關的指令都會觸發這個 skill，像是「課表」「公告」「成績」「作業截止日期」「下載教材」「新學期」之類的。

## 安全 / Security

- 登入都是你自己在 Chrome 裡手動操作，skill 不會自動填密碼
- 資料透過 Canvas REST API 抓取，用的是你瀏覽器的 session
- Mail 只讀標題、寄件人、日期，不讀信件內容
- 所有資料都留在你的電腦上

## License

MIT
