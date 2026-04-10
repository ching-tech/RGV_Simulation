## Context

App 狀態（`AppState`）已定義在 `src/types/index.ts`，包含 `rgv`、`track`、`storages` 三個主要資料群，目前透過 `localStorage` 自動持久化。匯出/匯入功能是在此基礎上加一層「手動的」檔案 I/O，讓使用者可以攜帶設定檔。

## Goals / Non-Goals

**Goals:**
- 匯出：將 `{ rgv, track, storages }` 序列化為 JSON 並觸發瀏覽器下載
- 匯入：讀取使用者選擇的 JSON 檔案，驗證基本結構後還原至 App 狀態
- 不覆蓋歷史紀錄（`history`）和計算結果（`lastResult`、`lastFlowResult`）

**Non-Goals:**
- 不支援多存檔管理（只有單一匯出/匯入）
- 不做完整 schema 驗證（只檢查頂層 key 是否存在）
- 不支援舊版格式自動轉換

## Decisions

**D1: 匯出格式 — 純 JSON，包含版本標記**
```json
{
  "version": 1,
  "rgv": { ... },
  "track": { ... },
  "storages": [ ... ]
}
```
加入 `version` 欄位方便未來格式升級識別。

**D2: 下載機制 — `<a download>` + `URL.createObjectURL`**
不需要任何第三方套件，純瀏覽器 API。建立 Blob → 產生 object URL → 模擬點擊 → 釋放 URL。

**D3: 匯入機制 — `<input type="file" accept=".json">`**
隱藏的 file input，由按鈕觸發 `.click()`。讀取後用 `JSON.parse`，catch 錯誤後顯示警告訊息。

**D4: 狀態還原 — 新增 `LOAD_CONFIG` action**
AppContext reducer 加一個 action，接受 `{ rgv, track, storages }` payload，merge 進現有 defaultState（確保新欄位有預設值）。匯入後清除 `lastResult` 和 `lastFlowResult`。

**D5: UI 位置 — Header 右側**
放在 Header 的右側，不影響側欄空間。兩個小按鈕：「匯出」「匯入」。

## Risks / Trade-offs

- **舊版設定缺少新欄位**（如 `width`/`depth`）→ `loadState` 已有 migration 邏輯（spread defaultState 再 spread parsed），同樣套用於 `LOAD_CONFIG`
- **使用者匯入損壞的 JSON** → `try/catch` 捕捉，顯示「檔案格式錯誤」提示，不 crash
- **大量庫位的檔案大小** → 純文字 JSON，幾十個庫位也只有幾 KB，無問題
