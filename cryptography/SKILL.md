---
name: cryptography
description: 撰寫或修改密碼儲存與驗證、Password Hash、加密 / 解密、簽章、Key Management、Token / Reset Code / Session Secret 產生、隨機數，或任何 Encryption / Hashing 流程時，務必使用此 Skill。
---

# Cryptography

- 密碼不得使用 MD5、SHA-1 或自製 Hash 方式儲存。
- Password Hash 使用專門的演算法，例如 Argon2、bcrypt、scrypt，或專案既有且安全的 Password Hasher。
- 不自行設計加密演算法。
- 需要密碼學隨機性時，使用安全亂數來源，例如 Python `secrets` 或平台提供的 CSPRNG；不使用一般 Pseudo-random Generator 產生 Token、Reset Code、Session Secret 等安全敏感值。
- 不在未理解 Key Management、IV / Nonce、Mode、Rotation 與 Storage 的情況下自行修改加密流程。
- 修改既有 Encryption / Hashing 流程時，必須考慮舊資料相容性與 Migration；核心機制的修改須符合 `AGENTS.md` 的相容性與確認規則。
