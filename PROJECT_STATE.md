# 專案狀態與跨裝置交接手冊 (PROJECT STATE)

> 本文件由 \work-session\ 技能自動維護。用於保障在更換電腦（辦公室桌機 ↔ 家用電腦 ↔ 筆電）或長時間中斷後，可在十秒內快速恢復專案記憶與進度。

---

## 專案核心目標 (Project Goals)

- **專案願景**：建構澳門特區 2014/2015 至 2024/2025 學年（第 1 至 11 屆）共 171 位獲獎卓越教師之最權威、最精準大數據庫，並轉化為供學校校董會與行政會直接決策、孵化卓越教師團隊之可視化互動戰略平台。
- **技術棧**：原生 HTML5 / CSS3 / Vanilla JavaScript + TailwindCSS (CDN) + Chart.js + FontAwesome 6 + Canvas API 白板畫筆互動系統（純前端、零構建、零後端依賴）。
- **核心交付物**：
  1. \index.html\：單頁式高階互動決策儀表板（含多維度數據過濾、Chart.js 圖表、11 屆 171 位教師完整資料庫、行政會路線圖、全屏演講投影片、高靈敏 Canvas 白板標註筆刷系統）。
  2. \README.md\：專案整體架構、背景知識與核心報告指引。
  3. \澳門卓越表現教師歷屆大數據統計與行政會決策手冊.md\：深度長篇行政決策報告。
  4. 結構化數據集：\macau_distinguished_teachers_raw.json\、\macau_distinguished_teachers_list.csv\。
  5. 雲端發布網址：GitHub Pages (\https://jtchen1225-a11y.github.io/macau-distinguished-teachers/\)。

---

## 當前進度階段 (Current Stage)

- **目前階段**：階段 4 - 數據分析與可視化擴展、TOP 6 名校競爭力雷達圖、第 12 屆趨勢預測看板與勝率診斷器已圓滿完成。
- **健康度**：極佳（Clean Tree，無編譯錯誤，純原生 JavaScript 載入極速）。
- **雲端部署狀態**：即將推送到 GitHub Pages 部署。

---

## 待辦事項清單 (Action Items / Checklist)

- [x] 第 1 至 11 屆 171 位卓越教師獲獎資料深度查核與清洗（學段、學科、校名標準化、不清晰者加註說明）。
- [x] 生成 JSON 與 CSV 格式之結構化資料庫檔案。
- [x] 撰寫完整「澳門卓越表現教師歷屆大數據統計與行政會決策手冊.md」。
- [x] 開發高質感暗黑會議風格之互動儀表板 `index.html`。
- [x] 整合「學校行政會——卓越教師孵化推進行動路線圖（六大支柱）」與「五大落地法則」。
- [x] 實現路線圖模組之收合與展開（支援 `localStorage` 記憶偏好與收合橫幅）。
- [x] 實現 11 頁全屏演講模式（包含快捷鍵導航 `←`/`→`/`Esc`、全屏切換）。
- [x] 實現高 DPI 視網膜級白板畫筆、螢光筆、橡皮擦、調色盤、復原與清除功能。
- [x] **【全新】三大發展時期學科佔比演進圖（前期 ➔ 中期 ➔ 近期 趨勢穿透）**。
- [x] **【全新】各學段主要學科獲獎結構交叉穿透圖（堆疊柱狀矩陣）**。
- [x] **【全新】全澳 TOP 6 名校六維核心競爭力雷達畫像（支援單校切換與六校綜合對比）**。
- [x] **【全新】第 12 屆（2025/2026 學年）卓越教師評審名額與賽道趨勢預測看板**。
- [x] **【全新】學校行政會「第 12 屆卓越教師申報勝率與競爭力診斷模擬器」**。
- [x] 部署上線至 GitHub Pages 並驗證各功能正常運作。
- [ ] （後續可選）未來新一屆（第 12 屆）名單公佈時之後續數據追加與維護更新。

---

## 跨電腦交接日誌 (Session Handover Logs)

### 📅 [2026-09-13 15:35] 雷達圖顯示問題排查與修復 (Bug Fix)
- **問題根因**：
  1. 演講白板 Canvas 元素（`#annotationCanvas`）位於頁面底部的 Modal 結構中，原腳本在頂層加載時直接對其綁定 `addEventListener`，導致拋出 `Cannot read properties of null (reading 'addEventListener')` 的未捕獲異常，中斷了後續代碼執行。
  2. 雷達圖資料與函數宣告原先放置於演講腳本之後，因上述異常導致 `radarSchoolData` 變量未初始化，進而使 `initRadarChart()` 拋出 `ReferenceError`，阻斷了雷達圖及勝率診斷器的渲染。
  3. CSS 層疊樣式中，全域 `.chart-card` 的白色背景覆蓋了雷達圖的暗色設定，導致視覺對比度異常。
- **修復措施**：
  1. 將白板 Canvas 繪圖引擎事件監聽封裝為 `initWhiteboardEngine()`，實施惰性安全初始化與防禦性空指針檢查。
  2. 將雷達圖資料定義、學校切換邏輯與診斷模擬器前移至 `DOMContentLoaded` 之前，確保完全獨立且最先加載。
  3. 強化 `.radar-chart-card` 與 `.advanced-analytics-section .chart-card` 的 CSS 特異性（Specificity），保持高質感暗色風格一致。
  4. 透過 Chrome Headless 自動化測試與 DOM 檢測，驗證 Canvas 0~6 全部成功掛載並正常繪製。
- **保留進度 / 未解卡點**：無任何卡點，代碼已推送到 GitHub main 分支（Commit: `f76f878`）。
- **下次開工建議入口**：
  - 開啟 GitHub Pages 網址驗證實機體驗：https://jtchen1225-a11y.github.io/macau-distinguished-teachers/
