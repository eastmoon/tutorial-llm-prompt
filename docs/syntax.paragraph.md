# 段落

大語言模型處理提示詞時，並非逐字執行，而是將整個輸入建構成語義上下文圖，段落的寫法直接影響模型對以下三件事的判斷：

1. 這段文字的「角色」是什麼？（指令 / 資料 / 範例 / 背景）
2. 這段文字的「優先級」是什麼？（必須遵守 / 參考 / 可忽略）
3. 這段文字的「作用範圍」是什麼？（全局 / 本段 / 單次）

## 段落的語義角色類型

模型會根據段落的寫法，自動將其歸類為不同角色：

```
<!-- 角色一：指令段落 -->
## Instruction
請用繁體中文回答，每段不超過三句話。
<!-- 模型解讀：這是我必須執行的行為規範 -->

<!-- 角色二：背景資料段落 -->
## Context
這個系統部署在 AWS 上，使用 EKS 管理容器叢集。
<!-- 模型解讀：這是我可以引用的參考資訊 -->

<!-- 角色三：範例段落 -->
## Example
輸入：什麼是 Pod？
輸出：Pod 是 Kubernetes 中最小的部署單位。
<!-- 模型解讀：這是我應該模仿的輸出格式與風格 -->

<!-- 角色四：使用者輸入段落 -->
## User Input
請解釋 HPA 的運作原理。
<!-- 模型解讀：這是我當前需要處理的任務 -->
```

## 段落順序對模型的影響

模型對段落位置有明確的權重傾向：

+ System Prompt 開頭段落
  - 用於「角色定義、格式規則、硬性限制」
  - 權重最高，設定全局行為
+ 中間段落
  - 用於「背景資料、判斷邏輯、變數定義」
  - 權重中等，提供背景與規則
+ 結尾段落 / 使用者輸入
  - 用於「當前任務、具體問題」
  - 權重次高，觸發執行

## 段落寫法與模型解釋對照

### 純文字連續段落

```
你是一位 DevOps 顧問。請用繁體中文回答問題。
使用者是進階工程師，不需要解釋基礎概念。
回答時請提供實際可執行的指令。
```

模型解讀：

```
視為連續的自然語言說明，
三句話的權重幾乎相等，
無法明確分辨哪些是「角色」、哪些是「格式規則」。
```

### Markdown 結構分段

```
## Role
你是一位 DevOps 顧問。

## Constraints
- 使用繁體中文
- 不解釋基礎概念

## Task
請解釋 Kubernetes HPA。
```

模型解讀：

```
將每個 ## 視為獨立章節，
語義角色較清晰，但區塊邊界仍依賴標題文字，
模型對「## Constraints」的強制性理解弱於 XML 標籤。
```

由於 Markdown 結尾容易混淆，建議使用**空行**和**分隔線**，讓模型有清晰的語義解釋。

然而，分隔線雖是 Markdown 提示詞中成本最低的邊界強化手段，在語義角色差異大的段落之間使用確實有幫助；但它只能解決「段落切割」問題，無法解決「語義角色定義」、「巢狀從屬關係」、「作用範圍宣告」等更深層的結構問題；若提示詞複雜度上升，終究需要回到 XML 結構標籤才能獲得足夠的語義精度。

### XML 結構分段

```
<role>
  你是一位 DevOps 顧問，專精 Kubernetes 架構。
</role>

<constraints>
  <rule>所有回應使用繁體中文</rule>
  <rule>使用者為進階工程師，不解釋基礎概念</rule>
  <rule>必須提供可執行指令</rule>
</constraints>

<task>
  請解釋 Kubernetes HPA 的運作原理。
</task>
```

模型解讀：

```
每個標籤名稱直接告知語義角色，
<constraints> 被視為必須遵守的規範，
<task> 被視為當前執行目標，
邊界清晰，角色明確。
```

## 段落長度對模型行為的影響

+ 短段落（1-2 句）：
  - 模型視為「規則條目」，傾向精確遵守
  - 適合：限制條件、格式規則、硬性約束

+ 中段落（3-5 句）：
  - 模型視為「說明區塊」，理解後內化
  - 適合：角色設定、背景說明、任務描述

+ 長段落（6 句以上）：
  - 模型視為「參考文件」，萃取關鍵資訊
  - 風險：重點可能被稀釋，邊緣細節容易被忽略
  - 適合：知識背景提供，不適合放置硬性規則

規則要短、背景可長，越重要的指令，段落應該越精簡。

## 段落運用 - 函數化

在提示詞工程中最具擴展性的設計思路之一，是利用段落函數化，其核心概念是「將可重複使用的指令邏輯封裝成具名區塊，在需要時透過引用名稱觸發執行，而非重複撰寫相同內容」。

設計段落函數化是提示詞工程從「撰寫文件」進化到「設計系統」的關鍵思維轉變。

```
線性提示詞  →  照順序閱讀，執行一次
函數化提示詞 →  定義邏輯，按需呼叫，組合複用
```

