# 特殊情況產生時，優雅地退出服務


### 一段解釋

在您的程式碼的某個地方，當一個錯誤拋出的時候，錯誤處理物件負責決定如何進行時 – 如果錯誤是可信的（即操作型錯誤，在最佳實踐#3瞭解進一步的解釋），寫入日誌檔案可能是足夠的。如果錯誤不熟悉，事情就變得棘手了 – 這意味著某些元件可能處於故障狀態，所有將來的請求都可能失敗。例如，假設一個單例（singleton）的，有狀態的令牌發行者服務拋出異常並失去它的狀態 — 從現在起，它可能會出現意外行為並導致所有請求失敗。在這種情況下，殺程序，使用“重啟”的工具（像Forever，PM2，等等）重新開始。



### 程式碼例項: 決定是否退出

```javascript
//收到未捕獲的異常時，決定是否要崩潰
//如果開發人員標記已知的操作型錯誤使用：error.isOperational=true， 檢視最佳實踐 #3
process.on('uncaughtException', function(error) {
 errorManagement.handler.handleError(error);
 if(!errorManagement.handler.isTrustedError(error))
 process.exit(1)
});
 
 
//封裝錯誤處理相關邏輯在集中的錯誤處理中
function errorHandler(){
 this.handleError = function (error) {
 return logger.logError(err).then(sendMailToAdminIfCritical).then(saveInOpsQueueIfCritical).then(determineIfOperationalError);
 }
 
 this.isTrustedError = function(error)
 {
 return error.isOperational;
 }

```


### 部落格引用: "最好的方式是立即崩潰"
摘自 部落格：Joyent
 
 > …從程式型錯誤中恢復過來的最好方法是立即崩潰。你應該使用一個重啟助手來執行您的程式，它會在崩潰的情況下自動啟動程式。當使用重啟助手，崩潰是面對臨時性的程式型錯誤時，恢復可靠的服務的最快的方法…


### 部落格引用: "錯誤處理有三種流派"
摘自 部落格：JS Recipes
 
 > …錯誤處理主要有三種流派:
1. 讓應用崩潰，並重啟。
2. 處理所有的錯誤，從不崩潰。
3. 介於兩者之間。


### 部落格引用: "不伴隨著建立一些易碎的狀態，是沒有保險的方式退出"
摘自 Node.JS 官方文件
 
 > …就throw工作在JavaScript的本質而言，幾乎沒有任何方法可以安全地“在您丟下的地方撿起”，而不會洩漏引用，或者建立其他類型的未定義的易碎性狀態。對拋出的錯誤作出響應的最安全的方法是關閉程序。當然，在一個普通的Web伺服器中，可能有很多連線開啟了，因為其他人觸發了一個錯誤，所以突然關閉這些連線是不合理的。更好的方法是將錯誤響應傳送給觸發錯誤的請求，同時讓其他人在正常時間內完成，並停止偵聽該工作者的新請求。