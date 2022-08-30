# Express的 '應用' 和 '服務' 分離

<br/><br/>


### 一段解釋

最新的Express生成器有一個值得保留的偉大實踐--API聲明與網路相關配置（埠、協議等）是分開的。這樣就可以在不執行網路呼叫的情況下對API進行線上測試，它所帶來的好處是：快速執行測試操作和獲取程式碼覆蓋率。它還允許在靈活多樣的網路條件下部署相同的API。額外好處：更好的關注點分離和更清晰的程式碼結構。

<br/><br/>

### 程式碼示例：API聲明應該在 app.js 檔案裡面

```javascript
var app = express();
app.use(bodyParser.json());
app.use("/api/events", events.API);
app.use("/api/forms", forms);

```

<br/><br/>

### 程式碼示例: 伺服器網路聲明，應該在 /bin/www 檔案裡面

```javascript
var app = require('../app');
var http = require('http');

/**
 * Get port from environment and store in Express.
 */

var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);

/**
 * Create HTTP server.
 */

var server = http.createServer(app);

```


### 示例程式碼: 使用超快的流行的測試包線上測試你的程式碼

```javascript
const app = express();

app.get('/user', function(req, res) {
  res.status(200).json({ name: 'tobi' });
});

request(app)
  .get('/user')
  .expect('Content-Type', /json/)
  .expect('Content-Length', '15')
  .expect(200)
  .end(function(err, res) {
    if (err) throw err;
  });
````
