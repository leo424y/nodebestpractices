# 處理子程序時要謹慎

### 一段解釋

儘管子程序非常棒, 但使用它們應該謹慎。如果無法避免傳遞使用者輸入，就必須經過脫敏處理。
未經脫敏處理的輸入執行系統級邏輯的危險是無限的, 從遠端程式碼執行到暴露敏感的系統資料, 甚至資料丟失。準備工作的檢查清單可能是這樣的

- 避免在每一種情況下的使用者輸入, 否則驗證和脫敏處理
- 使用user/group標識限制父程序和子程序的許可權
- 在隔離環境中執行程序, 以防止在其他準備工作失敗時產生不必要的副作用

### 程式碼示例: 未脫敏處理子程序的危害

```javascript
const { exec } = require('child_process');

...

// 例如, 以一個指令碼為例, 它採用兩個參數, 其中一個參數是未經脫敏處理的使用者輸入
exec('"/path/to/test file/someScript.sh" --someOption ' + input);

// -> 想象一下, 如果使用者只是輸入'&& rm -rf --no-preserve-root /'類似的東西, 會發生什麼
// 你會得到一個不想要的結果
```

### 額外資源

摘自Node.js child process [documentation](https://nodejs.org/dist/latest-v8.x/docs/api/child_process.html#child_process_child_process_exec_command_options_callback):

> 切勿將未經脫敏處理的使用者輸入傳遞給此函數。任何包含shell元字元（metacharacters）的輸入都可用於觸發任意命令的執行。
