# 在沙箱中執行不安全的程式碼

### 一段解釋

根據經驗, 應該只執行自己的javascript檔案。撇開理論不談, 現實世界中的場景需要執行在執行時動態傳遞的javascript檔案。例如, 考慮一個動態框架(如 webpack), 該框架接受自定義載入器(custom loaders), 並在構建時動態執行這些載入器。在存在一些惡意外掛的情況下, 我們希望最大限度地減少損害, 甚至可能讓工作流成功終止 - 這需要在一個沙箱環境中執行外掛, 該環境在資源、宕機和我們共享的資訊方面是完全隔離的。三個主要選項可以幫助實現這種隔離:

- 一個專門的子程序 - 這提供了一個快速的資訊隔離, 但要求制約子程序, 限制其執行時間, 並從錯誤中恢復
- 一個基於雲的無服務框架滿足所有沙盒要求，但動態部署和呼叫Faas方法不是本部分的內容
- 一些npm庫，比如[sandbox](https://www.npmjs.com/package/sandbox)和[vm2](https://www.npmjs.com/package/vm2)允許通過一行程式碼執行隔離程式碼。儘管後一種選擇在簡單中獲勝, 但它提供了有限的保護。

### 程式碼示例 - 使用Sandbox庫執行隔離程式碼

```javascript
const Sandbox = require("sandbox");
const s = new Sandbox();

s.run( "lol)hai", function( output ) {
  console.log(output);
  //output='Syntax error'
});

// Example 4 - Restricted code
s.run( "process.platform", function( output ) {
  console.log(output);
  //output=Null
})

// Example 5 - Infinite loop
s.run( "while (true) {}", function( output ) {
  console.log(output);
  //output='Timeout'
})
```
