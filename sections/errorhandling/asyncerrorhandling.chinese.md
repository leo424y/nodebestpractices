# 對於非同步的錯誤處理，請使用 Async-Await 或者 promises

### 一段解釋
由於大多數的程式設計師不熟悉回撥，不能很好的掌控回撥函數，導致被迫到處檢測錯誤，處理讓人不快的程式碼巢狀和難以理解的程式碼流程。Promise 的庫，比如 BlueBird，async，和 Q 封裝了一些程式碼，使用者可以使用 RETURN 和 THROW 的方式來控制程式的流程。具體來說，就是它們支援最受歡迎的 try-catch 錯誤處理風格，這使得主流程程式碼從在每一個方法中處理錯誤的方式中解放出來。

### 程式碼示例 – 使用 promises 捕獲錯誤

```javascript
doWork()
  .then(doWork)
  .then(doOtherWork)
  .then((result) => doWork)
  .catch((error) => {
    throw error;
  })
  .then(verify);
```

### 程式碼示例 - 使用 async/await 捕獲錯誤

```javascript
async function executeAsyncTask() {
  try {
    const valueA = await functionA();
    const valueB = await functionB(valueA);
    const valueC = await functionC(valueB);
    return await functionD(valueC);
  } catch (err) {
    logger.error(err);
  } finally {
    await alwaysExecuteThisFunction();
  }
}
```

### 程式碼示例 反模式 – 回撥方式的錯誤處理

<details>
<summary><strong>Javascript</strong></summary>

```javascript
getData(someParameter, function(err, result){
    if(err != null)
      //做一些事情類似於呼叫給定的回撥函數並傳遞錯誤
      getMoreData(a, function(err, result){
        if(err != null)
          //做一些事情類似於呼叫給定的回撥函數並傳遞錯誤
          getMoreData(b, function(c){
            getMoreData(d, function(e){
              if(err != null)
                //你有什麼想法? 
    });
});
```

</details>

<details>
<summary><strong>Typescript</strong></summary>

```typescript
getData(someParameter, function (err: Error | null, resultA: ResultA) {
  if (err !== null) {
    //做一些事情類似於呼叫給定的回撥函數並傳遞錯誤
    getMoreData(resultA, function (err: Error | null, resultB: ResultB) {
      if (err !== null) {
        //做一些事情類似於呼叫給定的回撥函數並傳遞錯誤
        getMoreData(resultB, function (resultC: ResultC) {
          getMoreData(resultC, function (err: Error | null, d: ResultD) {
            if (err !== null) {
              // 你有什麼想法？
            }
          });
        });
      }
    });
  }
});
```

</details>

### 部落格引用: "我們使用 promise 有一個問題"

摘自部落格 pouchdb.com

> ……實際上, 回撥會做一些更險惡的事情: 他們剝奪了我們的 stack, 這是我們通常在程式語言中想當然的事情。編寫沒有堆棧的程式碼很像駕駛一輛沒有剎車踏板的汽車: 你沒有意識到你有多麼需要它, 直到你伸手去找它, 而它不在那裡。promise 的全部目的是讓我們回到我們在非同步時丟失的語言基礎: return，throw 和 stack。但你必須知道如何正確使用 promise, 以便利用他們。

### 部落格引用: "promise 方法更加緊湊"

摘自部落格 gosquared.com

> ………promise 的方法更緊湊, 更清晰, 寫起來更快速。如果在任何 ops 中發生錯誤或異常,則由單個.catch()處理程式處理。有這個單一的地方來處理所有的錯誤意味著你不需要為每個階段的工作寫錯誤檢查。

### 部落格引用: "原生 ES6 支援 promise，可以和 generator 一起使用"

摘自部落格 StrongLoop

> ….回撥有一個糟糕的錯誤處理的報道。promise 更好。將 express 內建的錯誤處理與 promise 結合起來, 大大降低了 uncaught exception 的機率。原生 ES6 支援 promise, 通過編譯器 babel，它可以與 generator，ES7 提議的技術(比如 async/await)一起使用。

### 部落格引用: "所有那些您所習慣的常規的流量控制結構, 完全被打破"

摘自部落格 Benno’s

> ……關於基於非同步、回撥程式設計的最好的事情之一是, 基本上所有那些您習慣的常規流量控制結構, 完全被打破。然而, 我發現最易打破的是處理異常。Javascript 提供了一個相當熟悉的 try...catch 結構來處理異常。異常的問題是, 它們提供了在一個呼叫堆棧上 short-cutting 錯誤的很好的方法, 但最終由於不同堆棧上發生的錯誤導致完全無用…
