# 捕獲未處理的promise rejections
<br/><br/>


### 一段解釋

通常，大部分的現代node.js/express應用程式碼執行在promise裡 – 或者是在.then裡處理，一個回撥函數中，或者在一個catch塊中。令人驚訝的是，除非開發者記得新增.catch語句，在這些地方拋出的錯誤都不會被uncaughtException事件處理程式來處理，然後消失掉。當未處理的rejection出現，node最近的版本增加了一個警告訊息，儘管當事情出錯的時候這可能有助於發現問題，但這顯然不是一個適當的錯誤處理方法。簡單明瞭的解決方案是永遠不要忘記在每個promise鏈式呼叫中新增.catch語句，並重定向到一個集中的錯誤處理程式。然而，只在開發人員的規程上構建錯誤處理策略是有些脆弱的。因此，使用一個優雅的回撥並訂閱到process.on（'unhandledrejection'，callback）是高度推薦的 – 這將確保任何promise錯誤，如果不是本地處理，將在這處理。

<br/><br/>

### 程式碼示例: 這些錯誤將不會得到任何錯誤處理程式捕獲（除unhandledrejection）

```javascript
DAL.getUserById(1).then((johnSnow) =>
{
  //this error will just vanish
	if(johnSnow.isAlive == false)
	    throw new Error('ahhhh');
});

```
<br/><br/>
### 程式碼示例: 捕獲 unresolved 和 rejected 的 promise

```javascript
process.on('unhandledRejection', (reason, p) => {
  //我剛剛捕獲了一個未處理的promise rejection, 因為我們已經有了對於未處理錯誤的後備的處理機制（見下面）, 直接拋出，讓它來處理
  throw reason;
});
process.on('uncaughtException', (error) => {
  //我剛收到一個從未被處理的錯誤，現在處理它，並決定是否需要重啟應用
  errorManagement.handler.handleError(error);
  if (!errorManagement.handler.isTrustedError(error))
    process.exit(1);
});

```
<br/><br/>
### 部落格引用: "如果你犯了錯誤，在某個時候你就會犯錯誤。"
 摘自 James Nelson 的部落格
 
 > 讓我們測試一下您的理解。下列哪一項是您期望錯誤將會列印到控制檯的？

```javascript
Promise.resolve(‘promised value’).then(() => {
  throw new Error(‘error’);
});

Promise.reject(‘error value’).catch(() => {
  throw new Error(‘error’);
});

new Promise((resolve, reject) => {
  throw new Error(‘error’);
});
```

> 我不知道您的情況，但我的回答是我希望它們所有都能列印出一個錯誤。然而，現實是許多現代JavaScript環境不會為其中任何一個列印錯誤。做為人的問題是，如果你犯了錯誤，在某個時候你就會犯錯誤。記住這一點，很顯然，我們應該設計這樣一種方式，使錯誤儘可能少創造傷害，這意味著預設地處理錯誤，而不是丟棄錯誤。