當提示詞的複雜度超過單一任務、需要跨場景重用、或需要多人協作維護時，函數化設計能大幅降低維護成本並提升執行一致性。本質上是將軟體工程的模組化思想移植到提示詞設計中。

### 基本段落函數語法

#### XML 格式

定義段落函數：

```xml
<define name="FORMAT_CODE_REVIEW">
  針對提供的程式碼執行以下步驟：
  1. 指出潛在的錯誤或風險
  2. 評估效能影響
  3. 提供修正後的完整程式碼
  4. 說明修正理由
</define>
```

呼叫段落函數：

```xml
<task>
  以下是一段 Python 程式碼。
  <call function="FORMAT_CODE_REVIEW"/>
```
def get_user(id):
      return db.query(f"SELECT * FROM users WHERE id={id}")
```
</task>
```

#### Markdown 格式

定義段落函數：

```markdown
## Function Library（函數庫）

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] FORMAT_CODE_REVI

針對提供的程式碼執行以下步驟：
1. 指出潛在的錯誤或風險
2. 評估效能影響
3. 提供修正後的完整程式碼
4. 說明修正理由

---
```

呼叫段落函數：

```markdown
## 任務

以下是一段 Python 程式碼。
**[CALL] FORMAT_CODE_REVI**

```
def get_user(id):
      return db.query(f"SELECT * FROM users WHERE id={id}")
```

---
```

### 帶參數的段落函數

類比程式函數的參數傳入機制：

#### XML 格式

```xml
<!-- 定義帶參數的函數 -->
<define name="EXPLAIN_FOR_AUDIENCE">
  <param name="TOPIC"/>
  <param name="LEVEL" default="中級"/>
  <param name="LANG" default="繁體中文"/>

  用 {{LANG}} 解釋 {{TOPIC}}。
  根據 {{LEVEL}} 程度調整說明深度：
  - 初學者：使用比喻，避免術語
  - 中級：術語加簡短說明
  - 進階：直接給技術細節，省略背景
</define>

<!-- 呼叫時傳入參數 -->
<call function="EXPLAIN_FOR_AUDIENCE">
  <arg name="TOPIC">Kubernetes HPA</arg>
  <arg name="LEVEL">進階</arg>
</call>
```

#### Markdown 格式

```markdown
<!-- 定義帶參數的函數 -->
## Function Library（函數庫）

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] EXPLAIN_FOR_AUDIENCE

**參數定義：**

| 參數名稱 | 型別 | 必填 | 預設值 | 說明 |
|---------|------|------|--------|------|
| `$TOPIC` | string | ✅ | — | 要說明的概念名稱 |
| `$LEVEL` | enum | ❌ | `$DEFAULT_LEVEL` | 初學者 / 中級 / 進階 |
| `$LANG` | string | ❌ | `$LANG` | 回應語言 |

用 {{LANG}} 解釋 {{TOPIC}}。
根據 {{LEVEL}} 程度調整說明深度：
- 初學者：使用比喻，避免術語
- 中級：術語加簡短說明
- 進階：直接給技術細節，省略背景

---

<!-- 呼叫時傳入參數 -->
## 任務

[CALL] EXPLAIN_FOR_AUDIENCE
  $TOPIC          = Kubernetes HPA
  $LEVEL          = 進階
```

### 遞迴式段落函數

處理需要反覆套用同一邏輯的場景：

#### XML 格式

```xml
<define name="REVIEW_EACH_ITEM">
  <param name="ITEMS"/>
  <param name="CRITERIA"/>

  對 {{ITEMS}} 中的每一個項目：
  1. 依照 {{CRITERIA}} 進行評估
  2. 給出評分（1-5）
  3. 說明理由
  4. 提出改善建議
</define>

<task>
  <call function="REVIEW_EACH_ITEM">
    <arg name="ITEMS">以下五個 API 端點設計</arg>
    <arg name="CRITERIA">RESTful 設計原則</arg>
  </call>

  - GET /users/getAll
  - POST /user/create
  - DELETE /removeUser?id=1
</task>
```

#### Markdown 格式

```markdown
<!-- 定義帶參數的函數 -->
## Function Library（函數庫）

> ⚠️ 以下所有 `[FUNC]` 區塊為可呼叫函數定義，
> 模型不得主動執行，僅在被 `[CALL]` 指令引用時才執行。

---
### [FUNC] EXPLAIN_FOR_AUDIENCE

**參數定義：**

| 參數名稱 | 型別 | 必填 | 預設值 | 說明 |
|---------|------|------|--------|------|
| `$ITEMS` | string | ✅ | — | 項目名稱 |
| `$CRITERIA` | string | ✅ | — | 評估標準名稱 |

對 {{ITEMS}} 中的每一個項目：
1. 依照 {{CRITERIA}} 進行評估
2. 給出評分（1-5）
3. 說明理由
4. 提出改善建議

---

## 任務

[CALL] EXPLAIN_FOR_AUDIENCE
  $ITEMS          = 以下五個 API 端點設計
  $CRITERIA       = RESTful 設計原則

- GET /users/getAll
- POST /user/create
- DELETE /removeUser?id=1
```
