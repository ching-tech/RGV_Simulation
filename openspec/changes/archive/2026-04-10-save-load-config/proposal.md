## Why

目前所有設定（RGV 參數、軌道、庫位）僅儲存在 localStorage，無法在不同裝置或瀏覽器之間分享，也無法備份不同場景的設定。使用者需要能將目前全部參數匯出為檔案，並在任何環境下重新載入。

## What Changes

- 新增「匯出設定」功能：將目前 RGV、軌道、庫位全部參數下載為 `.json` 檔
- 新增「匯入設定」功能：讀取 `.json` 檔並還原至 App 狀態
- 匯入時做基本驗證，避免格式錯誤的檔案造成 crash
- UI 入口放在 Header 列，不佔用側欄空間

## Capabilities

### New Capabilities
- `config-file-io`: 將 AppState（rgv、track、storages）序列化為 JSON 檔案下載，以及從 JSON 檔案讀取並還原狀態

### Modified Capabilities
<!-- none -->

## Impact

- `src/App.tsx` — Header 新增匯出/匯入按鈕
- `src/context/AppContext.tsx` — 新增 `LOAD_CONFIG` action，接受完整 state payload 並還原
- 不需要新增依賴（使用瀏覽器原生 File API + `<a download>` 機制）
