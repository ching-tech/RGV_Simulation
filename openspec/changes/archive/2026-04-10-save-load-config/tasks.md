## 1. AppContext — 新增 LOAD_CONFIG action

- [x] 1.1 `src/context/AppContext.tsx`：Action type 加入 `LOAD_CONFIG`，payload 為 `{ rgv, track, storages }`
- [x] 1.2 reducer 處理 `LOAD_CONFIG`：merge defaultState → parsed payload（含 storages migration），清除 lastResult/lastFlowResult/history

## 2. Header — 匯出/匯入按鈕與邏輯

- [x] 2.1 `src/App.tsx`：實作 `exportConfig()` — 組出 `{ version:1, rgv, track, storages }`，Blob 下載為 `rgv-config.json`
- [x] 2.2 `src/App.tsx`：實作 `importConfig()` — 建立隱藏 file input，讀取 JSON，驗證頂層 key，dispatch `LOAD_CONFIG`，錯誤時顯示警告
- [x] 2.3 `src/App.tsx`：Header 右側加入「匯出」「匯入」兩個按鈕，匯入錯誤時顯示短暫錯誤提示
