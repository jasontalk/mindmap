# 心智圖工具（Mindmap）

一個放在網頁上的簡易心智圖工具，用來把計畫或專案拆成分項，方便理解與討論。
純前端單一 HTML 檔，不用安裝、不用後端伺服器；資料存在公司的共用雲端硬碟，成員輪流編輯。

**線上網址（直接使用）**：<https://jasontalk.github.io/mindmap/>

---

## 這個 repo 是什麼

- `index.html` — 工具本體（單一檔案，含所有程式與樣式）。透過 GitHub Pages 發佈成上面的網址。
- 心智圖的內容不存在這個 repo，而是存成 `.json` 檔放在共用雲端硬碟的 **Mindmap Projects** 資料夾。

> 一般使用者不需要看這個 repo，開網址就能用。這份說明主要是給維護者（未來要改程式的人）看的。

---

## 給使用者：怎麼用

### 開始使用

- **工具網址**：<https://jasontalk.github.io/mindmap/>（建議用 Chrome，加到書籤）
- **檔案存放**：公司共用雲端硬碟的「Mindmap Projects」資料夾，每個專案就是裡面的一個 `.json` 檔。

第一次按雲端按鈕（☁）時，Google 會跳出登入與授權畫面，請用**公司帳號**登入並同意，只會發生一次。

### 基本操作

| 動作 | 方法 |
| --- | --- |
| 新增子項目 | 選一個項目，按 `Tab` |
| 新增同層項目 | 按 `Enter` |
| 修改文字 | 按 `F2`，或用滑鼠**雙擊**該項目 |
| 刪除項目 | 按 `Delete` |
| 收合／展開分支 | 點項目右側的小圓圈，或按 `空白鍵` |
| 在項目間移動 | `方向鍵` |
| 移動畫面 | 用滑鼠**拖曳空白處** |
| 放大縮小 | 右下角 ＋ / − 按鈕，或滑鼠滾輪 |
| 一鍵看全圖 | 上方「置中檢視」 |
| 復原／重做 | `Ctrl+Z（⌘Z）` / 重做 |

新增項目後會直接進入編輯狀態，可以馬上打字。

### 開檔與存檔（重點）

- **☁ 從 Drive 開啟**：跳出檔案選擇器 → 到「Mindmap Projects」資料夾選一個 `.json` → 載入。
- **☁ 存到 Drive**：若這張圖是從 Drive 開啟的，按下去會**直接覆蓋更新**原檔；若是新做的圖（還沒存過），會請你**選一個資料夾**並在裡面**建立新檔**，之後再按存檔就會直接更新、不用再選。

另外「開新」可開一張全新的圖；「開啟檔案／存成檔案」是存到自己電腦；「匯出 PNG」可把整張圖存成圖片。

### 共用規則（請大家遵守）

1. **一次一人編輯**同一張圖，要改之前先確認沒有別人正在改。
2. **改完馬上「☁ 存到 Drive」**，讓下一個人打開的是最新版。
3. 一個專案就用**一個 `.json` 檔**、每次覆蓋更新，不要一直另存新檔；命名清楚一點（例如 `2026春季活動規劃.json`）。

### 常見問題

- **按 ☁ 沒反應或出現錯誤？** 先按 `Ctrl+Shift+R`（`⌘Shift+R`）強制重新整理再試。
- **找不到某個專案檔？** 確認你有「Mindmap Projects」資料夾的存取權，沒有的話請找管理人加入。
- **想留一個時間點的版本？** 先「☁ 從 Drive 開啟」載入，改個名字，再「存成檔案」或另存一份到 Drive。

---

## 給維護者：怎麼修改與部署

### 改程式後如何更新

1. 修改 `index.html`。
2. 在 repo 按 **Add file → Upload files**，上傳新的 `index.html` 覆蓋舊檔，**Commit changes**。
3. 等約 1 分鐘讓 GitHub Pages 重新部署。
4. 開網址時按 `Ctrl+Shift+R`（`⌘Shift+R`）強制重新整理，避免看到舊快取。

### Google Drive 整合設定

工具透過 Google OAuth + Drive API 直接讀寫共用雲端硬碟。`index.html` 最上方有三個設定值需要填：

- `GOOGLE_CLIENT_ID` — OAuth 用戶端 ID（Web application）
- `GOOGLE_API_KEY` — API 金鑰
- `GOOGLE_APP_ID` — Google Cloud 專案編號（Project number）

相關設定在 Google Cloud Console（專案：Mindmap App）：

- OAuth 同意畫面設為 **Internal**（僅公司內部），因此使用完整 `drive` 權限不需要 Google 驗證。
- API 金鑰限制在網站 `https://jasontalk.github.io/*`；OAuth 用戶端的 JavaScript 來源為 `https://jasontalk.github.io`。
- 因為檔案放在**共用雲端硬碟（Shared Drive）**，所有 Drive API 請求都必須帶 `supportsAllDrives=true`（讀取、更新、建立），少了會回 404。

### 注意

- 這是公開 repo，`index.html` 內的 Client ID／API Key 會被看到——這對純前端 App 是正常且安全的（已加網站來源限制，且同意畫面限公司內部）。
- 真正的機密是各專案的 `.json` 內容，那些只放在受限的共用雲端硬碟資料夾，**不要**把機密 `.json` 上傳到這個公開 repo。

---

## 技術摘要

- 純前端：HTML + CSS + 原生 JavaScript，單一 `index.html`。
- 相依：`html2canvas`（匯出 PNG）、Google Identity Services、Google Picker / Drive API。
- 發佈：GitHub Pages（`main` 分支，根目錄）。
