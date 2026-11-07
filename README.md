# ComputerClean

用 Claude Code 檢查並調校 Windows 11 電腦，讓程式執行更順暢。所有調整都可以還原。

## 檔案

| 檔案 | 用途 |
|---|---|
| `電腦調校指南.md` | 給 Claude Code 看的作業說明，內容有原則、檢查項目、判讀標準、調校動作和注意事項。可以帶到任何一台電腦使用 |
| `CLAUDE.md` | 這個資料夾的 Claude Code 專案說明，包括登錄機碼的細節和已知限制 |
| `backup/` | 修改前的登錄檔備份。只適用原本那台電腦，所以**不放進 git**（見 `.gitignore`） |

## 在其他電腦上使用

1. 把 `電腦調校指南.md` 複製到新電腦上的任一資料夾。
2. 在那個資料夾裡啟動 Claude Code。
3. 跟它說：「照 `電腦調校指南.md` 檢查並調校這台電腦」。

Claude Code 會先只讀取、檢查，向你報告之後，等你同意才會修改，而且改之前會先備份。

## 目前進度（第一台電腦，2026-11-06 ～ 11-07）

**已完成**
- 停用開機自動啟動：Edge ×3、Adobe Acrobat 同步、Teams、Cisco Webex
- 清理暫存檔：約 1 GB
- 視覺效果改為最佳效能（保留字型平滑和縮圖），並關閉透明效果
- 61 個一般 App 的背景執行改為「永不」；系統元件和 Defender App 維持原樣
- 解除安裝：Bonjour、McAfee WebAdvisor、McAfee Security App
- 瀏覽器：Chrome 開啟記憶體節省（最大）並減少擴充功能；Edge 關閉啟動加速和背景執行，並確認關閉後不再常駐

**成果**（重開機後實測）

| 項目 | 調校前 | 調校後 |
|---|---|---|
| 可用記憶體 | 7.3 GB | 10.0 GB |
| 處理程序數 | 269 | 220 |

**尚未處理**（需要系統管理員權限）
- Logitech Download Assistant
- Java 更新排程（jusched）

這兩項可以到「工作管理員 → 啟動應用程式」停用，被擋下來的話就要請 IT 處理。

**刻意保留**
- Trend Micro Apex One、SecurityHealth、SysMain、Windows Search
- DTS 音效服務（關掉可能會影響喇叭音質）
