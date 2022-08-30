# 使用成熟的logger提高錯誤可見性

### 一段解釋

我們都特別喜歡（loovve）console.log，但顯而易見地，對於嚴肅的項目, 有信譽和持久的Logger是必需的，比如[Winston][winston] (非常流行) or [Pino][pino](專注於效能的新庫)。一套實踐和工具將有助於更快速地解釋錯誤 – (1)使用不同的級別（debug, info, error）頻繁地log；(2)在記錄日誌時, 以 JSON 物件的方式提供上下文資訊, 請參見下面的示例；(3)使用日誌查詢API(在大多數logger中內建)或日誌檢視程式軟體監視和篩選日誌；(4)使用操作智慧工具(如 Splunk)為操作團隊公開和管理日誌語句。

[winston]: https://www.npmjs.com/package/winston
[bunyan]: https://www.npmjs.com/package/bunyan
[pino]: https://www.npmjs.com/package/pino

### 程式碼示例 – 使用Winston Logger

```javascript
//您的集中式logger物件
var logger = new winston.Logger({
  level: 'info',
  transports: [
    new (winston.transports.Console)(),
    new (winston.transports.File)({ filename: 'somefile.log' })
  ]
});

//在某個地方使用logger的自定義程式碼
logger.log('info', 'Test Log Message with some parameter %s', 'some parameter', { anything: 'This is metadata' });

```

### 程式碼示例 – 查詢日誌資料夾 (搜尋條目)

```javascript
var options = {
    from: new Date - 24 * 60 * 60 * 1000,
    until: new Date,
    limit: 10,
    start: 0,
    order: 'desc',
    fields: ['message']
  };


  // 查詢在今天和昨天之間記錄的項目
  winston.query(options, function (err, results) {
    //對於結果的回撥處理
  });

```

### 部落格引用: "Logger要求"
 摘自部落格 Strong Loop

 > 讓我們確定一些要求 (對於logger):
1. 為每條日誌新增時間戳。這條很好自我解釋-你應該能夠告知每個日誌條目發生在什麼時候。
2. 日誌格式應易於被人類和機器理解。
3. 允許多個可配置的目標流。例如, 您可能正在將trace log寫入到一個檔案中, 但遇到錯誤時, 請寫入同一檔案, 然後寫入到錯誤日誌檔案，並同時傳送電子郵件…
