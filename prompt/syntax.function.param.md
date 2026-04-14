---
name: 參數化函數
description: 參數化函數語法使用方式範本。
---

## Function Library（函數庫）

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] EXPLAIN_FOR_AUDIENCE

**參數定義：**

| 參數名稱 | 型別 | 必填 | 預設值 | 說明 |
|---------|------|------|--------|------|
| `$TOPIC` | string | ✅ | — | 要說明的概念名稱 |
| `$LEVEL` | enum | ❌ | 中級 | 初學者 / 中級 / 進階 |
| `$LANG` | string | ❌ | 繁體中文 | 回應語言 |

用 {{LANG}} 解釋 {{TOPIC}}。
根據 {{LEVEL}} 程度調整說明深度：
- 初學者：使用比喻，避免術語
- 中級：術語加簡短說明
- 進階：直接給技術細節，省略背景

---

## 任務

[CALL] EXPLAIN_FOR_AUDIENCE
  $TOPIC          = Kubernetes HPA
  $LEVEL          = 進階

[CALL] EXPLAIN_FOR_AUDIENCE
  $TOPIC          = Docker
