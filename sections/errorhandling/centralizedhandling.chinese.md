# 集中處理錯誤，通過但不是在中介軟體裡處理錯誤


### 一段解釋

如果沒有一個專用的錯誤處理物件，那麼由於操作不當，在雷達下重要錯誤被隱藏的可能性就會更大。錯誤處理物件負責使錯誤可見，例如通過寫入一個格式化良好的logger，通過電子郵件將事件傳送到某個監控產品或管理員。一個典型的錯誤處理流程可能是：一些模組拋出一個錯誤 -> API路由器捕獲錯誤 -> 它傳播錯誤給負責捕獲錯誤的中介軟體（如Express，KOA）-> 集中式錯誤處理程式被呼叫 -> 中介軟體正在被告之這個錯誤是否是一個不可信的錯誤（不是操作型錯誤），這樣可以優雅的重新啟動應用程式。注意，在Express中介軟體中處理錯誤是一種常見但又錯誤的做法，這樣做不會覆蓋在非Web介面中拋出的錯誤。



### 程式碼示例 – 一個典型錯誤流

```javascript
//DAL層, 在這裡我們不處理錯誤
DB.addDocument(newCustomer, (error, result) => {
    if (error)
        throw new Error("Great error explanation comes here", other useful parameters)
});
 
//API路由程式碼, 我們同時捕獲非同步和同步錯誤，並轉到中介軟體
try {
    customerService.addNew(req.body).then(function (result) {
        res.status(200).json(result);
    }).catch((error) => {
        next(error)
    });
}
catch (error) {
    next(error);
}
 
//錯誤處理中介軟體，我們委託集中式錯誤處理程式處理錯誤
app.use(function (err, req, res, next) {
    errorHandler.handleError(err).then((isOperationalError) => {
        if (!isOperationalError)
            next(err);
    });
});

```

### 程式碼示例 – 在一個專門的物件裡面處理錯誤

```javascript
module.exports.handler = new errorHandler();
 
function errorHandler() {
    this.handleError = function (err) {
        return logger.logError(err).then(sendMailToAdminIfCritical).then(saveInOpsQueueIfCritical).then(determineIfOperationalError);
    }
}
```

### 程式碼示例 – 反模式：在中介軟體內處理錯誤

```javascript
//中介軟體直接處理錯誤，那誰將處理Cron任務和測試錯誤呢？
app.use(function (err, req, res, next) {
    logger.logError(err);
    if(err.severity == errors.high)
        mailer.sendMail(configuration.adminMail, "Critical error occured", err);
    if(!err.isOperational)
        next(err);
});

```

### 部落格引用: "有時較低的級別不能做任何有用的事情, 除非將錯誤傳播給他們的呼叫者"
 摘自部落格 Joyent, 對應關鍵字 “Node.JS error handling” 排名第一
 
 > …您可能會在stack的多個級別上處理相同的錯誤。這發生在當較低階別不能執行任何有用的操作，除了將錯誤傳播給它們的呼叫方, 從而將錯誤傳播到其呼叫方, 等等。通常, 只有top-level呼叫方知道適當的響應是什麼, 無論是重試操作、向使用者報告錯誤還是其他事情。但這並不意味著您應該嘗試將所有錯誤報告給單個top-level回撥, 因為該回撥本身無法知道錯誤發生在什麼上下文中…

 
### 部落格引用: "單獨處理每個錯誤將導致大量的重複"
 摘自部落格 JS Recipes, 對應關鍵字 “Node.JS error handling” 排名17
 
 > ……僅僅在Hackathon啟動api.js控制器中, 有超過79處重複的錯誤物件。單獨處理每個錯誤將導致大量的程式碼重複。您可以做的下一個最好的事情是將所有錯誤處理邏輯委派給一個express中介軟體…


### 部落格引用: "HTTP錯誤不會在資料庫程式碼中出現"
 摘自部落格 Daily JS, 對應關鍵字 “Node.JS error handling” 排名14
 
 > ……您應該在error物件中設定有用的屬性, 但使用此類屬性時應保持一致。而且, 不要越過流: HTTP錯誤不會在資料庫程式碼中出現。或者對於瀏覽器開發人員來說, Ajax 錯誤在與伺服器互動的程式碼中有一席之地, 而不是處理Mustache模板的程式碼…

