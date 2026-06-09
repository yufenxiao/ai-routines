# 如何取得 Medium Cookies

Cookie 通常 30–90 天有效。若 routine 顯示「cookie 已過期」，重跑這個流程即可。

## 工具

安裝 Chrome 擴充套件：**Cookie-Editor**
（搜尋 "Cookie-Editor" 安裝即可，選 icons8 出的那個）

## 步驟

1. 用 Chrome 開啟 `https://medium.com`，確認已用 Google 帳號登入
2. 點右上角 Cookie-Editor 圖示
3. 點「Export」→「Export as JSON」
4. 複製所有內容
5. 在你的電腦建立（或覆蓋）這個檔案：
   ~/projects/ai-routines/routines/02-UX-reading/medium-cookies.json
6. 把複製的 JSON 貼進去，儲存

## 注意

- `medium-cookies.json` 已加入 `.gitignore`，不會被 commit 到 GitHub
- 這個檔案等同你的 Medium 登入憑證，不要分享給任何人

---
修改現有檔案：.gitignore

加入這一行：

routines/02-UX-reading/medium-cookies.json

---
