---
name: commit-secret-scan
description: 準備 Commit、建立 PR、新增或修改設定檔（`.env`、config、CI 設定）、需要做 Secret 掃描或提交前檢查、或發現 Secret / Credential 可能已進入 Git History 時，務必使用此 Skill。
---

# Secret 掃描與提交前檢查

## 提交前

- 若專案已有 Secret Scanning、Pre-commit、CI Security Check 或等效機制，優先遵守既有流程。
- 若具備執行能力且任務涉及 Commit、PR 或新增設定檔，優先使用專案既有的 Secret Scan。沒有既有工具時，不得假設「沒有掃描就代表沒有 Secret」，並在回覆中說明未掃描。
- Commit 前確認沒有提交 Secret、Credential、`.env`、私人設定或大型暫存資料。
- 不因掃描誤報而停用整體 Secret Scan；應針對誤報做精確排除並保留其他檢查。

## 發現 Secret 時

- 先停止公開或提交該內容，不完整展示（只顯示遮罩後的部分）。
- 依專案流程進行遮罩、移除與必要的 Credential Rotation。
- 若 Secret 已進入 Git History，不只刪除目前檔案；視情況進行 Credential Rotation 與 History 清理。
- 修改 Git History、Force Push 屬高風險操作，執行前須依 `AGENTS.md` 確認。
