---
name: ask-question-yaml
description: 基於 YAML 語法設計 Workflow，以期待大語言模型能回答預其結果。
---

## 定義

QUESTION = 'Google'

---

## 步驟

### 預設

+ step_id: "step-1"
+ agent: "explainer"
+ input_schema: ""

---

### 詢問

+ step_id: "step-2"
+ agent: "explainer"
+ input_schema: "QUESTION = '大語言模型'"

---

## 流程

預設 -> 詢問 -> 輸出 `✅ 執行完畢`

---
