# 使用https加密客戶端-伺服器連線

<br/><br/>


### 一段解釋

使用服務，比如[Let'sEncrypt](https://letsencrypt.org/)證書頒發機構提供 __free__ ssl/tls證書，您可以輕鬆地獲得證書, 以確保您的應用程式安全。Node.js框架，比如[Express](http://expressjs.com/)(基於核心`https`模組) 輕鬆支援基於ssl/tls的服務，此外, 配置可以通過幾行額外的程式碼完成。

您還可以在指向應用程式的反向代理上配置ssl/tls，例如使用[nginx](http://nginx.org/en/docs/http/configuring_https_servers.html)或者HAProxy.

<br/><br/>

### 程式碼示例 – 使用express框架啟用SSL/TLS

```javascript
const express = require('express');
const https = require('https');
const app = express();
const options = {
    // 路徑應根據您的設定進行相應的更改
    cert: fs.readFileSync('./sslcert/fullchain.pem'),
    key: fs.readFileSync('./sslcert/privkey.pem')
};
https.createServer(options, app).listen(443);
```

<br/><br/>
