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

## Python 版本要求

- 開始 Python 相關工作前，先確認專案的 Python 版本要求，來源依序為
  `pyproject.toml`、`.python-version`、`runtime.txt`、README、既有環境。
- 實際使用的直譯器必須確認：
  - Anaconda / conda 環境名稱與實際路徑（不得硬套固定路徑）。
  - venv / .venv 是否啟用。
  - 執行 `python --version`（或 `python -V`）取得實際版本。
- 無法確認版本時，明確說明不確定，不猜測、不捏造版本號。

## requirements（必要）

- Python 專案必須維護 `requirements.txt`（或 `pyproject.toml`、lockfile）。
  - 新專案建立時，應同時建立 `requirements.txt`。
  - 依賴變更、套件升級或環境異動後，一併同步更新。
- 產出方式：以實際環境 `pip freeze` 產出完整清單，或列出主要直接依賴（含版本），
  並標註產生方式與時間。
- 不得以「小程式、測試用途、只有 venv」為由省略 `requirements.txt`。
- 需自訂 index 的套件（如 `torch==X.X+cuXXX`），
  於 requirements.txt 或 README 中註明對應 `--index-url`。

## 環境不符時的處理

- 當既有環境與專案需求不符（Python 版本、套件版本、缺套件）時，
  不得自行決定新增環境或修改既有環境，必須先詢問使用者：
  - A. 新增環境（新建 conda env / venv，較不影響既有專案）。
  - B. 修改既有環境（pip install / 升級，可能影響其他專案）。
- 詢問時同時說明兩者的影響範圍與取捨，等使用者確認後才執行。

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
