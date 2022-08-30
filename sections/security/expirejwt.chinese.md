# 支援黑名單的JWTs

### 一段解釋

按照設計, JWTs(JSON Web Tokens)是完全無狀態的, 因此, 一旦有效令牌由頒發者簽署, 應用程式就可以驗證該令牌是否真實。這導致了安全的問題, 這裡，洩漏的令牌仍可使用且無法撤銷, 因為只要問題令牌提供的簽名與應用程式所期望的相匹配, 簽名就仍然有效。
因此, 在使用JWT身份驗證時, 應用程式應管理過期或已吊銷令牌的黑名單, 以便在需要吊銷令牌的情況下保留使用者的安全性。

### `express-jwt-blacklist` 示例

在Node.js項目中，使用`express-jwt`，並執行`express-jwt-blacklist`的例子

```javascript
const jwt = require('express-jwt');
const blacklist = require('express-jwt-blacklist');
 
app.use(jwt({
  secret: 'my-secret',
  isRevoked: blacklist.isRevoked
}));
 
app.get('/logout', function (req, res) {
  blacklist.revoke(req.user)
  res.sendStatus(200);
});
```

### 其他部落格作者說什麼

摘自部落格[Marc Busqué](http://waiting-for-dev.github.io/blog/2017/01/25/jwt_secure_usage/):
> ...在JWT之上新增一個吊銷層(revocation layer), 即使它意味著失去其無狀態性質。
