# 區分操作型錯誤和程式型錯誤

### 一段解釋

區分以下兩種錯誤類型將最大限度地減少應用程式停機時間並幫助避免出現荒唐的錯誤: 操作型錯誤指的是您瞭解發生了什麼情況及其影響的情形 – 例如, 由於連線問題而導致對某些 HTTP 服務的查詢失敗問題。另一方面, 程式型錯誤指的是您不知道原因, 有時是錯誤不知道來自何處的情況 – 可能是一些程式碼試圖讀取未定義的值或 DB 連線池記憶體洩漏。操作型錯誤相對容易處理 – 通常記錄錯誤就足夠了。當程式型錯誤出現，事情變得難以應付, 應用程式可能處於不一致狀態, 你可以做的，沒有什麼比優雅的重新啟動更好了。



### 程式碼示例 – 將錯誤標記為可操作 (受信任)

```javascript
//將錯誤標記為可操作 
var myError = new Error("How can I add new product when no value provided?");
myError.isOperational = true;
 
//或者, 如果您使用的是一些集中式錯誤工廠 (請參見項目符號中的示例"僅使用內建錯誤物件")
function appError(commonType, description, isOperational) {
    Error.call(this);
    Error.captureStackTrace(this);
    this.commonType = commonType;
    this.description = description;
    this.isOperational = isOperational;
};
 
throw new appError(errorManagement.commonErrors.InvalidInput, "Describe here what happened", true);

```

### 部落格引用: "程式型錯誤是程式中的 bug"
 摘自部落格 Joyent, 對於關鍵字“Node.JS error handling”排名第一
 
 > …從程式型錯誤中恢復的最好方法是立即崩潰。您應該使用restarter執行程式, 以便在發生崩潰時自動重新啟動程式。在一個使用了restarter的地方, 在面對一個瞬態程式型錯誤, 崩潰是最快的方式來恢復可靠的服務…

 ### 部落格引用: "不伴隨著建立一些未定義的脆性狀態，沒有安全的方式可以離開"
 摘自Node.JS官方文件
 
 > …從 JavaScript throw 的工作原理上講, 幾乎沒有任何方法可以安全地“在你跌倒的地方重新爬起來”而不引發洩漏且不建立一些其他形式的未定義的脆性狀態。響應（未定義的）拋出錯誤的最安全方法是關閉程序。當然, 通常 web 伺服器可能會有許多連線正在通訊, 由於某個人觸發了錯誤而突然關閉那些連線是不合理的。更好的方法是讓該工作程序向觸發錯誤的請求傳送錯誤響應, 同時保持其它請求正常進行直至完成, 並停止偵聽的新的請求。（譯者注：為優雅重啟做準備）

 ### 部落格引用: "否則，您置您應用的狀態於風險之中"
  摘自部落格 debugable.com, 對於關鍵字“Node.JS uncaught exception”排名第3
 
 > …所以, 除非你真的知道你在做什麼, 否則你應該在收到一個"uncaughtException"異常事件之後, 對你的服務進行一次優雅的重新啟動。否則, 您應用的狀態, 或和第三方庫的狀態變得不一致, 都被置於風險之中，導致各種荒唐的錯誤…

 ### 部落格引用: "對於錯誤處理，有三種學院派想法"
 摘自部落格: JS Recipes
 
 > …對於錯誤處理，主要有三種學院派想法:
1. 讓應用崩潰並重啟.
2. 處理所有可能的錯誤，從不崩潰.
3. 兩者之間的折中方案
