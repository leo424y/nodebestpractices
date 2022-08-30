# 配置環境變數 NODE_ENV = production

<br/><br/>


### 一段解釋

程序的環境變數是一組鍵值對，可用於任何執行程式，通常用於配置。雖然可以使用其他任何變數，但Node鼓勵使用一個名為NODE_ENV的變數來標記我們是否正在開發。這一決定允許元件在開發過程中能提供更好的診斷，例如禁用快取或發出冗長的日誌語句。任何現代部署工具 — Chef、Puppet、CloudFormation等 — 在部署時都支援設定環境變數。

<br/><br/>


### 程式碼例項：配置和讀取NODE_ENV環境變數

```javascript
//在啟動node程序前，在bash中設定環境變數
$ NODE_ENV=development
$ node
 
//使用程式碼讀取環境變數
If(process.env.NODE_ENV === “production”)
    useCaching = true;
```

<br/><br/>


### 其他博主說了什麼
摘自這篇部落格[dynatrace](https://www.dynatrace.com/blog/the-drastic-effects-of-omitting-node_env-in-your-express-js-applications/):
> ...在node.js中有一個約定， 它使用名為NODE_ENV的變數來設定當前工作模式。我們看到它實際上是讀取NODE_ENV，如果它沒有設定，則預設為“development”。我們清楚的看到，通過設定NODE_ENV為production，node.js可以處理請求的數量可以提高大約三分之二，而CPU的使用率會略有下降。 *讓我強調一下:設定NODE_ENV為production可以讓你的應用程式快3倍!*


![NODE_ENV=production](../../assets/images/setnodeenv1.png "NODE_ENV=production")


<br/><br/>

