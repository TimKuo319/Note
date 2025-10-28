## 問題發生原因
### 什麼是 Execution Policy？

Execution Policy（執行原則）是 Windows PowerShell 的一個安全機制，用來控制哪些腳本可以被執行。它的目的是防止惡意腳本在不知情的情況下在你的電腦上執行。

### 為什麼 npm 會觸發這個問題？

當你在 Windows 上安裝 npm 套件時，npm 會在 `node_modules/.bin` 目錄下創建一些執行檔，包括：

- `.ps1` 檔案（PowerShell 腳本）
- `.cmd` 檔案（批次檔）
- 無副檔名的檔案（Unix-style）

當你在 PowerShell 中執行 npm 指令（例如 `npm start` 或 `npx`）時，PowerShell 會嘗試執行這些 `.ps1` 腳本檔案。如果你的 Execution Policy 設定過於嚴格，PowerShell 就會拒絕執行這些腳本。

## 解決方案 - 修改 Policy

網路上查到大多的方式會需要使用 admin 權限開啟 PowerShell 後進行 Policy 修改。

如果要在不提權的狀況下修改 Policy 的話只需要將 Scope 限縮到當前使用者即可

```PowerShell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```


