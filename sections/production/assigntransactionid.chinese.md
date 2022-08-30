# 在每一個log語句指定‘TransactionId’

<br/><br/>


### 一段解釋

一個典型的日誌是來自所有元件和請求的條目的倉庫。當檢測到一些可疑行或錯誤時，為了與其他屬於同一特定流程的行（如使用者“約翰”試圖購買某物）相匹配，就會變得難以應付。特別在微服務環境下，當一個請求/交易可能跨越多個計算機，這變得更加重要和具有挑戰性。解決這個問題，可以通過指定一個唯一的事務標識符給從相同的請求過來的所有條目，這樣當檢測到一行，可以複製這個id，並搜尋包含這個transaction id的每一行。但是，在node中實現這個不是那麼直截了當的，這是由於它的單執行緒被用來服務所有的請求 – 考慮使用一個庫，它可以在請求層對資料進行分組 – 在下一張幻燈片檢視示例程式碼。當呼叫其它微服務，使用HTTP頭“x-transaction-id”傳遞transaction id去保持相同的上下文。

<br/><br/>


### 程式碼示例: 典型的nginx配置

```javascript
//當接收到一個新的要求，開始一個新的隔離的上下文和設定一個事務transaction id。下面的例子是使用NPM庫continuation-local-storage去隔離請求

const { createNamespace } = require('continuation-local-storage');
var session = createNamespace('my session');

router.get('/:id', (req, res, next) => {
    session.set('transactionId', 'some unique GUID');
    someService.getById(req.params.id);
    logger.info('Starting now to get something by Id');
});

//現在, 任何其他服務或元件都可以訪問上下文、每個請求、資料
class someService {
    getById(id) {
        logger.info(“Starting now to get something by Id”);
        //其它邏輯
    }
}

//Logger現在可以將事務 id 追加到每個條目, 以便同一請求中的項將具有相同的值
class logger{
    info (message)
    {console.log(`${message} ${session.get('transactionId')}`);}
}
```
