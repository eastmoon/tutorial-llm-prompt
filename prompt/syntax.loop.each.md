---
name: ForEach 迴圈
description: ForEach 迴圈句使用方式的語法範本。
---

<!-- 定義迴圈符號 -->
## 定義

**[ITERATE_OVER]**：要迭代的集合或範圍
**[FOR_EACH]**：每次迭代對單一項目執行的動作
**[UNTIL]**：終止條件（處理完畢 / 達到數量 / 條件成立）
**[FORMAT]**：迭代間輸出格式設定

## 變數

ITEM = [markdown | json | plain]

## 任務

**[ITERATE_OVER]** `$ITEMS` 清單中的每一個項目

**[FOR_EACH]** 單一項目執行以下步驟：
1. 標示項目編號（第 N 項，共 M 項）
2. 解釋內容
3. 提供使用時機

**[UNTIL]** `$ITEMS` 清單中所有項目皆處理完畢

**[FORMAT]** 每個項目之間以分隔線 `---` 區隔
