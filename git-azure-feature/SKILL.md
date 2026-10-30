---
name: git-azure-feature
description: 當使用者要新增功能並推送到 Azure DevOps 時，依照固定的 git 分支流程操作（列出遠端分支、fetch、從 Production 建立新功能分支、本地開發與提交、推送並設定 upstream）。適用於「新增功能上 git 到 Azure」的場景。由 AI 決定分支名稱與 commit message 時，必須先檢查 git diff 了解程式碼變更，再決定，不得照舊或套用範例。
---

# Azure DevOps 新增功能分支流程技能

## 何時使用

當使用者要：
- 新增一個功能（feature / bugfix / hotfix 等分支）
- 以遠端 `Production` 分支為起點建立新分支
- 將開發完成的程式碼提交並推送到 Azure DevOps
- 第一次推送時設定 upstream 追蹤

關鍵字：`git`、`Azure DevOps`、`Production`、`推送`、`新增功能分支`、`feature branch`、`push -u`

## 前提條件

- 本機已 clone 專案，且遠端名稱通常為 `origin`（Azure DevOps 遠端）。
- 使用者已有對 Azure DevOps 遠端的讀寫權限。
- 新分支名稱建議使用 `feature/xxx`、`bugfix/xxx` 格式（例如 `feature/order-export`、`bugfix/close-xls`）。

## 操作步驟

### 步驟 1：列出所有遠端分支，確認 Production 存在

```bash
git branch -r
```

預期輸出應包含類似下列行：

```
  origin/HEAD -> origin/main
  origin/main
  origin/Production
```

確認有 `origin/Production` 這一行後，再進行下一步。

> 遠端名稱（如 `origin`）取自 `git branch -r` 輸出中斜線前面的部分，**不要填入完整的 Azure DevOps URL**。

### 步驟 2：取得遠端最新狀態

```bash
git fetch origin
```

這會同步遠端的所有分支與標記，確保本機看到的 `origin/Production` 是最新狀態。

### 步驟 3：以遠端 Production 為起點建立新功能分支

```bash
git checkout -b <新分支名稱> origin/Production
```

範例：

```bash
git checkout -b feature/order-export origin/Production
```

或

```bash
git checkout -b bugfix/close-xls origin/Production
```

成功後會切換到新分支，並自動設定追蹤關係（若遠端分支存在）。

### 步驟 4：開發、提交

在新分支上進行程式碼開發，完成後提交：

```bash
git add .
git commit -m "<分支類型>: <簡短說明>"
```

範例：

```bash
git commit -m "OrderExport: 新增訂單匯出功能"
```

**AI 決定分支名稱與 commit message 時的規則：**
- 必須先執行 `git diff` 查看實際程式碼改了什麼，再決定分支名稱與 commit message。
- 不得照舊或直接套用範例（如 `feature/order-export`、`OrderExport: 新增訂單匯出功能`）。
- 分支名稱與 commit message 應反映實際變更內容（例如 `bugfix/orderimport-numeric-validation` / `DOCAI/OrderImport: 數值清洗改用 decimal.TryParse...`）。

提交前請檢查：
- 不要誤提交暫存檔、建置輸出、個人環境設定（如 `.env`、`user.secrets`）。
- 若涉及 Commit、PR 或新增設定檔，請先執行 `commit-secret-scan` Skill 檢查是否有密碼、Token、API Key、連線字串等敏感資訊進入 Git History。

### 步驟 5：推送到 Azure DevOps

```bash
git push -u origin <新分支名稱>
```

範例：

```bash
git push -u origin feature/order-export
```

`-u` 只在**第一次推送該分支**時需要，用於設定 upstream 追蹤；之後推送可簡化為 `git push`。

成功後遠端會回覆類似訊息，並可依需求在 Azure DevOps 建立 Pull Request。

## 常見錯誤與注意事項

| 情境 | 正確做法 | 禁止做法 |
|------|----------|----------|
| 遠端名稱 | 使用 `origin`（或 `git branch -r` 斜線前的部分） | 手動貼入 Azure DevOps 完整 URL |
| 建立分支基準 | `origin/Production` | 直接從本機 `main` 或當前分支分出 |
| 第一次推送 | `git push -u origin <分支>` | 僅 `git push`（會失敗，因未設定 upstream）|
| 敏感資訊 | Commit 前掃描，不致入原始碼 | 將密碼、API Key、連線字串寫入程式碼 |

## 與 AGENTS.md 的關係

- 本 Skill 是領域細則，不得與專案根目錄 `AGENTS.md` 的安全底線衝突；衝突時以 `AGENTS.md` 為準。
- Commit 相關操作受 `AGENTS.md` 的「Git」與「高風險操作與人工確認」規則約束，涉及 Production、不可逆變更、正式環境 Credential、外部資料傳輸時，須先取得使用者明確要求並確認目標與影響範圍。
