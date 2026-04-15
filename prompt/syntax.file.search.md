---
name: 搜尋檔案
description: 基於規則搜尋目錄中檔案的語法範本。
allowed-tools: Read, Bash
---

## 目標

搜尋特定目標內的指定內容，並進行評估與分析。

## 定義

+ DEMO_ROOT_DIR 為範本根目錄路徑，其路徑為 `cache/demo`
+ DEMO_DIR 為範本目錄路徑，其路徑為 `$DEMO_ROOT_DIR` 下的子目錄
+ DEMO_NAME 為範本名稱，其名稱為 `$DEMO_ROOT_DIR` 下的子目錄的名稱

## 執行步驟

### 1. 搜尋目標目錄

+ 搜尋為於 $DEMO_ROOT_DIR 的目錄，並取得目錄位置 ( DEMO_DIR ) 、目錄名稱 ( DEMO_NAME )
+ 目錄位置內必須包括 lyrics.md 與 poetry.md，若無檔案則該目錄應忽略
+ 目錄名稱為一串數字，此數字為時間戳記，請還原為實際時間，並寫入目錄標題 ( DEMO_TITLE )

### 2. 分析與評估

+ 移動到 $DEMO_DIR 下
+ 讀取 poetry.md 並分析文句意義，總結成 POETRY_RESULT
+ 讀取 lyrics.md 並分析文句意義，總結成 LYRICS_RESULT

務必嚴格遵循以下格式，不得增減任何內容。

## 分析與總結 : $DEMO_TITLE

+ 唐詩：POETRY_RESULT
+ 宋詞：LYRICS_RESULT
