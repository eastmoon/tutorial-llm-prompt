---
name: 生成檔案
description: 基於描述的規則生成檔案的語法範本。
allowed-tools: Write, Read, Bash
---

## 目標

基於執行時間建立目錄，並生成詩句與詞語。

## 執行步驟

### 1. 建立目錄

+ 基於現在時間生成時間戳記 CURRENT_TIMESTAMP。
+ 基於時間戳構成目錄 DEMO_DIR，其構成結構為 `cache/demo/$CURRENT_TIMESTAMP`。
+ 若 $DEMO_DIR 不存在，則建立此目錄。

### 2. 生成詩句檔案

+ 隨機自唐詩三百首中取得一首詩，並寫入 `$DEMO_DIR/poetry.md`

### 3. 生成詞語檔案

+ 隨機自宋詞三百首中取得一首詞，並寫入 `$DEMO_DIR/lyrics.md`
