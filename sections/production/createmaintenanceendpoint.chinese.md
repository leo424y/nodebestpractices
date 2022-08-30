# 建立維護端點

<br/><br/>


### 一段解釋

維護端點是一個簡單的安全的HTTP API, 它是應用程式程式碼的一部分, 它的用途是讓ops/生產團隊用來監視和公開維護功能。例如, 它可以返回程序的head dump (記憶體快照), 報告是否存在記憶體洩漏, 甚至允許直接執行 REPL 命令。在常規的 devops 工具 (監視產品、日誌等) 無法收集特定類型的資訊或您選擇不購買/安裝此類工具時, 需要使用此端點。黃金法則是使用專業的和外部的工具來監控和維護生產環境, 它們通常更加健壯和準確的。這就意味著, 一般的工具可能無法提取特定於node或應用程式的資訊 – 例如, 如果您希望在 GC 完成一個週期時生成記憶體快照 – 很少有 npm 庫會很樂意為您執行這個, 但流行的監控工具很可能會錯過這個功能。

<br/><br/>


### 程式碼示例: 使用程式碼生產head dump

```javascript
var heapdump = require('heapdump');
 
router.get('/ops/headump', (req, res, next) => {
    logger.info(`About to generate headump`);
    heapdump.writeSnapshot(function (err, filename) {
        console.log('headump file is ready to be sent to the caller', filename);
        fs.readFile(filename, "utf-8", function (err, data) {
            res.end(data);
        });
    });
});
```

<br/><br/>

### 推薦資源

[Getting your Node.js app production ready (Slides)](http://naugtur.pl/pres3/node2prod)

▶ [Getting your Node.js app production ready (Video)](https://www.youtube.com/watch?v=lUsNne-_VIk)

![Getting your Node.js app production ready](../../assets/images/createmaintenanceendpoint1.png "Getting your Node.js app production ready")
