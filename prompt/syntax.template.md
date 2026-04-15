---
name: 樣板
description: 樣板句使用方式的語法範本。
allowed-tools: Read, Bash
---

## 輸入

```text
$ARGUMENTS
```

你 **必需** 考慮輸入內容，若輸入為空，則終止流程，並呈現錯誤訊息。

## 目標

基於輸入內容解釋為輸出內容。

## 執行步驟

解釋 ARGUMENTS，並輸出到 EXPLAIN_OUTPUT。

## 回應規則

+ 使用 Markdown 格式
+ 禁止出現「當然！」「好的！」等開場白
+ 只輸出格式內容，不加任何前言

## 回應

務必基於回應規則，並嚴格遵循 `./template/response.md` 格式，不得增減任何內容。

## 錯誤

務必基於回應規則，並嚴格遵循 `./template/error.md` 格式，不得增減任何內容。
