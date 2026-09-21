---
name: database-safety
description: 撰寫、修改或執行 SQL、Stored Procedure、ORM 查詢、Database Schema、Migration、資料修復腳本，或處理餘額、庫存、點數、額度、配額、序號等有限資源、Transaction、Lock、批次 UPDATE / DELETE 時，務必使用此 Skill。只要使用者要求提供會修改或刪除資料的 SQL 指令或程式碼（UPDATE、DELETE、TRUNCATE、DROP、ALTER、MERGE 等），一律使用此 Skill，並先提供備份指令。
---

# SQL / Database 安全

## 修改 / 刪除前必須先提供備份

使用者要求提供會**修改或刪除資料**的 SQL 指令或程式碼時（`UPDATE`、`DELETE`、`TRUNCATE`、`DROP`、`ALTER`、`MERGE`、Schema Migration、資料修復腳本，以及會執行這些操作的程式碼），**必須先提供對應的備份指令或程式碼，再提供修改 / 刪除本身**。

### 回覆順序

1. **備份**：備份將被影響的資料。
2. **備份驗證**：比對備份筆數與預計影響筆數（例如 `SELECT COUNT(*)`），確認備份成功後才執行下一步。
3. **修改 / 刪除本身**：附上與備份相同的 WHERE 條件。
4. **還原方式**：提供從備份還原的指令或程式碼。

### 備份規則

- 備份範圍必須與修改範圍一致：使用與修改相同的 WHERE 條件；`DROP`、`TRUNCATE`、Schema 變更、無條件或大範圍操作則備份整張表（含結構）。
- 備份指令依專案實際使用的 DBMS 撰寫，不假設版本或語法；DBMS 不明時先詢問。常見寫法：
  - SQL Server：`SELECT * INTO <table>_bak_<yyyyMMdd_HHmm> FROM <table> WHERE ...`
  - MySQL / PostgreSQL / Oracle：`CREATE TABLE <table>_bak_<yyyyMMdd_HHmm> AS SELECT * FROM <table> WHERE ...`
- 備份表命名需含來源表名與時間，避免覆蓋既有備份，並與既有物件不衝突。
- 表內備份無法防範整個資料庫損毀；`DROP`、`TRUNCATE`、大量刪除、Schema 變更等不可逆或大範圍操作，建議額外做資料庫層級備份或匯出，並說明原因。
- 若以程式碼（例如 Python）提供，備份步驟必須寫在同一支程式中、先於修改執行，且備份失敗時中止，不得繼續修改。
- 備份含正式資料時，留在原本的資料庫 / 環境內，不複製到本機、測試環境或第三方服務（見 `AGENTS.md` 的 Secret 與資料外傳規則）。
- 備份不屬於「暫存內容」：不得依 `AGENTS.md` 的暫存清除規則自動刪除備份表或備份檔，由使用者決定何時刪除；回覆中告知備份的名稱與位置。

### 例外

- 純 `INSERT` 或 `SELECT` 不會損失既有資料，不需要備份（若可能影響既有資料，例如 `INSERT ... ON DUPLICATE` / `MERGE`，仍視為修改）。
- 使用者明確表示不需要備份時可以省略，但回覆中須簡短說明風險一次；影響 Production 時仍須依 `AGENTS.md` 確認目標與範圍。
- 若是由 Agent 直接執行（而非只提供指令），同樣必須先備份並驗證成功，且須符合 `AGENTS.md` 的授權與確認規則。

## 一般 SQL 規則

- 優先使用 Parameterized Query、ORM 或 Stored Procedure 的安全參數方式；禁止將使用者輸入直接串接進 SQL。
- Dynamic SQL 必須限制可接受的欄位、排序方向、Table / Column 名稱等內容（Allowlist）。
- 不使用無限制的 `SELECT *`、無條件大量查詢或無限制 Batch（備份時為完整保存資料而使用 `SELECT *` 除外）。
- `DELETE`、`UPDATE`、`TRUNCATE`、`DROP`、Schema Migration 等可能造成資料損失的操作，必須確認範圍與條件；大量 UPDATE / DELETE 須確認 WHERE 條件、影響筆數與交易策略，並先以 `SELECT` 預覽將被影響的資料。
- 涉及餘額、庫存、點數、額度、配額、序號或其他有限資源時，注意 Race Condition、Transaction、Lock 與 Idempotency。
- 優先採用可預覽、可回復或 Dry Run 的方式（例如包在 Transaction 中，確認影響筆數後才 `COMMIT`）。
- 修改 Database Schema、Migration 或資料修復腳本屬高風險操作，須符合 `AGENTS.md` 的確認規則。
