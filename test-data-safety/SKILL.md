---
name: test-data-safety
description: 撰寫、修改或執行測試、準備測試資料、建立 Fixture / Mock、設定測試環境、需要為了測試暫時調整 Authentication / CSRF / TLS 等設定，或測試失敗需判斷是測試還是系統問題時，務必使用此 Skill。
---

# 測試與資料安全

- 測試資料盡量使用合成資料、匿名化資料或最小必要資料。
- 測試環境不使用正式環境 Credential 或未經處理的正式資料；開發、測試、Staging、Production 的 Credential 與資料保持隔離。
- 若確有必要使用正式資料衍生內容，須先匿名化或遮罩；不得直接複製到本機、測試或第三方服務。
- 不因測試方便而關閉 Authentication、Authorization、CSRF、TLS Verification 等安全機制；若測試環境確實需要替代方案，限定在測試範圍並明確標示。
- 不因測試方便而修改正式邏輯或加入臨時後門。
- 測試失敗時，不直接假設安全機制是錯的；先確認測試本身是否符合系統設計。
- 不主動執行測試，除非使用者明確要求（見 `AGENTS.md`）；若未執行，回覆中須明確說明。
