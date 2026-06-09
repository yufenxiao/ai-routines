# 你的任務：每日 Medium UX/AI 中文導讀

## 啟動先讀

1. `CLAUDE.md` —— 全局語言與排版規範
2. `routines/02-UX-reading/style.md` —— 語氣、版權、結構模板
3. `routines/02-UX-reading/archive.md` —— 歷史導讀紀錄

兩份規範衝突時以 `routines/02-UX-reading/style.md` 為準。

---

## 步驟

### 1. 載入 Medium cookies

用 `browser_run_code_unsafe` 執行以下程式碼：

```javascript
const fs = require('fs');
const cookiePath = `${process.env.HOME}/projects/ai-routines/routines/02-UX-reading/medium-cookies.json`;

let rawCookies;
try {
  rawCookies = JSON.parse(fs.readFileSync(cookiePath, 'utf8'));
} catch(e) {
  throw new Error('❌ 找不到 medium-cookies.json，請先按照 setup/how-to-get-cookies.md 設定');
}

const playwrightCookies = rawCookies.map(c => ({
  name: c.name,
  value: c.value,
  domain: c.domain,
  path: c.path || '/',
  expires: c.expirationDate || -1,
  httpOnly: c.httpOnly || false,
  secure: c.secure || false,
  sameSite: c.sameSite === 'no_restriction' ? 'None' :
            c.sameSite === 'lax' ? 'Lax' :
            c.sameSite === 'strict' ? 'Strict' : 'Lax'
}));

await context.addCookies(playwrightCookies);
return `✅ 已載入 ${playwrightCookies.length} 個 cookies`;

接著用 browser_navigate 前往 https://medium.com，用 browser_snapshot 確認頁面顯示登入後 UI（有個人 avatar）。若仍顯示登入按鈕，停止執行並提示用戶重新取得 cookies（參見 setup/how-to-get-cookies.md）。

---
2. 從 archive.md 排除近 30 天已讀文章

讀取 routines/02-UX-reading/archive.md，抓出收錄日在過去 30 天內的所有文章 URL，本次選文必須避開。

---
3. 發現候選文章

今天的主題方向按當天日期 mod 3 輪換：

┌──────┬─────────────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 餘數 │            方向             │                                                              RSS feeds                                                               │
├──────┼─────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 0    │ UX 原理、研究方法、設計思考 │ https://uxdesign.cc/feedhttps://medium.com/feed/tag/ux-designhttps://medium.com/feed/tag/user-experience                             │
├──────┼─────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 1    │ AI × UX 設計實作            │ https://medium.com/feed/tag/ai-uxhttps://medium.com/feed/tag/ux-for-aihttps://medium.com/feed/tag/product-design                     │
├──────┼─────────────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 2    │ AI 趨勢與設計師職涯         │ https://medium.com/feed/tag/artificial-intelligencehttps://medium.com/feed/tag/future-of-designhttps://medium.com/feed/tag/ux-career │
└──────┴─────────────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────────────────┘

用 WebFetch 逐一抓取以上 RSS XML，解析每篇文章的：
- 標題 <title>
- URL <link>
- 發布日期 <pubDate>
- 摘要 <description>

---
4. 選出本次文章

從候選清單選一篇，條件優先順序：
1. 不在 archive 近 30 天（必要）
2. AI 方向（餘數 1、2）：<pubDate> 在近 12 個月內（必要）
3. UX 方向（餘數 0）：不限時間，但優先近期
4. 排除：純課程廣告、「10 Things...」點擊誘餌型標題、工具推廣文
5. 偏好：有明確論點、研究佐證、或實作案例

---
5. 讀取全文

用 browser_navigate 前往選中文章，等待頁面載入，用 browser_snapshot 取得完整文章內容。

若文章仍顯示 paywall（代表 cookie 可能過期）：
- 不要嘗試繞過
- 停止執行，提示用戶更新 cookies（setup/how-to-get-cookies.md）

---
6. 依 style.md 寫導讀

參照 routines/02-UX-reading/style.md 的結構模板，用繁體中文撰寫 600–1000 字導

---
7. 更新 archive.md

附加一行到 archive.md 表格底部：

- 收錄日：今天（YYYY-MM-DD）
- 原文發布日：從文章頁面或 RSS <pubDate> 取得（YYYY-MM-DD；找不到填 unknown）
- 原文標題：[英文原標題](文章 URL)
- 一句話總結：從導讀「一句話總結」段落抽出

然後執行：

git checkout main
git pull origin main
git add routines/02-UX-reading/archive.md
git commit -m "add: Medium 導讀 YYYY-MM-DD"
git push origin main

只動 archive.md，不要動其他檔案。不要建立 claude/ 開頭的新分支。

---
8. 推送 Telegram

格式轉換規則：
- # 標題 / ## 標題 → <b>標題</b> 後換行
- **粗體** → <b>粗體</b>
- [文字](URL) → <a href="URL">文字</a>
- - 項目 → • 項目
- 移除 --- 分隔線
- 保留表情符號

執行：

curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
  --data-urlencode "text=<轉換好的 HTML>" \
  -d "parse_mode=HTML" \
  -d "disable_web_page_preview=false"

---
9. 輸出導讀本體

把導讀完整輸出在 session 中。

---
成功標準

- ✅ 輸出 600–1000 字、附原文連結、結構符合 style.md、不是逐句翻譯的繁體中文導讀
- ✅ archive.md 多了一行新紀錄並已 push 到 main
- ✅ 沒有重複導讀過去 30 天內的同一篇文章
- ✅ AI 類文章：發布日期在近 12 個月內

---
