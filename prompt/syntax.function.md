---
name: 函數
description: 函數語法使用方式範本。
---

## 函數庫

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] FORMAT_CODE_REVI

針對提供的程式碼執行以下步驟：
1. 指出潛在的錯誤或風險
2. 評估效能影響
3. 提供修正後的完整程式碼
4. 說明修正理由

---

## 任務

以下是一段 Python 程式碼。
**[CALL] FORMAT_CODE_REVI**

```
def get_user(id):
      return db.query(f"SELECT * FROM users WHERE id={id}")
```
