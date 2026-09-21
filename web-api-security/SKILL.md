---
name: web-api-security
description: 新增或修改 Web API、Controller、Handler、Page、Service，或涉及登入、Authentication、Authorization、權限、角色、多租戶（Tenant / Company）、Session、Cookie、CSRF、CORS、輸入驗證、Rate Limit、Quota、SSRF（Server 代為請求 URL）、Webhook、SSO、API Contract 時，務必使用此 Skill。修改任何安全設定並需檢查相容性時也要使用。
---

# Web / API 安全

## Authentication / Authorization

- 所有需要保護的 API、Page、Handler、Service 都必須有適當的 Authentication / Authorization。
- 不可只依賴前端隱藏按鈕、Disabled、JavaScript 或 UI 權限控制；Server 必須重新驗證使用者權限。
- 不可信任 Client 提供的 `UserId`、`Role`、`CompanyId`、`TenantId` 等欄位來決定實際權限。
- 防止 IDOR / BOLA / Object-Level Authorization 問題。
- 防止 Mass Assignment / Over-posting：不要直接將 Client Model Binding 的完整物件當成可修改資料來源；敏感欄位如 Role、Permission、Owner、CompanyId、TenantId、IsAdmin、Status 等，應由 Server 明確決定或使用 Allowlist / DTO。
- 多租戶資料必須在 Server / Database 層正確限制 Tenant / Company 範圍。
- 若使用 Row-Level Security，必須確認實際查詢條件與 Session / Tenant Context 不會被使用者竄改。

## CSRF / Session / Cookie

- Cookie-based Authentication 的寫入操作應考慮 CSRF 防護。
- Session / Authentication Cookie 依用途正確設定 `HttpOnly`、`Secure`、`SameSite`。
- 不因除錯方便而關閉 CSRF、Cookie Security 或 Session Protection。
- Token 不放在 URL Query String。

## CORS

- 不使用 `AllowAnyOrigin`、`*` 等過度寬鬆的設定來解決前端問題。
- 依實際需求限制 Allowed Origins、Methods、Headers 與 Credentials；不可同時使用 wildcard Origin 與 Credentials。

## Input Validation

- 來自 Client、URL、Header、Form、JSON、Cookie、File、Webhook、第三方 API 的資料都視為不受信任。
- Server 必須驗證型別、格式、長度、範圍與業務規則；不以 Client-side Validation 作為唯一防線。
- 輸出到 HTML、JavaScript、SQL、Shell、Command、Log、URL 等不同 Context 時，使用對應的安全編碼或參數化方式。

## Rate Limit / Resource Control

- 對登入、驗證碼、寄信、檔案處理、昂貴 API、報表產生、批次任務等操作考慮 Rate Limit、Quota、Timeout 與 Cost Limit。
- 使用者端傳入的 Plan、Quota、Permission、Limit 不得直接作為 Server 的權威資料。
- 避免無限制的大型查詢、檔案、Request Body、Batch、Concurrency 或記憶體配置造成 DoS。

## SSRF

- Server 代替使用者存取 URL 時，驗證 Scheme、Host、Port、IP 與允許的目的地。
- 禁止存取內部網路、Loopback、Link-local、Metadata Service 等敏感位址。
- DNS Resolve 後仍須檢查實際 IP；每次 Redirect 都重新驗證目的地。
- 不因 URL 看起來合法就直接讓 Server 發出 Request。

## Webhook

- 不可只依賴 URL 不可猜測或來源 IP 判斷可信度。
- 第三方提供簽章時，驗證 HMAC / Signature、Timestamp、Nonce。
- 注意 Replay Attack、Duplicate Event 與 Idempotency。
- Webhook Payload 仍視為不受信任資料。

## 修改安全機制時的相容性

- 修改權限、Session、Cookie、CORS、CSRF、Encryption、SSO、API Contract 時，檢查相關 Client / Server / Integration，特別檢查既有相依功能。
- 不透過關閉安全功能來解決相容性問題；衝突時明確指出 Trade-off。
- 修改 Authentication、Authorization、ACL、IAM、Firewall、CORS、Security Policy 屬高風險操作，須符合 `AGENTS.md` 的確認規則。
