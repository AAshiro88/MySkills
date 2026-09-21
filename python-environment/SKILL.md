---
name: python-environment
description: 使用或執行 Python、建立 / 修改 Python 專案、處理 venv、Anaconda、conda、requirements.txt、pyproject.toml、pip install、撰寫或更新 README 中的環境與版本說明時，務必使用此 Skill，即使使用者沒有明確提到「環境」。
---

# Python 環境與套件

## 使用前確認環境

- 先確認專案是否已有 `venv`、`.venv`、`requirements.txt`、`pyproject.toml`、`poetry.lock`、`uv.lock` 或其他環境規範。
- 優先使用專案既有虛擬環境與套件版本。
- 本機專案使用 Anaconda，優先檢查既有 Anaconda 環境；常見安裝位置為 `C:\ProgramData\Anaconda3`，不得在未確認實際環境前硬套固定路徑。
- 使用 `conda activate` 前，先確認目標環境名稱與專案既有設定，不任意建立或切換環境。
- 不在系統 Python 中任意安裝套件。

## 安裝套件

- 新增套件前確認是否真的需要；能用既有套件解決時，不增加依賴。
- `pip install` 使用 HTTPS 的 PyPI 或專案認可的內部 Mirror。
- 對 `git+https://...`、本機套件、未驗證來源等安裝方式，先確認來源與必要性。
- 優先遵守 lockfile、hash、版本鎖定與既有 dependency policy。
- 不因安裝失敗而自行更換成未驗證的套件來源。
- 其餘來源驗證、供應鏈風險規則，見 `dependency-supply-chain` Skill。

## 文件中的版本記錄

- Python 專案的 README 或環境說明文件，一律記錄實際使用的 Python 版本與主要套件版本。
- 版本號必須來自 `requirements.txt`、lockfile、實際環境或使用者明確提供的資訊，不得捏造或猜測 Python、Framework、Library 或 Package 版本號。
