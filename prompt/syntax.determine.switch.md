---
name: Switch 判斷
description: Switch 判斷句使用方式的語法範本。
---

## 輸入

```text
$ARGUMENTS
```

你 **必需** 考慮輸入內容規則
+ 輸入必需為數字
+ 若輸入為空，則隨機產生一個數字到 $ARGUMENTS

## 目標

基於輸入內容解釋為輸出內容。

## 執行步驟

根據 `$ARGUMENTS` 選擇對應方式：

| $ARGUMENTS | 執行方式 |
|--------------|---------|
| 奇數 | 執行 ```Y = X * 2```，輸出到 $RESULT |
| 偶數 | 執行 ```Y = X * X```，輸出到 $RESULT |

**輸入**：$ARGUMENTS
**輸出**：$RESULT
