#  僅使用內建的錯誤物件


### 一段解釋

JS天生的寬容性及其多變的程式碼流選項（例如 EventEmitter, Callbacks, Promises等等）使得開發者有太多引發錯誤的方式 – 有些人使用字元串，有些人使用自定義的類型。使用Node.js的內建錯誤物件有助於在你的程式碼和第三方庫之間保持一致性，它還保留了重要資訊，比如StackTrace。當引發異常時，給異常附加上下文屬性（如錯誤名稱和相關的HTTP錯誤程式碼）通常是一個好的習慣。要實現這種一致性和實踐，請考慮使用附加屬性擴充套件錯誤物件，見下面的程式碼示例。


### 程式碼示例 – 正確做法

```javascript
//從典型函數拋出錯誤, 無論是同步還是非同步
if(!productToAdd)
    throw new Error("How can I add new product when no value provided?");

//從EventEmitter拋出錯誤
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));

//從promise拋出錯誤
 return new promise(function (resolve, reject) {
    Return DAL.getProduct(productToAdd.id).then((existingProduct) => {
        if(existingProduct != null)
            reject(new Error("Why fooling us and trying to add an existing product?"));

```

### 程式碼示例 – 反例

```javascript
//拋出字元串錯誤缺少任何stack trace資訊和其他重要屬性
if(!productToAdd)
    throw ("How can I add new product when no value provided?");

```

### 程式碼示例 – 更好做法

```javascript
//從node錯誤派生的集中錯誤物件
function appError(name, httpCode, description, isOperational) {
    Error.call(this);
    Error.captureStackTrace(this);
    this.name = name;
    //...在這賦值其它屬性
};

appError.prototype = Object.create(Error.prototype);
appError.prototype.function Object() { [native code] } = appError;

module.exports.appError = appError;

//客戶端拋出一個錯誤
if(user == null)
  throw new appError(commonErrors.resourceNotFound, commonHTTPErrors.notFound, "further explanation", true)
```

### 部落格引用：“I don’t see the value in having lots of different types”

摘自部落格Ben Nadel, 對於關鍵字“Node.JS錯誤物件”，排名第五

> … 就我個人而言，我沒看到弄很多不同類型的錯誤物件的價值 – JavaScript作為一種語言，似乎不適合基於建構函式的錯誤捕獲。因此，區分物件屬性似乎比區分建構函式類型容易得多…

### 部落格引用: "字元串不是錯誤"

摘自部落格 devthought.com, 對於關鍵字 “Node.JS error object” 排名第6

> … 傳遞字元串而不是錯誤會導致模組間協作性降低。它打破了和API的約定，可能在執行`instanceof`這樣的錯誤檢查，或想了解更多關於錯誤的資訊。正如我們將看到的，錯誤物件在現代JavaScript引擎中擁有非常有趣的屬性，同時保留傳遞給建構函式的訊息…

### 部落格引用: "從Error物件繼承不會增加太多的價值"

摘自部落格 machadogj

> … 我對Error類的一個問題是不太容易擴充套件。當然, 您可以繼承該類並建立自己的Error類, 如HttpError、DbError等。然而, 這需要時間, 並且不會增加太多的價值, 除非你是在做一些關於類型的事情。有時, 您只想新增一條訊息, 並保留內部錯誤, 有時您可能希望使用參數擴充套件該錯誤, 等等…

### 部落格引用: "Node.js引發的所有JavaScript和系統錯誤都繼承自Error"

摘自 Node.JS 官方文件

> … Node.js引發的所有JavaScript和系統錯誤繼承自，或是JavaScript標準錯誤類的例項, 這保證至少提供了該類的可用屬性。一個通用的JavaScript錯誤物件, 它不表示錯誤為什麼發生的任何特定環境。錯誤物件捕獲一個"stack trace", 詳細說明了錯誤被例項化時在程式碼中的點, 並可能提供錯誤的文字描述。由Node.js生成的所有錯誤, 包括所有的系統和JavaScript錯誤, 都將是Error類的例項, 或繼承自Error類 …
