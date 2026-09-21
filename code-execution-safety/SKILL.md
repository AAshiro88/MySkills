---
name: code-execution-safety
description: 撰寫或修改涉及 Deserialization（pickle、YAML、JSON / XML / Binary Serialization）、`eval`、`exec`、動態程式碼執行，或由程式執行 Shell、PowerShell、Command、Python 子程序的功能時，務必使用此 Skill。
---

# Deserialization / Code Execution

- 不使用不安全的 Deserialization 處理不受信任資料。
- 禁止對不受信任資料使用 `pickle.loads`、`eval`、`exec` 或等效任意程式碼執行方式。
- 使用 YAML 時，不得對不受信任內容使用不安全 Loader（例如使用 `safe_load`）。
- JSON / XML / Binary Serialization 應使用安全且有限制的反序列化方式。
- 不執行從使用者輸入、文件或外部內容取得的程式碼。
- Shell、PowerShell、Command、Python 等執行功能必須限制可執行內容、參數、權限與工作目錄；避免以字串串接方式組成指令。
