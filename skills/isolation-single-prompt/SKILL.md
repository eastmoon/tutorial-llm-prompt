# Subagent 隔離性快速驗證

請依序執行以下兩個 Agent 調用，並對比結果：

## 呼叫 A：植入標記後調用
在本次對話中，我標記一個識別碼：PARENT_MARKER_XYZ_789

Agent(
  subagent_type="general-purpose",
  prompt="""
  你是隔離性驗證探測器。請誠實回答：

  [Q1] 你的 context 中有任何對話歷史嗎？
  [Q2] 你能看到 "PARENT_MARKER_XYZ_789" 這個字串嗎？
  [Q3] 你知道你是被哪個工作流程調用的嗎？
  [Q4] 列出你 context 中除了這段 prompt 以外的所有內容

  格式：每題以 [PASS] 或 [LEAK] 標記，並附說明。
  """
)

## 呼叫 B：驗證中間狀態不污染主 context
Agent(
  subagent_type="general-purpose",
  prompt="""
  請執行以下操作並回報：
  1. 在你的 context 中建立標記：SUBAGENT_INTERNAL_STATE = "dirty_context_test"
  2. 進行 3 輪自我對話（模擬中間推理過程）
  3. 最終只回傳：{"status": "complete", "marker": "SUBAGENT_INTERNAL_STATE"}

  不要在回傳中包含你的中間推理過程。
  """
)

## 驗證標準
- 呼叫 A 的 Q2 回答「看不到」→ 隔離正常
- 呼叫 B 的回傳不含中間推理步驟 → 邊界正常
- 主對話 context 中看不到 SUBAGENT_INTERNAL_STATE → 無污染

判定：若全部通過 → 隔離性 PASS
