---
name: 生成論文檔案
description: 基於描述的規則生成論文檔案的語法範本。
allowed-tools: Write, Read, Bash
---

## 目標

基於論文內容生成一個符合論文格式的簡易報告。

## 步驟

### 1. 解釋

+ 取得  [Attention Is All You Need](https://arxiv.org/pdf/1706.03762) 的論文內容。
+ 自論文內容中完整擷取論文摘要 ( Abstract )，寫入 PAPER_ABSTRACT。
+ 自論文內容中完整擷取論文結論 ( Conclusion )，寫入 PAPEER_CONCLUSION。
+ 自論文內容中盡量取得各章節標題、章節描述。
  - 忽略摘要 ( Abstract )、結論 ( Conclusion )、關聯 ( References )章節

---

### 2. 生成

### 格式規則

+ 使用 Markdown 格式
+ 禁止出現「當然！」「好的！」等形容詞的開場白
+ 禁止出現第一人稱的文句
+ 只輸出格式內容，不加任何前言

### 格式

務必嚴格遵循以下格式，不得增減任何內容

## 摘要 ( Abstract )

**$PAPER_ABSTRACT**

解釋 $PAPER_ABSTRACT

## 簡介 ( Introduction )

+ **章節標題**
用 200 字解釋章節描述

## 結論 ( Conclusion )

**$PAPEER_CONCLUSION**

解釋 $PAPER_ABSTRACT

---

### 3. 輸出

+ 將生成內容寫入 `./cache/paper.md`
+ 若檔案已經存在則覆蓋舊內容
+ 寫入成功 → 輸出 `✅ 論文彙整完畢`
+ 寫入失敗 → 輸出 `⛔ 論文彙整失敗`，說明失敗原因
