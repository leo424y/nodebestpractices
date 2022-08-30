# 避免不安全的重定向

### 一段解釋

當我們在 Node.js 或者 Express 中實現重定向時，在伺服器端進行輸入校驗非常重要。當攻擊者發現你沒有校驗使用者提供的外部輸入時，他們會在論壇、社交媒體以和其他公共場合釋出他們精心製作的連結來誘使使用者點選，以此達到漏洞利用的目的。

案例： express 使用使用者輸入的不安全的重定向

```javascript
const express = require('express');
const app = express();

app.get('/login', (req, res, next) => {

  if (req.session.isAuthenticated()) {
    res.redirect(req.query.url);
  }

}); 
```

建議的避免不安全重定向的方案是，避免依賴使用者輸入的內容來進行重定向。如果一定要使用使用者輸入的內容，可以通過使用白名單重定向的方式來避免暴露漏洞。

案例：使用白名單實現安全的重定向

```javascript
const whitelist = { 
  'https://google.com': 1 
};

function getValidRedirect(url) { 
    // 檢查url是否以/開頭
  if (url.match(/^\/(?!\/)/)) { 
    // 前置我們的域名來確保（安全）
    return 'https://example.com' + url; 
  } 

    // 否則對照白名單列表
  return whitelist[url] ? url : '/'; 
}

app.get('/login', (req, res, next) => {

  if (req.session.isAuthenticated()) {
    res.redirect(getValidRedirect(req.query.url));
  }

}); 
```

### 其他博主的看法

來自部落格[NodeSwat](https://blog.nodeswat.com/unvalidated-redirects-b0a2885720db)：

> 幸運的是，緩解此漏洞的方法非常簡單-不要使用未經驗證的使用者輸入作為重定向的基礎。

來自部落格[Hailstone](https://blog.hailstone.io/how-to-prevent-unsafe-redirects-in-node-js/)：

> 但是，如果伺服器端的重定向邏輯沒有對url參數的資料進行校驗的話，則你的使用者可能最終訪問的地址跟你的地址看起來幾乎完全一致（examp1e.com），但這最終滿足了犯罪黑客們的需求。
