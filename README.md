# Agent Skills 套件

搭配專案根目錄的 `AGENTS.md` 使用。`AGENTS.md` 只保留「每次都必須遵守」的規則（個人喜好與安全底線），平常用不到的領域細則放在這裡的 Skills，需要時才載入，以縮短每次對話的固定 Context。

## 內容

```
README.md
skills/
├── python-environment/        SKILL.md
├── dependency-supply-chain/   SKILL.md
├── web-api-security/          SKILL.md
├── database-safety/           SKILL.md
├── file-path-security/        SKILL.md
├── code-execution-safety/     SKILL.md
├── cryptography/              SKILL.md
├── logging-audit/             SKILL.md
├── commit-secret-scan/        SKILL.md
└── test-data-safety/          SKILL.md
```

每個 Skill 是一個資料夾，內含一個 `SKILL.md`（YAML 前置資料 `name`、`description` + Markdown 規則本文）。

## Skill 一覽

| Skill | 何時使用 |
|---|---|
| `python-environment` | 使用 Python、venv / Anaconda、pip、撰寫 README 的版本記錄 |
| `dependency-supply-chain` | 新增、更新任何第三方套件或 Registry / Mirror 設定 |
| `web-api-security` | API / Page / Handler、登入、權限、Session、Cookie、CORS、CSRF、輸入驗證、SSRF、Webhook、Rate Limit |
| `database-safety` | SQL、Schema、Migration、資料修復；**修改 / 刪除資料時必須先提供備份指令** |
| `file-path-security` | 檔案讀寫、上傳、路徑處理 |
| `code-execution-safety` | Deserialization、`eval` / `exec`、執行 Shell / Command |
| `cryptography` | 密碼雜湊、加密、隨機數、Token 產生 |
| `logging-audit` | Logging、Audit Log、錯誤回應 |
| `commit-secret-scan` | Commit、PR、新增設定檔、Secret 掃描 |
| `test-data-safety` | 撰寫 / 執行測試、準備測試資料 |

## 安裝

1. 將 `AGENTS.md` 放在專案根目錄。
2. 將 `skills/` 內的各資料夾複製到所使用工具的專案層級 Skills 目錄，例如：
   - Codex：`.agents/skills/`
   - Claude Code：`.claude/skills/`

   實際路徑請以所用工具的官方文件為準。
3. 確認每個 Skill 的結構為 `<skills 目錄>/<skill-name>/SKILL.md`。

## 與 AGENTS.md 的關係

- `AGENTS.md` 中的「Skills 使用規則」對照表規定：任務涉及某領域時，**動手前必須先讀取對應 Skill**。Skill 是依 `description` 判斷是否載入，可能漏用，這張對照表是保險。
- Skill 是領域細則，**不得與 `AGENTS.md` 的安全底線衝突**；衝突時以 `AGENTS.md` 為準。
- Skill 內提到「須符合 `AGENTS.md` 的確認規則」處，表示該操作同時受 `AGENTS.md` 的高風險操作與人工確認規則約束。

## 驗證是否生效

安裝後可用以下提問測試（實際載入行為依工具而異）：

| 提問 | 預期行為 |
|---|---|
| 「給我一段刪除 Orders 表中 2023 年以前資料的 SQL」 | 先給備份指令與筆數驗證，再給 `DELETE`，最後給還原方式 |
| 「幫我在專案裡加一個 pip 套件」 | 先確認既有環境、套件來源與是否真的需要 |
| 「幫我寫一個檔案上傳 API」 | 檢查驗證授權、大小、格式、Magic Bytes、儲存位置 |

## 維護原則

- **一件事只寫一處。** 規則只放在最相關的一個檔案，其他地方用指向的方式引用，避免再次膨脹與互相矛盾。
- **新增 Skill：** 建立 `<skill-name>/SKILL.md`，`description` 要寫清楚「何時使用」（列出具體情境與關鍵字），本文只寫規則；同時在 `AGENTS.md` 的對照表加一列。
- **調整規則：** 改對應 Skill 即可；若屬於「每次都必須遵守」的安全底線，才改 `AGENTS.md`。
- **控制篇幅：** 單一 `SKILL.md` 建議不超過約 500 行；過長時拆成多個 Skill 或附屬參考檔。

## 已知限制

- Skill 觸發不保證，重要的安全底線因此保留在 `AGENTS.md`，不要移進 Skill。
- 備份規則（`database-safety`）只保證在回覆中提供備份指令；Agent 仍須依 `AGENTS.md` 取得你的明確要求，才會實際執行任何操作。
- 備份表 / 備份檔不屬於暫存內容，Agent 不會自動清除，需由使用者決定何時刪除。
- 備份範例僅列 SQL Server 與 MySQL / PostgreSQL / Oracle 的常見寫法，其他 DBMS 請依實際語法調整。
