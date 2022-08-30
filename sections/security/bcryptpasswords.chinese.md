# 對於密碼，避免使用Node.js的Crypto庫，使用Bcrypt

### 一段解釋

當儲存使用者密碼的時候，建議使用[bcrypt npm module](https://www.npmjs.com/package/bcrypt)提供的自適應雜湊演算法bcrypt，而不是使用Node.js的crypto模組。由於`Math.random()`的可預測性，它也不應該作為密碼或者令牌生成的一部分。

相較於JavaScript實現，`bcrypt`或者類似的模組應該被使用。當使用`bcrypt`時，可以指定相應數量的回合數（rounds），以提供安全的雜湊。這將設定work factor或者用於資料處理的回合次數，而更多的雜湊回合次數導致更安全的雜湊值（儘管這是CPU耗時的代價）。雜湊回合（hashing rounds）的引入意味著蠻力因子會顯著降低, 因此密碼破解會減慢, 從而增加產生一次嘗試所需的時間。

### 程式碼示例

```javascript
// 使用10個雜湊回合非同步生成安全密碼
bcrypt.hash('myPassword', 10, function(err, hash) {
  // 在使用者記錄中儲存安全雜湊
});

// 將提供的密碼輸入與已儲存的雜湊進行比較
bcrypt.compare('somePassword', hash, function(err, match) {
  if(match) {
   // 密碼匹配
  } else {
   // 密碼不匹配
  } 
});
```

### 其他部落格作者的說法

摘自部落格[Max McCarty](https://dzone.com/articles/nodejs-and-password-storage-with-bcrypt):
> ... 它不只是使用正確的雜湊演算法。我已經廣泛討論了正確的工具，包括必要的成分"時間"，如何作為密碼雜湊演算法的一部分, 以及它對於試圖通過蠻力破解密碼的攻擊者，意味著什麼。
