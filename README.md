# tutorial-llm-prompt

本專案針對現代大語言模型服務的提示詞腳本、語法進行彙整。

本專案執行與測試方式根據選擇的大語言模型服務會有不同，相關事前調延請參考如下專案：

+ [research-ai-chatbot](https://github.com/eastmoon/research-ai-chatbot)，調查雲端大語言服務費用與運用資訊。
+ [infra-ghc-cli](https://github.com/eastmoon/infra-ghc-cli)，Github Copilot CLI 開發運維配置與使用方式。
+ [infra-claude-cli](https://github.com/eastmoon/infra-claude-cli)，Claude CLI 開發運維配置與使用方式。
+ [infra-gemini-cli](https://github.com/eastmoon/infra-gemini-cli)，Google Gemini CLI 開發運維配置與使用方式。
+ [infra-pi-mono](https://github.com/eastmoon/infra-pi-mono)，第三方 CLI 庫 Pi 開發運維配置與使用方式。
+ [research-spec-driven-development](https://github.com/eastmoon/research-spec-driven-development)，調查規格驅動開發 ( Spec-Driven Development ) 概念與語意編程 ( Semantic Coding ) 方法論。

## 使用方式

完成大語言模型服務的應用程式或命列介面環境，並基於以下流程確保大語言模型可使用提示詞指令。

1. 若為 Claude 大語言模型，將目錄 ```./prompt``` 內容複製至 ```.claude/commands``` 目錄中

原則上，不同的大語言模型會使用位於專案目錄下 ```.[大語言模型服務名稱]/commands``` 的目錄存放可供使用的目錄；但也有例外，例如 Pi 使用為 ```.pi/prompt``` 目錄，但其服務啟動會自動將 commands 轉入 prompt 中。

2. 啟動 Claude 應用程式，並選擇並信任專案目錄，此時在其介面執行 ```/[命令腳本]```，如 ```/syntax.variable``` 即可執行 [syntax.variable.md](./prompt/syntax.variable.md) 檔案。

原則上，不同大語言模型會有相應的應用服務介面、命令列介面，如果沒有也可運用第三方命令列介面；可若使用為網頁服務，也可將此命令作為檔案上傳後執行，或根據大語言模型提供的服務來建立代理人，例如 Google Gemini 的 Gem。

## 提示詞範本

### 段落

如何在提示詞文件中運用段落，讓大語言模型概觀理解文章目的。

+ [說明](./docs/syntax.paragraph.md)
+ 範本
  - [分段](./prompt/syntax.paragraph.md)
  - [函數](./prompt/syntax.function.md)
  - [參數化函數](./prompt/syntax.function.param.md)
  - [遞迴函數](./prompt/syntax.function.recursive.md)

### 註解

如何在提示詞文件中撰寫註解，讓大語言模型忽略此內容。

+ [說明](./docs/syntax.commnet.md)
+ [範本](./prompt/syntax.commnet.md)

### 版型

如何在提示詞文件中撰寫符合期望的輸出格式。

+ [說明](./docs/syntax.format.md)
+ [範本](./prompt/syntax.format.md)

### 變數

如何在提示詞文件中撰寫變數，用來保存特定數據、動態內容。

+ [說明](./docs/syntax.variable.md)
+ [範本](./prompt/syntax.variable.md)

### 判斷

如何在提示詞文件中撰寫判斷句，用來處理邏輯結構。

+ [說明](./docs/syntax.determine.md)
+ 範本
  - [假如句型](./prompt/syntax.determine.md)
