---
name: file-path-security
description: 撰寫或修改會讀寫檔案、處理使用者提供的檔名 / 路徑 / URL Path、檔案上傳、下載、解壓縮、Upload Directory 設定的程式時，務必使用此 Skill。
---

# File / Path 安全

- 所有使用者提供的檔案名稱、資料夾名稱、Path、URL Path 都視為不受信任。
- 防止 Path Traversal，例如 `../`、`..\\`、Encoded Traversal、Symbolic Link 等。
- 讀寫檔案前確認實際 Canonical Path 仍位於允許的 Root Directory。
- 不直接將使用者輸入作為任意檔案系統路徑。
- 不允許使用者控制的檔案覆蓋系統檔、設定檔、執行檔或其他非預期檔案。
- File Upload 必須檢查 Authentication / Authorization、大小、格式、Magic Bytes、檔名、儲存位置與執行權限。
- 上傳檔案不可因副檔名而被信任為安全內容。
- 避免將 Upload Directory 設定為可直接執行 Script 的位置。
- 不因方便而使用任意檔案讀取、任意檔案寫入或任意路徑 Delete。
