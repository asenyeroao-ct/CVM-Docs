# CVM colorBot 安裝指南

共享指南索引: [README.md](./README.md)

本指南將協助您完成 CVM colorBot 的完整安裝與設置流程。

## 前置條件

在開始安裝 CVM colorBot 之前，請確保您的系統滿足以下要求：

- **作業系統**: Windows 10 或更高版本
- **Python**: Python 3.11 或更高版本（推薦 Python 3.11.9）
- **網路連線**: 用於下載依賴套件（首次安裝時）

如果您尚未安裝 Python，請先參考 [Python 安裝指南](./Python-install.md) 完成 Python 的安裝。

## 第 1 步: 下載專案

1. 從 GitHub 下載或克隆 CVM colorBot 專案。
2. 將專案解壓縮到您選擇的目錄（例如 `D:\CVM-colorBot-main`）。
3. 確認專案目錄中包含以下檔案：
   - `main.py`（主程式入口）
   - `setup.bat`（自動安裝腳本）
   - `run.bat`（應用程式啟動腳本）
   - `requirements.txt`（依賴套件清單）

## 第 2 步: 執行自動安裝腳本

1. 在專案根目錄中找到 `setup.bat` 檔案。
2. 雙擊執行 `setup.bat`，或在命令提示字元中執行：
   ```bash
   setup.bat
   ```

3. 安裝腳本會自動執行以下操作：
   - 檢查 Python 是否已安裝並在 PATH 中
   - 創建虛擬環境（`venv` 資料夾）
   - 升級 pip 到最新版本
   - 安裝所有必要的依賴套件

4. 等待安裝完成。安裝過程可能需要幾分鐘，取決於您的網路速度。

## 第 3 步: 驗證安裝

安裝完成後，請確認以下項目：

1. **檢查虛擬環境**：
   - 確認專案目錄中已創建 `venv` 資料夾
   - 確認 `venv\Scripts\` 目錄存在

2. **檢查依賴套件**（可選）：
   在命令提示字元中執行：
   ```bash
   venv\Scripts\activate
   pip list
   ```
   您應該能看到以下主要套件：
   - `customtkinter`
   - `numpy`
   - `opencv-python`
   - `pyserial`
   - `mss`
   - 以及其他依賴套件

## 第 4 步: 啟動應用程式

安裝完成後，您可以透過以下方式啟動 CVM colorBot：

### 方法 1: 使用啟動腳本（推薦）

1. 雙擊專案根目錄中的 `run.bat` 檔案。
2. 應用程式會自動使用虛擬環境中的 Python 啟動。

### 方法 2: 手動啟動

1. 開啟命令提示字元（CMD）或 PowerShell。
2. 切換到專案目錄：
   ```bash
   cd D:\CVM-colorBot-main
   ```
3. 啟動應用程式：
   ```bash
   venv\Scripts\python.exe main.py
   ```

## 第 5 步: 首次配置

首次啟動 CVM colorBot 時，建議進行以下基本配置：

1. **選擇擷取模式**：
   - 在 `General` 標籤頁中選擇適合的擷取模式
   - 可選模式：`UDP`、`NDI`、`Capture Card`、`MSS`
   - 詳細設置請參考對應的設置指南：
     - [OBS UDP 設置](./OBS-UDP-setup.md)
     - [MAKCU 設置](./MAKCU-setup.md)

2. **配置滑鼠 API**：
   - 在 `General` 標籤頁中選擇 `Input API`
   - 如果使用 MAKCU 設備，選擇 `Serial`（除非是 `3.8 left (firewall)` 固件或 `makxdV2` 硬體，則選擇 `MakV2Binary`）
   - 配置相關連接參數（如 COM 埠、波特率等）
   - 詳細設置請參考 [MAKCU 設置](./MAKCU-setup.md)

3. **調整基本設定**：
   - 注意：FOV 大小是在 OBS 中調整的（參考 [OBS UDP 設置](./OBS-UDP-setup.md) 和 [FOV Size 對照表](./FOV-size.md)），而非在 CVM 中設定
   - 在 CVM 中配置瞄準模式和參數
   - 根據需要啟用或停用各項功能

## 故障排查

### 問題 1: Python 未找到

**錯誤訊息**: `[ERROR] Python is not installed or not in PATH`

**解決方法**:
1. 確認 Python 已正確安裝（執行 `python --version` 檢查）
2. 確認 Python 已加入系統 PATH
3. 參考 [Python 安裝指南](./Python-install.md) 重新安裝 Python

### 問題 2: 虛擬環境創建失敗

**錯誤訊息**: `[ERROR] Failed to create virtual environment`

**解決方法**:
1. 確認有足夠的磁碟空間
2. 確認專案目錄有寫入權限
3. 嘗試以系統管理員身分執行 `setup.bat`
4. 檢查 Python 安裝是否完整

### 問題 3: 依賴套件安裝失敗

**錯誤訊息**: `[ERROR] Failed to install dependencies`

**解決方法**:
1. 檢查網路連線是否正常
2. 確認 pip 已更新到最新版本：
   ```bash
   python -m pip install --upgrade pip
   ```
3. 嘗試手動安裝依賴套件：
   ```bash
   venv\Scripts\activate
   pip install -r requirements.txt
   ```
4. 如果特定套件安裝失敗，可以嘗試單獨安裝：
   ```bash
   pip install <套件名稱>
   ```

### 問題 4: 應用程式無法啟動

**錯誤訊息**: `[ERROR] Program exited with error`

**解決方法**:
1. 確認虛擬環境已正確創建
2. 確認所有依賴套件已正確安裝
3. 檢查 `config.json` 檔案是否存在且格式正確
4. 查看控制台輸出的詳細錯誤訊息
5. 確認系統滿足所有前置條件

### 問題 5: 模組導入錯誤

**錯誤訊息**: `ModuleNotFoundError` 或 `ImportError`

**解決方法**:
1. 確認您使用的是虛擬環境中的 Python：
   ```bash
   venv\Scripts\python.exe main.py
   ```
2. 重新安裝缺失的套件：
   ```bash
   venv\Scripts\activate
   pip install <缺失的套件名稱>
   ```
3. 如果問題持續，嘗試重新執行 `setup.bat`

## 後續步驟

安裝完成後，建議您：

1. **閱讀相關設置指南**：
   - [OBS UDP 設置](./OBS-UDP-setup.md) - 配置 UDP 擷取模式
   - [MAKCU 設置](./MAKCU-setup.md) - 配置 MAKCU 硬體設備
   - [Kmbox Net 連接教學](./Kmbox-Net-setup.md) - 配置 Kmbox Net 設備
   - [FOV Size 對照表](./FOV-size.md) - 了解 FOV 尺寸設定

2. **熟悉應用程式介面**：
   - 探索各個標籤頁的功能
   - 了解各項配置選項的用途
   - 測試基本功能是否正常運作

3. **優化設定**：
   - 根據您的硬體和需求調整參數
   - 保存配置檔案以便日後使用
   - 測試不同模式以找到最適合的設定

## 注意事項

- **僅支援 Windows 系統**: CVM colorBot 專為 Windows 設計，不支援其他作業系統
- **定期更新**: 建議定期檢查並更新專案和依賴套件
- **備份配置**: 定期備份您的 `config.json` 和配置檔案
- **系統權限**: 某些功能可能需要系統管理員權限（如串口存取）

## 需要協助？

如果您在安裝過程中遇到問題：

1. 檢查本文檔的「故障排查」章節
2. 查看專案的 GitHub Issues
3. 確認您的系統和 Python 版本符合要求
4. 提供詳細的錯誤訊息以便診斷問題
