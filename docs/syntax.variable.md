# 變數

大語言模型本身無持久記憶，「變數記憶」的本質是：**在 Context Window 內明確定義 → 要求模型引用 → 必要時讓模型回寫狀態**。

## 一、基本變數宣告語法

最簡單的方式是在提示詞開頭統一定義變數區塊：

```xml
<variables>
  USER_NAME     = Jacky
  USER_ROLE     = DevOps Engineer
  LANGUAGE      = 繁體中文
  TONE          = 專業且簡潔
  PROJECT       = SNMP 監控平台
</variables>

<!-- 以下所有回應請引用上方變數，以 {{變數名稱}} 方式標記使用位置 -->

請以 {{TONE}} 的語氣，用 {{LANGUAGE}} 回答 {{USER_NAME}} 關於 {{PROJECT}} 的問題。
```

需注意在提示詞工程中，對於宣告可以使用 XML 或 Markdown 格式，這兩者有其差異性：

對於 XML 風格，模型傾向將內容視為「資料」，模型的內部解讀接近「這是一組結構化定義，我需要在後續指令中引用這些值。」
```
<variables>
  <var name="USER">Jacky</var>
  <var name="ROLE">DevOps Engineer</var>
</variables>
```

對於 Markdown 風格，模型傾向將內容視為「文件段落」，模型的內部解讀接近「這是文件的一個章節，標題叫做 Declare Variable，內容是一些說明文字。」
```
## Declare Variable

USER = Jacky
ROLE = DevOps Engineer
```

嚴格來說 Markdown 雖然適合人類閱讀，但本身存在解析模糊邊界，為避免大語言模型解讀錯誤，應注意以下使用時機：

使用 XML <variable> 的時機：
✅ 需要精確的變數作用域控制
✅ 提示詞結構複雜，有多個區塊互相引用
✅ 變數需要巢狀或分層（global / task / session）
✅ 提示詞會由程式動態生成或解析
✅ 要求模型將變數視為「設定」而非「內容」

使用 Markdown ## 的時機：
✅ 提示詞簡單，只有少量變數
✅ 受眾是人類閱讀為主，模型執行為輔
✅ 快速原型，不追求嚴謹邊界
✅ 渲染後要給人看的文件型提示詞

## Mustache 風格（`{{variable}}`）

業界最常見的提示詞變數慣例，可讀性高：

```
你是一位 {{ROLE}} 專家。

使用者資訊：
- 姓名：{{USER_NAME}}
- 技術等級：{{SKILL_LEVEL}}
- 偏好語言：{{LANG}}

任務：請針對 {{USER_NAME}} 的 {{SKILL_LEVEL}} 程度，
      用 {{LANG}} 解釋 {{TOPIC}}。
```

填入後：

```
你是一位 DevOps 專家。

使用者資訊：
- 姓名：Jacky
- 技術等級：進階
- 偏好語言：繁體中文

任務：請針對 Jacky 的進階程度，
      用繁體中文解釋 Kubernetes HPA。
```

此外，不同的開發人員會有不同的變數宣告習慣，例如以 Unix Shell / POSIX 為基礎的 ```$VARIABLE``` 風格，但此種寫法容易造成混淆的情況，例如 $100 會被視為金額而非變數。

若用嚴謹度來看待，則各自寫法會如下：

```
# 基本型（邊界模糊）
$USERNAME

# 改良型（邊界清晰）
${USERNAME}

# 與 Mustache 對比
{{username}}
```
> 在提示詞工程中，若堅持 Shell 風格，建議統一使用 ${VAR} 而非裸 $VAR。

## 讓模型「記憶並回寫」變數狀態

適合多輪對話或狀態追蹤場景：

```xml
<system>
你是一位技術顧問。每次回應結束時，
必須更新並輸出 <session_state> 區塊，
記錄本輪對話萃取出的關鍵變數。
</system>

<session_state>
  DISCUSSED_TOPICS = []
  USER_DECISIONS   = []
  PENDING_QUESTIONS = []
  CURRENT_FOCUS    = null
</session_state>

請開始討論我的監控架構設計。
```

模型回應後會附上：

```xml
<session_state>
  DISCUSSED_TOPICS  = ["SNMP 架構", "Polling vs Trap"]
  USER_DECISIONS    = ["採用 Pull 模式"]
  PENDING_QUESTIONS = ["告警閾值如何設定？"]
  CURRENT_FOCUS     = "資料收集層設計"
</session_state>
```

下一輪將此區塊貼回，模型即可「記住」上下文。


## 變數作用域分層

```xml
<!-- ── 全域變數（整個對話有效）── -->
<global_vars>
  APP_NAME     = MonitorX
  STACK        = Python + Prometheus + Grafana
  OUTPUT_LANG  = 繁體中文
</global_vars>

<!-- ── 任務變數（本次任務有效）── -->
<task_vars>
  TASK         = 撰寫 SNMP Exporter 設定說明
  AUDIENCE     = 剛入門的 SRE
  MAX_LENGTH   = 500字
</task_vars>

<!-- ── 執行期變數（由使用者輸入填入）── -->
請說明如何為 {{TARGET_DEVICE}} 設定 {{OID_GROUP}}。
```

## 變數型別設計

```xml
<variables>
  <!-- 字串型 -->
  USER_NAME    = "Jacky"

  <!-- 列舉型（限定合法值） -->
  OUTPUT_FORMAT = [markdown | json | plain]  <!-- 當前值：markdown -->

  <!-- 布林型 -->
  SHOW_EXAMPLE  = true
  VERBOSE_MODE  = false

  <!-- 數值型 -->
  MAX_ITEMS     = 5

  <!-- 列表型 -->
  FOCUS_AREAS   = ["效能", "安全性", "可觀測性"]
</variables>
```
