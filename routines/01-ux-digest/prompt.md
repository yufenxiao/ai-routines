你的任務：每日 UX/AI 中文導讀

## 啟動先讀

1. `CLAUDE.md` —— 全局語言與排版規範
2. `routines/01-ux-digest/style.md` —— 語氣、版權原則、結構模板
3. `routines/01-ux-digest/archive.md` —— 歷史導讀紀錄

兩份規範衝突時以 `routines/01-ux-digest/style.md` 為準。

## 步驟

1. 從 `archive.md` 抓出**過去 30 天**已導讀過的文章 URL，本次選文必須避開。

2. 依照**當天日期 mod 5**決定今天的來源與主題，然後用 WebSearch 搜尋：

   | 餘數 | 來源 | site: 限定 | 搜尋關鍵字方向 |
   |------|------|-----------|--------------|
   | 0 | Nielsen Norman Group | `site:nngroup.com` | UX 原理、研究方法、認知設計（usability, mental models, UX research, cognitive load） |
   | 1 | UX Collective | `site:uxdesign.cc` | UX 設計實作、設計師職涯（UX design, product design, design process, design career） |
   | 2 | Nielsen Norman Group | `site:nngroup.com` | AI × UX（AI UX patterns, designing with AI, UX for AI, AI agent UX） |
   | 3 | UX Planet / UX Matters | 先試 `site:uxplanet.org`，若結果太少改用 `site:uxmatters.com` | UX 趨勢、設計思考（UX trends, future of UX, design thinking） |
   | 4 | Google PAIR / MIT Tech Review | 先試 `site:pair.withgoogle.com`，若結果太少改用 `site:technologyreview.com` | AI 趨勢、人機互動（AI trends, human-AI interaction, responsible AI） |

   - 餘數 2、4（AI 類）：搜尋結果的發布日期須在**近 12 個月**內
   - 餘數 0、1、3（UX 類）：不限時間，優先近期

3. 從搜尋結果挑一篇：資訊密度高、有明確論點、不是純課程廣告或工具推廣文。

4. 嘗試用 WebFetch 抓全文。
   - 若 WebFetch 回傳 403 或遇到 paywall，不要嘗試任何繞過手段。
   - 改用多次精準 WebSearch（搭配文章標題或關鍵段落）拼出主要論點。此時須遵守兩條限制：
     - 只寫在搜尋摘要中明確出現的論點，不推論、不補充「原文可能的意思」

5. 依照 `routines/01-ux-digest/style.md` 的結構模板，用繁體中文寫導讀。

6. 把本次文章資訊**附加一行到 `archive.md` 表格底部**，欄位順序照該檔案表頭。其中：
   - **收錄日**：routine 執行當天（YYYY-MM-DD）
   - **原文發布日**：從文章頁面或搜尋結果取得（YYYY-MM-DD）；找不到精確日期填 `YYYY-MM` 或 `unknown`
   - **原文標題**：`[英文原標題](原文 URL)`
   - **一句話總結**：從導讀「一句話總結」段落抽出

   然後執行：

   ```bash
   git checkout main
   git pull origin main
   git add routines/01-ux-digest/archive.md
   git commit -m "add: UX 導讀 YYYY-MM-DD"
   git push origin main

   不要建立 claude/ 開頭的新分支。只動 archive.md，不要動其他檔案。

7. 把導讀內容轉成 Telegram HTML 格式推送：

7. 格式轉換規則：
  - # 標題 / ## 標題 → <b>標題</b> 後換行
  - **粗體** → <b>粗體</b>
  - [文字](URL) → <a href="URL">文字</a>
  - - 項目 → • 項目
  - 移除 --- 分隔線
  - 表情符號保留

curl -s -X POST "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  --data-urlencode "chat_id=${TELEGRAM_CHAT_ID}" \
  --data-urlencode "text=<轉換好的 HTML>" \
  -d "parse_mode=HTML" \
  -d "disable_web_page_preview=false"
8. 導讀本體同時作為本次 session 的最終輸出訊息。

成功標準

- ✅ 輸出 600–1000 字、附原文連結、結構符合 style.md、不是逐句翻譯的繁體中文導讀
- ✅ archive.md 多了一行新紀錄並已 push 到 main
- ✅ 沒有重複導讀過去 30 天內的同一篇文章
- ✅ AI 類文章（餘數 2、4）：發布日期在近 12 個月內
