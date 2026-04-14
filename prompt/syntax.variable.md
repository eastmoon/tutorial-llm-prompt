---
name: 變數
description: 變數使用方式的語法範本。
allowed-tools: Read, Bash
---

## 輸入

```text
$ARGUMENTS
```

若輸入不為空，在開始處理前，你 **必需** 考慮輸入。

## 目標

基於輸入內容解釋為輸出內容。

## 執行步驟

### 1. 解釋

解釋 ARGUMENTS，並輸出到 EXPLAIN_OUTPUT。

### 2. 列舉

基於 ARGUMENT 來列舉 5 項優點，5 項缺點，並輸出到 LIST_OUTPUT

## 回應

### 格式規則

+ 使用 Markdown 格式
+ 禁止出現「當然！」「好的！」等開場白
+ 只輸出格式內容，不加任何前言

### 格式

務必嚴格遵循以下格式，不得增減任何內容。

## 簡介

『$EXPLAIN_OUTPUT』

## 優點與缺點

$LIST_OUTPUT
