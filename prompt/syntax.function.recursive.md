---
name: 遞迴函數
description: 遞迴函數語法使用方式範本。
---

<!-- 定義帶參數的函數 -->
## Function Library（函數庫）

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] EXPLAIN_FOR_AUDIENCE

**參數定義：**

| 參數名稱 | 型別 | 必填 | 預設值 | 說明 |
|---------|------|------|--------|------|
| `$ITEMS` | string | ✅ | — | 項目名稱 |
| `$CRITERIA` | string | ✅ | — | 評估標準名稱 |

對 {{ITEMS}} 中的每一個項目：
1. 依照 {{CRITERIA}} 進行評估
2. 給出評分（1-5）
3. 說明理由
4. 提出改善建議

---

## 任務

[CALL] EXPLAIN_FOR_AUDIENCE
  $ITEMS          = 以下五個 API 端點設計
  $CRITERIA       = RESTful 設計原則

- GET /users/getAll
- POST /user/create
- DELETE /removeUser?id=1
