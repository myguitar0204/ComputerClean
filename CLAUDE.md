# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 專案性質

這不是程式碼專案（非 git repo、沒有 build/test）。它是用來**清理與調校這台 Windows 11 電腦**的工作目錄：調整開機啟動項目、背景程式、視覺效果等。一律以繁體中文與使用者對談。

完整的檢查與調校流程（可帶到其他電腦重複使用）寫在 `電腦調校指南.md`，執行調校時照它的步驟做。

## backup/

`backup/` 內是修改前匯出的登錄檔備份（UTF-16LE 編碼，需 `iconv -f UTF-16LE` 才能 grep）：

| 檔案 | 對應機碼 |
|---|---|
| `backup_startup_hkcu.reg` | `HKCU\...\Explorer\StartupApproved\Run` |
| `backup_startup_hklm.reg` | `HKLM\...\Explorer\StartupApproved\Run` |
| `backup_advanced.reg` | `HKCU\...\Explorer\Advanced` |
| `backup_desktop.reg` | `HKCU\Control Panel\Desktop` |
| `backup_visualfx.reg` | `HKCU\...\Explorer\VisualEffects` |
| `backup_personalize.reg` | `HKCU\...\Themes\Personalize`（透明效果） |
| `backup_background_apps.reg` | `HKCU\...\CurrentVersion\BackgroundAccessApplications`（背景應用程式） |

還原：`reg import backup\<檔名>.reg`（HKLM 需系統管理員權限）。修改其他機碼前，先用 `reg export` 存到 `backup/`。

## 開機啟動的運作方式（停用而非刪除，保持可還原）

- **傳統程式**：登錄在 `HKCU/HKLM\Software\Microsoft\Windows\CurrentVersion\Run`（與 `WOW6432Node`）。停用方式是在對應的 `Explorer\StartupApproved\Run` 寫入 12 bytes 二進位值，首位元組 `03`＝停用、`02`/`06`＝啟用：
  `Set-ItemProperty $sa '<名稱>' ([byte[]](3,0,0,0,0,0,0,0,0,0,0,0)) -Type Binary`
- **Store / AppX 程式**（Teams、M365 Copilot＝`Microsoft.MicrosoftOfficeHub`、Microsoft Start 等）：位於
  `HKCU:\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppModel\SystemAppData\<套件>\<TaskId>` 的 `State`（DWORD）：`2`＝啟用、`1`＝使用者停用、`0`＝未設定/停用。
- Teams 有兩處啟動（Run 機碼 + AppX `TeamsTfwStartupTask`），且 App 內「自動啟動 Teams」設定可能把它改回來。

## 已知限制

- 目前 session 非系統管理員：寫入 `HKLM` 會被拒（例如 Logitech Download Assistant），需請使用者以系統管理員 PowerShell 執行，或用工作管理員「啟動應用程式」分頁。
- `HKCU\...\Explorer\Advanced\TaskbarDa`（工作列小工具）被 Windows 的 UCPD 保護擋住，無法以程式修改；只能請使用者在 `ms-settings:taskbar` 手動關閉，或以系統管理員設定原則 `HKLM\SOFTWARE\Policies\Microsoft\Dsh\AllowNewsAndInterests = 0`。
- 防毒（Trend Micro Apex One）與 `SecurityHealth` 應保留，不要停用。
