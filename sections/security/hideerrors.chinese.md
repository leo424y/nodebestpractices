# 在客戶端隱藏錯誤詳情

### 一段解釋

在生產環境中，應該避免在客戶端暴露應用程式的錯誤詳情，因為這會帶來暴露程式敏感資訊的風險，比如：伺服器檔案路徑、使用的第三方模組以及其他可能被攻擊者利用的內部工作流程資訊等。Express擁有內建的錯誤處理機制，以此來處理好在程式中可能會出現的任何錯誤。預設的錯誤處理中介軟體方法被新增到了中介軟體堆棧的最後面。如果你傳了一個錯誤給`next()`，又沒有通過自定義的錯誤處理機制來處理它，這個錯誤會被Express內建的錯誤處理機制處理；這個錯誤會被寫到客戶端的堆棧跟蹤資訊裡。當`NODE_ENV`被設定為`development`的時候，才會把詳細的錯誤資訊寫到客戶端，但是當`NODE_ENV`被設定為`production`的時候，不會把詳細的堆棧跟蹤資訊寫入到客戶端，只會返回HTTP的響應碼。

### 程式碼示例：Express錯誤處理

``` javascript
// 生產環境錯誤處理
// 不把堆棧跟蹤資訊展示給使用者看
app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
        message: err.message,
        error: {}
    });
});
```

### 其他資源

🔗 [Express.js 錯誤處理相關文件](https://expressjs.com/en/guide/error-handling.html)