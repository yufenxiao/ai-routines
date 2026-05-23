# AI Routines 工作區

我自己的 Claude Code routine 集中管理 repo。每個 routine 是排程／自動觸發的 AI 任務，跑在 Anthropic 雲端，把例行性的閱讀、整理、研究自動化。

## 目錄結構

````
ai-routines/
├── CLAUDE.md                 ← 全局規範索引（routine 啟動會自動讀）
├── README.md                 ← 這份文件
├── style/                    ← 全局風格規範
│   └── language-tone.md      ← 語言與排版
└── routines/                 ← 每個 routine 一個資料夾
    └── 01-nng-digest/
        ├── prompt.md         ← 貼到 routine UI 的 prompt
        ├── style.md          ← 這個 routine 特有的風格
        └── archive.md        ← 歷史導讀紀錄（routine 自動維護）
````

之後如果有多個 routine 都要用到同一份素材，可以再開 `shared/` 資料夾統一管理。

## 目前的 routines

| 編號 | 名稱 | 觸發 | 用途 |
|---|---|---|---|
| 01 | nng-digest | 每日排程 | 抓一篇 NN/g UX 文章寫成繁體中文導讀 |

## 新增 routine 的流程

1. 在 `routines/` 下開新資料夾，編號順序累加（例如 `02-xxx/`）
2. 至少放兩個檔案：
   - `prompt.md` — 貼到 routine UI 的指令
   - `style.md` — 這個 routine 特有的風格規範（會覆蓋全局）
3. 到 [claude.ai/code/routines](https://claude.ai/code/routines) 建立 routine：
   - **Repository** 選這個 repo
   - **Prompt** 貼 `prompt.md` 分隔線之間的內容
   - 如果 routine 需要寫檔回 repo（例如維護 archive），**Permissions** 區塊勾 **Allow unrestricted branch pushes**
4. 更新本 README 的「目前的 routines」表格
