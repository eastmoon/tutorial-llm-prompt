---
name: ForRange 迴圈
description: ForRange 迴圈句使用方式的語法範本。
---

<!-- 定義迴圈符號 -->
## 定義

**[ITERATE_OVER]**：要迭代的集合或範圍
**[FOR_EACH]**：每次迭代對單一項目執行的動作
**[UNTIL]**：終止條件（處理完畢 / 達到數量 / 條件成立）
**[FORMAT]**：迭代間輸出格式設定

## 變數

START_INDEX = 1
FOR_RANGE = 5

## 任務

**[ITERATE_OVER]** 從 `$START_INDEX` 到 `$START_INDEX + $FOR_RANGE - 1`

**[FOR_EACH]** 當前計數值 `$I`：
- 在每個項目開頭標示 `[$I/$FOR_RANGE]`
- 說明一件 `1900 + $I` 的歷史事件

**[UNTIL]** `$I` 達到 `$START_INDEX + $FOR_RANGE - 1`
