---
name: While 迴圈
description: While 迴圈句使用方式的語法範本。
---

<!-- 定義迴圈符號 -->
## 定義

+ **[ITERATE_OVER]**：要迭代的集合或範圍
+ **[FOR_EACH]**：每次迭代對單一項目執行的動作
+ **[UNTIL]**：終止條件（處理完畢 / 達到數量 / 條件成立）

## 變數

CONTENT = |
    虛擬化系統運用諸多不同的系統。
    Docker 是一個容器管理系統。
    它可以幫你管理很多容器。
    用起來很方便。
CRITERIA = [
    說明需包含核心功能（調度、自愈、擴展）,
    不使用「很」等模糊形容詞,
    字數在 50 至 100 字之間,
    需包含至少一個具體使用場景
  ]
MAX_ROUNDS = 3

## 任務

**[ITERATE_OVER]** `$CONTENT` 的精煉過程

**[FOR_EACH]** 每輪迭代 `$ROUND`（從第 1 輪開始）：

1. 標示當前輪次：`[第 $ROUND 輪 / 最多 $MAX_ROUNDS 輪]`
2. 對照 `$CRITERIA` 逐條檢查 `$CONTENT`
3. 列出未通過的條件
4. 輸出修正後的 `$CONTENT`
5. 重新檢查所有 `$CRITERIA`

**[UNTIL]** 滿足以下任一條件即停止：
- 所有 `$CRITERIA` 全部通過 → 輸出 `✅ 精煉完成`
- `$ROUND` 達到 `$MAX_ROUNDS` → 輸出 `⚠️ 已達最大輪次，列出仍未通過的條件`
