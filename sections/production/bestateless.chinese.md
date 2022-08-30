# 儲存無狀態，幾乎每天停掉伺服器

<br/><br/>


### 一段解釋

你曾經有沒有遇到一類嚴重的線上問題，比如一個伺服器丟失了一些配置或資料？這可能是由於對於不屬於部署部分的本地資源的一些不必要的依賴。許多成功的產品對待伺服器就像鳳凰鳥–它週期性地死亡和重生不帶來任何損傷。換句話說，伺服器只是一個硬體，執行你的程式碼一段時間後，可以更換。
這個方法:
- 允許動態新增和刪除伺服器，無任何負面影響； 
- 簡化了維護，因為它使我們不必費精力對每個伺服器狀態進行評估。

<br/><br/>


### 程式碼示例: 反模式

```javascript
//典型錯誤1: 儲存上傳檔案在本地伺服器上
var multer  = require('multer') // 處理multipart上傳的express中介軟體
var upload = multer({ dest: 'uploads/' })

app.post('/photos/upload', upload.array('photos', 12), function (req, res, next) {})

//典型錯誤2: 在本地檔案或者記憶體中，儲存授權會話（密碼）
var FileStore = require('session-file-store')(session);
app.use(session({
    store: new FileStore(options),
    secret: 'keyboard cat'
}));

//典型錯誤3: 在全局物件中儲存資訊
Global.someCacheLike.result = {somedata}
```

<br/><br/>

### 其他部落格作者說什麼
摘自部落格 [Martin Fowler](https://martinfowler.com/bliki/PhoenixServer.html):
> ...某天，我有了啟動一個對操作的認證服務的幻想。認證評估將由我和一位同事出現在企業資料中心，並通過一個棒球棒，一個電鋸和一把水槍設定關鍵生產伺服器。評估將基於操作團隊需要多長時間才能重新執行所有應用程式。這可能是一個愚蠢的幻想，但有一個真正的智慧在這裡。而你應該放棄棒球棒，去定期的幾乎燒燬你的伺服器，這是一個好的做法。伺服器應該像鳳凰，經常從灰燼中升起...
 
<br/><br/>
