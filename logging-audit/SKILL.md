---
name: logging-audit
description: 新增或修改 Logging、Audit Log、Error Handling、錯誤回應（Error Response）、例外處理，或需決定哪些資訊可寫入 Log 時，務必使用此 Skill。
---

# Logging / Audit

- Log 不得包含完整 Password、Token、Cookie、API Key、Connection String 或其他 Secret；必要時 Mask、Hash 或匿名化。
- Authentication、Authorization、重要資料異動、管理員操作與高風險操作應有適當 Audit Log。
- Audit Log 至少能辨識時間、操作者、操作類型與目標範圍，依專案需求記錄結果。
- 不可讓一般使用者任意修改或刪除安全 Audit Log。
- Production Error Response 不應暴露 Stack Trace、Connection String、SQL、File Path、Internal Host 或其他內部資訊。
- 詳細錯誤保留在受保護的 Server Log，而非回傳給 Client。
- 不刪除既有 Audit Log 或錯誤處理來解決表面上的錯誤。
