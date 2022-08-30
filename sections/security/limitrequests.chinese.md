# 通過平衡器或者中介軟體限制併發請求

### 一段解釋

你的Node.js應用程式應該實現限流，以此來保護它不會因為同一時間太多請求而崩潰。限流就是一個任務，需要一個專門為此設計的服務來輔助才能達到最佳的執行效果，比如nginx，但是通過應用程式中介軟體也可以實現，比如[express-rate-limiter](https://www.npmjs.com/package/express-rate-limit)。

### 程式碼示例：Express對某些路由使用限流中介軟體

使用[express-rate-limiter](https://www.npmjs.com/package/express-rate-limit)模組

``` javascript
const RateLimit = require('express-rate-limit');
// 如果請求需要經過代理，確保客戶端IP能夠傳給req.ip這一點非常重要
app.enable('trust proxy');

const apiLimiter = new RateLimit({
  windowMs: 15*60*1000, // 15分鐘
  max: 100,
});

// 僅應用於以/user/開頭的請求
app.use('/user/', apiLimiter);
```

### 其他博主的看法

摘自 [NGINX 部落格](https://www.nginx.com/blog/rate-limiting-nginx/):

> 限流可以是出於安全考慮，比如用於降低暴力破解密碼攻擊的風險。可以通過限制輸入請求速率在某個基於真實使用者的特定值來防止DDOS攻擊，這對分析（通過日誌）特定的URLs也有一定的幫助。但通常情況下，限流是用來保護上游應用程式不會因為同一時間太多請求而崩潰。