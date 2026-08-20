# Firecrawl 網頁擷取工具

Firecrawl 官方 Claude plugin，讓 Claude 可以即時搜尋網頁、抓取頁面內容、爬整個網站，回傳乾淨的 Markdown。routine 需要即時網路資料時可以用。

## 安裝狀態

這個 repo 的 `.claude/settings.json` 已經宣告好 marketplace 與 plugin，**在這個 repo 開 Claude Code 就會自動載入**，不用再手動裝 plugin。

等價的手動指令（想裝到 user scope、所有專案都能用時）：

```bash
claude plugin marketplace add firecrawl/firecrawl-claude-plugin
claude plugin install firecrawl@firecrawl
```

## 前置作業（每台機器做一次）

plugin 本身只是 skill 包，實際跑的是 `firecrawl` CLI，要另外安裝：

```bash
npm install -g firecrawl-cli
firecrawl --status          # 確認安裝與額度
```

## 認證

三種方式，任選一種：

| 方式 | 指令 | 說明 |
|---|---|---|
| 瀏覽器登入 | `firecrawl login --browser` | 最快，會開瀏覽器註冊免費帳號 |
| API key | `firecrawl login --api-key "fc-xxx"` | key 在 <https://firecrawl.dev/app/api-keys> 拿 |
| 環境變數 | `export FIRECRAWL_API_KEY=fc-xxx` | 適合寫進 shell profile 或 CI |

不認證也能用 keyless 免費層，但有 rate limit，正式用還是建議申請免費帳號。

## 常用指令

```bash
firecrawl search "關鍵字"    # 搜尋網頁，可一併抓內容
firecrawl scrape <url>       # 單頁轉乾淨 Markdown
firecrawl map <url>          # 列出整站 URL
firecrawl crawl <url>        # 爬整站內容
```

抓下來的結果會存在專案目錄的 `.firecrawl/`，已加進 `.gitignore`。

## 在雲端 routine 使用的限制

Claude Code 雲端環境的 network egress policy 預設**不允許連 `api.firecrawl.dev`**（實測 CONNECT tunnel 回 403），所以 routine 跑在雲端時 Firecrawl CLI 會失敗。要在 routine 裡用，得先到 environment 設定把 `api.firecrawl.dev` 加進允許的網域，詳見 <https://code.claude.com/docs/en/claude-code-on-the-web>。在本機跑 Claude Code 不受這個限制。

## claude.ai 上的原生 connector

Firecrawl 另外有 claude.ai 的原生 connector（已上架 Anthropic Connector Directory），跟這裡的 Claude Code plugin 是兩套東西。原生 connector 要走 OAuth 授權，只能自己在 claude.ai 的 Settings → Connectors 裡點開啟用，沒辦法從 Claude Code 代裝。
