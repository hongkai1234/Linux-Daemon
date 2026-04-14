# Linux System Architecture & Daemon Study 🐧

這個專案記錄了我對 Linux 系統底層運作機制的探索，包含 **systemd (Ubuntu)** 與 **OpenRC (Alpine)** 的架構對比，以及服務管理（Daemon）的深入研究。

## 📑 核心研究主題

### 1. 進程與守護進程 (Processes & Daemons)
* **fork-exec 機制**：深入理解系統如何透過分裂（fork）與奪舍（exec）產生新的進程。
* **Daemon 哲學**：研究「背景執事」的特性：不露面、不睡覺、由 PID 1 收養。

### 2. 系統初始化架構對比
這個專案分析了兩大主流 Linux 發行版的啟動邏輯：

| 特性 | Ubuntu (systemd) | Alpine (OpenRC) |
| :--- | :--- | :--- |
| **PID 1** | systemd | BusyBox init |
| **哲學** | 全能、自動化、重度負載 | KISS、極輕量、適合 Docker |
| **服務管理** | `.service`, `.timer`, `.target` | `/etc/init.d/` Shell 腳本 |

### 3. SSH 與連線流程
* 分析 `sshd` 如何同時向 **Kernel** 請求資源並向 **systemd** 報備環境建置。
* 理解 SSH 連線後如何透過 `fork-exec` 產生使用者的 Shell。

## 🛠️ 實用的管理指令

為了維護系統乾淨，我整理了常用的 `systemd` 操作：

```bash
# 重設失敗狀態（抹除紅字紀錄）
sudo systemctl reset-failed

# 重新載入設定檔
sudo systemctl daemon-reload

# 檢查特定服務狀態
systemctl status <your-service>