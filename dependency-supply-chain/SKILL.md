---
name: dependency-supply-chain
description: 新增、更新、移除任何第三方套件、Library、Framework、Package、Registry / Mirror 設定，或執行套件安裝 Script（pip、npm、NuGet、Maven 等）時，務必使用此 Skill。也適用於評估某個套件是否可信、是否該升級大型 Framework。
---

# 第三方套件與供應鏈

## 新增或更新前

- 確認專案目前的套件管理方式、版本策略與來源。
- 確認 Package Name、來源、維護狀態、版本與已知漏洞。
- 確認是否真的需要新增；能用既有套件解決時不增加依賴。
- 不從不明來源安裝套件；優先使用官方套件來源或專案認可的內部 Registry / Mirror。

## 供應鏈風險

- 注意 Dependency Confusion、Typosquatting、Slopsquatting 等風險。
- 不因名稱相似就安裝未驗證的套件；AI 建議的套件名稱也須先確認確實存在且可信。
- 遵守 lockfile、版本鎖定與 hash verification 規範。
- 不直接執行第三方套件提供的安裝 Script、Post-install Script 或不明指令，除非來源與必要性已確認。
- 套件新增或更新後，注意已知漏洞、來源可信度與名稱混淆。

## 升級

- 不任意升級大型 Framework 或核心套件，以免引入相容性與安全風險。
- 「安裝或更新未驗證的第三方套件」屬高風險操作，須先取得使用者確認。
