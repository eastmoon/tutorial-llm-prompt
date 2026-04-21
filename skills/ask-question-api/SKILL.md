---
name: ask-question-api
description: 基於 Agent API 語法設計 Workflow，以期待大語言模型能回答預其結果。
allowed-tools: Agent
---

## 定義

QUESTION = 'Google'

---

## 步驟

### 預設

Agent(
  subagent_type="explainer",
  description="不輸入任何提問",
  prompt=""
)

---

### 詢問

Agent(
  subagent_type="explainer",
  description="訊問指定問題",
  prompt="""
  寫入 '大語言模型' 到 QUESTION
  """
)

---

## 流程

預設 -> 詢問 -> 預設 -> 輸出 `✅ 執行完畢`

---
