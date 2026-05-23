# 排程任務 prompt：每日 NN/g 中文導讀

1. 這份檔案分隔線之間的內容貼到 [claude.ai/code/routines](https://claude.ai/code/routines) 的 Prompt 欄位。改了之後別忘了同步更新 routine UI 上的 prompt。

2. **歷史導讀紀錄請查閱同層的 `archive.md`。**

---

你的任務：每日 NN/g UX 中文導讀

## 啟動先讀

1. `CLAUDE.md`（會自動載入 `style/language-tone.md`）—— 全局語言與排版規範
2. `routines/01-nng-digest/style.md` —— 這個 routine 的語氣、版權原則、結構模板
3. `routines/01-nng-digest/archive.md` —— 歷史導讀紀錄

兩份規範衝突時以 `routines/01-nng-digest/style.md` 為準。

## 步驟

1. 從 `archive.md` 抓出**過去 30 天**已導讀過的文章 URL，本次選文必須避開。

2. 用 WebSearch（限定 `site:nngroup.com`）搜尋一篇近期文章。主題每天輪換以下三個方向之一：
   - UX 未來趨勢（state of UX、future of UX、UX）
   - 設計師職涯／AI 是否會取代設計師（future-proof designer、UX job market、AI replacing designers）
   - AI／UI／UX 設計實作（AI UX patterns、UX for AI、AI for UX、generative UI、designing with AI、AI agent UX）

3. 從搜尋結果挑一篇資訊密度高、發布日期較新、且不是純行銷課程文。

4. 嘗試用 WebFetch 抓全文。
   - 若 WebFetch 回傳 403，不要嘗試任何繞過手段。
   - 改用多次精準 WebSearch（搭配文章標題或關鍵段落）拼出文章主要論點。此時須遵守兩條限制：
     - 只寫在搜尋摘要中明確出現的論點，不推論、不補充「原文可能的意思」
     - 導讀最上方必須加一行：⚠️ 本篇無法直接抓取全文，重點根據搜尋摘要整理，建議點連結對照原文。

5. 依照 `routines/01-nng-digest/style.md` 的結構模板，用繁體中文寫導讀。

6. 把本次的文章資訊**附加一行到 `archive.md` 表格底部**，欄位順序照該檔案表頭。其中：
   - **收錄日**：routine 執行當天（YYYY-MM-DD）
   - **原文發布日**：從 NN/g 文章頁面或搜尋結果抓出文章的 publication date（YYYY-MM-DD）。NN/g 文章頁面通常在標題下方顯示日期；若 WebFetch 被擋，從 WebSearch 結果或 URL 的年份線索推斷。若真的找不到精確日期，填入 `YYYY-MM`（只到月）或 `unknown`
   - **原文標題**：用 Markdown 連結語法把 URL 包進去，格式為 `[英文原標題](原文 URL)`，不要另外開連結欄位
   - **一句話總結**：從導讀的「一句話總結」段落抽出來

   然後執行：

   ```
   git checkout main
   git pull origin main
   git add routines/01-nng-digest/archive.md
   git commit -m "add: NN/g 導讀 YYYY-MM-DD"
   git push origin main
   ```

   **不要建立 `claude/` 開頭的新分支**，直接 commit 到 main。只動 `archive.md`，不要動其他檔案。

7. 導讀本體直接作為本次 session 的**最終輸出訊息**，不需要寫成檔案。

## 成功標準

- ✅ 輸出是一篇 600–1000 字、附原文連結、結構符合 `style.md` 模板、不是逐句翻譯的繁體中文導讀
- ✅ `archive.md` 多了一行新紀錄並已 push 到 main
- ✅ 沒有重複導讀過去 30 天內的同一篇文章
