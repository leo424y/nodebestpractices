[✔]: assets/images/checkbox-small-blue.png

# Node.js 最佳實踐

<h1 align="center">
  <img src="assets/images/banner-2.jpg" alt="Node.js Best Practices" />
</h1>

<br/>

<div align="center">
  <img src="https://img.shields.io/badge/⚙%20Item%20count%20-%2082%20Best%20Practices-blue.svg" alt="82 items"/> <img src="https://img.shields.io/badge/%F0%9F%93%85%20Last%20update%20-%20Jun%205%202019-green.svg" alt="Last update: June 5, 2019"/> <img src="https://img.shields.io/badge/ %E2%9C%94%20Updated%20For%20Version%20-%20Node%2012.4.0%20LTS-brightgreen.svg" alt="Updated for Node 12.4.0 LTS"/>
</div>

<br/>

 [![nodepractices](./assets/images/twitter-s.png)](https://twitter.com/nodepractices/) **Follow us on Twitter!** [**@nodepractices**](https://twitter.com/nodepractices/)
 <br/>

# 歡迎! 首先您應該知道的三件事情:
**1. 當您讀到這裡，實際上您讀了很多關於Node.js的優秀文章 -** 這是對Node.js最佳實踐中排名最高的內容的總結和分享

**2. 這裡是最大的彙集，且每週都在增長 -** 當前，超過50個最佳實現，樣式指南，架構建議已經呈現。每天都有新的issue和PR被建立，以使這本線上書籍不斷更新。我們很樂於見到您能在這裡做出貢獻，不管是修復一些程式碼的錯誤，或是提出絕妙的新想法。請檢視我們的[milestones](https://github.com/goldbergyoni/nodebestpractices/milestones?direction=asc&sort=due_date&state=open)

**3. 大部分的條目包含額外的資訊 -** 大部分的最佳實踐條目的旁邊，您將發現 **🔗Read More** 連結，它將呈現給您示例程式碼，部落格引用和更多資訊

<br/><br/><br/>

## [目錄](#table-of-contents)
1. [項目結構實踐 (5) ](#1-project-structure-practices)
2. [異常處理實踐 (11) ](#2-error-handling-practices)
3. [編碼規範實踐 (12) ](#3-code-style-practices)
4. [測試和總體質量實踐 (8) ](#4-testing-and-overall-quality-practices)
5. [進入生產實踐 (16) ](#5-going-to-production-practices)
6. :star: 新: [安全實踐(23)](#6-security-best-practices)
7. Performance Practices ([coming soon](https://github.com/goldbergyoni/nodebestpractices/milestones?direction=asc&sort=due_date&state=open))


<br/><br/><br/>
<h1 id="1-project-structure-practices"><code>1. 項目結構實踐</code></h1>

## ![✔] 1.1 元件式構建你的解決方案

 **TL;DR:** 大型項目的最壞的隱患就是維護一個龐大的，含有幾百個依賴的程式碼庫 - 當開發人員準備整合新的需求的時候，這樣一個龐然大物勢必減緩了開發效率。反之，把您的程式碼拆分成元件，每一個元件有它自己的資料夾和程式碼庫，並且確保每一個元件小而簡單。檢視正確的項目結構的例子請訪問下面的 ‘更多’ 連結。

**否則:** 當編寫新需求的開發人員逐步意識到他所做改變的影響，並擔心會破壞其他的依賴模組 - 部署會變得更慢，風險更大。當所有業務邏輯沒有被分開，這也會被認為很難擴充套件

🔗 [**更多: 元件結構**](./sections/projectstructre/breakintcomponents.chinese.md)

<br/><br/>

## ![✔] 1.2 分層設計元件，保持Express在特定的區域

**TL;DR:** 每一個元件都應該包含'層級' - 一個專注的用於接入網路，邏輯，資料的概念。這樣不僅獲得一個清晰的分離考量，而且使模擬和測試系統變得異常容易。儘管這是一個普通的模式，但介面開發者易於混淆層級關係，比如把網路層的物件（Express req, res）傳給業務邏輯和資料層 - 這會令您的應用彼此依賴，並且只能通過Express使用。

**否則:** 對於混淆了網路層和其它層的應用，將不易於測試，執行CRON的任務，其它非-Express的呼叫者無法使用

🔗 [**更多: 應用分層**](./sections/projectstructre/createlayers.chinese.md)

<br/><br/>

## ![✔] 1.3 封裝公共模組成為NPM的包

**TL;DR:** 由大量程式碼構成的一個大型應用中，貫徹全局的，比如日誌，加密和其它類似的公共元件，應該進行封裝，並暴露成一個私有的NPM包。這將使其在更多的程式碼庫和項目中被使用變成了可能。

**否則:** 您將不得不重造部署和依賴的輪子

🔗 [**更多: 通過需求構建**](./sections/projectstructre/wraputilities.chinese.md)

<br/><br/>

## ![✔] 1.4 分離 Express 'app' and 'server'

**TL;DR:** 避免定義整個[Express](https://expressjs.com/)應用在一個單獨的大檔案裡， 這是一個不好的習慣 - 分離您的 'Express' 定義至少在兩個檔案中： API聲明(app.js) 和 網路相關(WWW)。對於更好的結構，是把你的API聲明放在元件中。

**否則:** 您的API將只能通過HTTP的呼叫進行測試（慢，並且很難產生測試覆蓋報告）。維護一個有著上百行程式碼的檔案也不是一個令人開心的事情。

🔗 [**更多: 分離 Express 'app' and 'server'**](./sections/projectstructre/separateexpress.chinese.md)

<br/><br/>

## ![✔] 1.5 使用易於設定環境變數，安全和分級的配置


**TL;DR:** 一個完美無瑕的配置安裝應該確保 (a) 元素可以從檔案中，也可以從環境變數中讀取 (b) 密碼排除在提交的程式碼之外 (c) 為了易於檢索，配置是分級的。僅有幾個包可以滿足這樣的條件，比如[rc](https://www.npmjs.com/package/rc), [nconf](https://www.npmjs.com/package/nconf), [config](https://www.npmjs.com/package/config) 和 [convict](https://www.npmjs.com/package/convict)。

**否則:** 不能滿足任意的配置要求將會使開發，運維團隊，或者兩者，易於陷入泥潭。

🔗 [**更多: 配置最佳實踐**](./sections/projectstructre/configguide.chinese.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ 返回頂部</a></p>

<h1 id="2-error-handling-practices"><code>2. 錯誤處理最佳實踐</code></h1>

## ![✔] 2.1  使用 Async-Await 和 promises 用於非同步錯誤處理

**TL;DR:** 使用回撥的方式處理非同步錯誤可能是導致災難的最快的方式(a.k.a the pyramid of doom)。對您的程式碼來說，最好的禮物就是使用規範的promise庫或async-await來替代，這會使其像try-catch一樣更加簡潔，具有熟悉的程式碼結構。

**否則:** Node.js回撥特性, function(err, response), 是導致不可維護程式碼的一個必然的方式。究其原因，是由於混合了隨意的錯誤處理程式碼，臃腫的內嵌，蹩腳的程式碼模式。

🔗 [**更多: 避免回撥**](./sections/errorhandling/asyncerrorhandling.chinese.md)

<br/><br/>

## ![✔] 2.2 僅使用內建的錯誤物件

**TL;DR:** 很多人拋出異常使用字元串類型或一些自定義類型 - 這會導致錯誤處理邏輯和模組間的呼叫複雜化。是否您reject一個promise，拋出異常或發出(emit)錯誤 - 使用內建的錯誤物件將會增加設計一致性，並防止資訊的丟失。


**否則:** 呼叫某些模組，將不確定哪種錯誤類型會返回 - 這將會使恰當的錯誤處理更加困難。更壞的情況是，使用特定的類型描述錯誤，會導致重要的錯誤資訊缺失，比如stack trace！

🔗 [**更多: 使用內建錯誤物件**](./sections/errorhandling/useonlythebuiltinerror.chinese.md)

<br/><br/>

## ![✔] 2.3 區分執行錯誤和程式設計錯誤

**TL;DR:** 執行錯誤（例如, API接受到一個無效的輸入）指的是一些已知場景下的錯誤，這類錯誤的影響已經完全被理解，並能被考慮周全的處理掉。同時，程式設計錯誤（例如，嘗試讀取未定義的變數）指的是未知的編碼問題，影響到應用得當的重啟。

**否則:** 當一個錯誤產生的時候，您總是得重啟應用，但為什麼要讓 ~5000 個線上使用者不能訪問，僅僅是因為一個細微的，可以預測的，執行時錯誤？相反的方案，也不完美 – 當未知的問題（程式問題）產生的時候，使應用依舊可以訪問，可能導致不可預測行為。區分兩者會使處理更有技巧，並在給定的上下文下給出一個平衡的對策。

🔗 [**更多: 執行錯誤和程式設計錯誤**](./sections/errorhandling/operationalvsprogrammererror.chinese.md)

<br/><br/>

## ![✔] 2.4 集中處理錯誤，不要在Express中介軟體中處理錯誤

**TL;DR:** 錯誤處理邏輯，比如給管理員傳送郵件，日誌應該封裝在一個特定的，集中的物件當中，這樣當錯誤產生的時候，所有的終端（例如 Express中介軟體，cron任務，單元測試）都可以呼叫。

**否則:** 錯誤處理的邏輯不放在一起將會導致程式碼重複和非常可能不恰當的錯誤處理。

🔗 [**更多: 集中處理錯誤**](./sections/errorhandling/centralizedhandling.chinese.md)

<br/><br/>

## ![✔] 2.5 對API錯誤使用Swagger文件化

**TL;DR:** 讓你的API呼叫者知道哪種錯誤會返回，這樣他們就能完全的處理這些錯誤，而不至於系統崩潰。Swagger，REST API的文件框架，通常處理這類問題。

**否則:** 任何API的客戶端可能決定崩潰並重啟，僅僅因為它收到一個不能處理的錯誤。注意：API的呼叫者可能是你（在微服務環境中非常典型）。


🔗 [**更多: 使用Swagger記錄錯誤**](./sections/errorhandling/documentingusingswagger.chinese.md)

<br/><br/>

## ![✔] 2.6 當一個特殊的情況產生，停掉服務是得體的

**TL;DR:** 當一個不確定錯誤產生（一個開發錯誤，最佳實踐條款#3) - 這就意味著對應用運轉健全的不確定。一個普通的實踐將是建議仔細地重啟程序，並使用一些‘啟動器’工具，比如Forever和PM2。

**否則:** 當一個未知的異常被拋出，意味著某些物件包含錯誤的狀態（例如某個全局事件發生器由於某些內在的錯誤，不在產生事件），未來的請求可能失敗或者行為異常。

🔗 [**更多: 停掉服務**](./sections/errorhandling/shuttingtheprocess.chinese.md)

<br/><br/>



## ![✔] 2.7 使用一個成熟的日誌工具提高錯誤的可見性

**TL;DR:** 一系列成熟的日誌工具，比如Winston，Bunyan和Log4J，會加速錯誤的發現和理解。忘記console.log吧。

**否則:** 瀏覽console的log，和不通過查詢工具或者一個好的日誌檢視器，手動瀏覽繁瑣的文字檔案，會使你忙於工作到很晚。

🔗 [**更多: 使用好用的日誌工具**](./sections/errorhandling/usematurelogger.chinese.md)


<br/><br/>


## ![✔] 2.8 使用你最喜歡的測試框架測試錯誤流

**TL;DR:** 無論專業的自動化測試或者簡單的手動開發測試 - 確保您的程式碼不僅滿足正常的場景，而且處理並且返回正確的錯誤。測試框架，比如Mocha & Chai可以非常容易的處理這些問題（在"Gist popup"中檢視程式碼例項） 。

**否則:** 沒有測試，不管自動還是手動，您不可能依賴程式碼去返回正確的錯誤。而沒有可以理解的錯誤，那將毫無錯誤處理可言。


🔗 [**更多: 測試錯誤流向**](./sections/errorhandling/testingerrorflows.chinese.md)

<br/><br/>

## ![✔] 2.9 使用APM產品發現錯誤和宕機時間

**TL;DR:** 監控和效能產品 (別名 APM) 先前一步的檢測您的程式碼庫和API，這樣他們能自動的，像使用魔法一樣的強調錯誤，宕機和您忽略的效能慢的部分。

**否則:** 您花了很多的力氣在測量API的效能和錯誤，但可能您從來沒有意識到真實場景下您最慢的程式碼塊和他們對UX的影響。


🔗 [**更多: 使用APM產品**](./sections/errorhandling/apmproducts.chinese.md)

<br/><br/>


## ![✔] 2.10 捕獲未處理的promise rejections

**TL;DR:** 任何在promise中被拋出的異常將被收回和遺棄，除非開發者沒有忘記去明確的處理。即使您的程式碼呼叫的是process.uncaughtException！解決這個問題可以註冊到事件process.unhandledRejection。

**否則:** 您的錯誤將被回收，無蹤跡可循。沒有什麼可以需要考慮。


🔗 [**更多: 捕獲未處理的promise rejection**](./sections/errorhandling/catchunhandledpromiserejection.chinese.md)

<br/><br/>

## ![✔] 2.11 快速查錯，驗證參數使用一個專門的庫

**TL;DR:** 這應該是您的Express最佳實踐中的一部分 – assert API輸入避免難以理解的漏洞，這類漏洞以後會非常難以追蹤。而驗證程式碼通常是一件乏味的事情，除非使用一些非常炫酷的幫助庫比如Joi。

**否則:** 考慮這種情況 – 您的功能期望一個數字參數 “Discount” ，然而呼叫者忘記傳值，之後在您的程式碼中檢查是否 Discount!=0 （允許的折扣值大於零），這樣它將允許使用者使用一個折扣。OMG，多麼不爽的一個漏洞。你能明白嗎？

🔗 [**更多: 快速查錯**](./sections/errorhandling/failfast.chinese.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ 返回頂部</a></p>

<h1 id="3-code-style-practices"><code>3. 編碼風格實踐</code></h1>

## ![✔] 3.1 使用ESLint

**TL;DR:** [ESLint](https://eslint.org)是檢查可能的程式碼錯誤和修復程式碼樣式的事實上的標準，不僅可以識別實際的間距問題, 而且還可以檢測嚴重的反模式程式碼, 如開發人員在不分類的情況下拋出錯誤。儘管ESlint可以自動修復程式碼樣式，但其他的工具比如[prettier](https://www.npmjs.com/package/prettier)和[beautify](https://www.npmjs.com/package/js-beautify)在格式化修復上功能強大，可以和Eslint結合起來使用。

**否則:** 開發人員將必須關注單調乏味的間距和線寬問題, 並且時間可能會浪費在過多考慮項目的程式碼樣式。

<br/><br/>

## ![✔] 3.2 Node.js特定的外掛

**TL;DR:** 除了僅僅涉及 vanilla JS 的 ESLint 標準規則，新增 Node 相關的外掛，比如[eslint-plugin-node](https://www.npmjs.com/package/eslint-plugin-node), [eslint-plugin-mocha](https://www.npmjs.com/package/eslint-plugin-mocha) and [eslint-plugin-node-security](https://www.npmjs.com/package/eslint-plugin-security)

**否則:** 許多錯誤的Node.js程式碼模式可能在檢測下逃生。例如，開發人員可能需要某些檔案，把一個變數作為路徑名 (variableAsPath) ，這會導致攻擊者可以執行任何JS指令碼。Node.JS linters可以檢測這類模式，並及早預警。

<br/><br/>

## ![✔] 3.3 在同一行開始一個程式碼塊的大括號

**TL;DR:** 程式碼塊的第一個大括號應該和聲明的起始保持在同一行中。

### 程式碼示例
```javascript
  // 建議
  function someFunction() {
    // 程式碼塊
  }

  // 避免
  function someFunction()
  {
    // 程式碼塊
  }
```

**否則:** 不遵守這項最佳實踐可能導致意外的結果，在Stackoverflow的帖子中可以檢視到，如下：

🔗 [**更多:** "Why does a results vary based on curly brace placement?" (Stackoverflow)](https://stackoverflow.com/questions/3641519/why-does-a-results-vary-based-on-curly-brace-placement)

<br/><br/>

## ![✔] 3.4 不要忘記分號

**TL;DR:** 即使沒有獲得一致的認同，但在每一個表示式後面放置分號還是值得推薦的。這將使您的程式碼, 對於其他閱讀程式碼的開發者來說，可讀性，明確性更強。

**否則:** 在前面的章節裡面已經提到，如果表示式的末尾沒有新增分號，JavaScript的直譯器會在自動新增一個，這可能會導致一些意想不到的結果。

<br/><br/>

## ![✔] 3.5 命名您的方法

**TL;DR:** 命名所有的方法，包含閉包和回撥, 避免匿名方法。當剖析一個node應用的時候，這是特別有用的。命名所有的方法將會使您非常容易的理解記憶體快照中您正在檢視的內容。

**否則:** 使用一個核心dump（記憶體快照）偵錯線上問題，會是一項非常挑戰的事項，因為你注意到的嚴重記憶體洩漏問題極有可能產生於匿名的方法。

<br/><br/>

## ![✔] 3.6 變數、常量、函數和類的命名約定

**TL;DR:** 當命名變數和方法的時候，使用 ***lowerCamelCase*** ，當命名類的時候，使用 ***UpperCamelCase*** （首字母大寫），對於常量，則 ***UPPERCASE*** 。這將幫助您輕鬆地區分普通變數/函數和需要例項化的類。使用描述性名稱，但使它們儘量簡短。

**否則:** JavaScript是世界上唯一一門不需要例項化，就可以直接呼叫建構函式（"Class"）的編碼語言。因此，類和函數的建構函式由採用UpperCamelCase開始區分。

### 程式碼示例 ###
```javascript
  // 使用UpperCamelCase命名類名
  class SomeClassExample () {

    // 常量使用const關鍵字，並使用lowerCamelCase命名
    const config = {
      key: 'value'
    };

    // 變數和方法使用lowerCamelCase命名
    let someVariableExample = 'value';
    function doSomething() {

    }

  }
```

<br/><br/>

## ![✔] 3.7 使用const優於let，廢棄var

**TL;DR:** 使用`const`意味著一旦一個變數被分配，它不能被重新分配。使用const將幫助您免於使用相同的變數用於不同的用途，並使你的程式碼更清晰。如果一個變數需要被重新分配，以在一個迴圈為例，使用`let`聲明它。let的另一個重要方面是，使用let聲明的變數只在定義它的塊作用域中可用。 `var`是函數作用域，不是塊級作用域，既然您有const和let讓您隨意使用，那麼[不應該在ES6中使用var](https://hackernoon.com/why-you-shouldnt-use-var-anymore-f109a58b9b70)。

**否則:** 當經常更改變數時，偵錯變得更麻煩了。

🔗 [**更多: JavaScript ES6+: var, let, or const?** ](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

<br/><br/>

## ![✔] 3.8 先require, 而不是在方法內部

**TL;DR:** 在每個檔案的起始位置，在任何函數的前面和外部require模組。這種簡單的最佳實踐，不僅能幫助您輕鬆快速地在檔案頂部辨別出依賴關係，而且避免了一些潛在的問題。

**否則:** 在Node.js中，require 是同步執行的。如果從函數中呼叫它們，它可能會阻塞其他請求，在更關鍵的時間得到處理。另外，如果所require的模組或它自己的任何依賴項拋出錯誤並使伺服器崩潰，最好儘快查明它，如果該模組在函數中require的，則可能不是這樣的情況。

<br/><br/>

## ![✔] 3.9 require 資料夾，而不是檔案

**TL;DR:** 當在一個資料夾中開發庫/模組，放置一個檔案index.js暴露模組的
內部，這樣每個消費者都會通過它。這將作為您模組的一個介面，並使未來的變化簡單而不違反規則。

**否則:** 更改檔案內部結構或簽名可能會破壞與客戶端的介面。

### 程式碼示例
```javascript
  // 建議
  module.exports.SMSProvider = require('./SMSProvider');
  module.exports.SMSNumberResolver = require('./SMSNumberResolver');

  // 避免
  module.exports.SMSProvider = require('./SMSProvider/SMSProvider.js');
  module.exports.SMSNumberResolver = require('./SMSNumberResolver/SMSNumberResolver.js');
```

<br/><br/>


## ![✔] 3.10 使用 `===` 操作符

**TL;DR:** 對比弱等於 `==`，優先使用嚴格的全等於 `===` 。`==`將在它們轉換為普通類型後比較兩個變數。在 `===` 中沒有類型轉換，並且兩個變數必須是相同的類型。

**否則:** 與 `==` 操作符比較，不相等的變數可能會返回true。

### 程式碼示例
```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
```
如果使用`===`， 上面所有語句都將返回 false。

<br/><br/>

## ![✔] 3.11 使用 Async Await, 避免回撥

**TL;DR:** Node 8 LTS現已全面支援非同步等待。這是一種新的方式處理非同步請求，取代回撥和promise。Async-await是非阻塞的，它使非同步程式碼看起來像是同步的。您可以給你的程式碼的最好的禮物是用async-await提供了一個更緊湊的，熟悉的，類似try catch的程式碼語法。

**否則:** 使用回撥的方式處理非同步錯誤可能是陷入困境最快的方式 - 這種方式必須面對不停地檢測錯誤，處理彆扭的程式碼內嵌，難以推理編碼流。

🔗[**更多:** async await 1.0 引導](https://github.com/yortus/asyncawait)

<br/><br/>

## ![✔] 3.12 使用 (=>) 箭頭函數

**TL;DR:** 儘管使用 async-await 和避免方法作為參數是被推薦的, 但當處理那些接受promise和回撥的老的API的時候 - 箭頭函數使程式碼結構更加緊湊，並保持了根方法上的語義上下文 (例如 'this')。

**否則:** 更長的程式碼（在ES5方法中）更易於產生缺陷，並讀起來很是笨重。

🔗 [**更多: 這是擁抱箭頭函數的時刻**](https://medium.com/javascript-scene/familiarity-bias-is-holding-you-back-its-time-to-embrace-arrow-functions-3d37e1a9bb75)


<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ 返回頂部</a></p>


<h1 id="4-testing-and-overall-quality-practices"><code>4. 測試和總體的質量實踐</code></h1>

## ![✔] 4.1 至少，編寫API（元件）測試

**TL;DR:** 大多數項目只是因為時間表太短而沒有進行任何自動化測試，或者測試項目失控而正被遺棄。因此，優先從API測試開始，這是最簡單的編寫和提供比單元測試更多覆蓋率的事情（你甚至可能不需要編碼而進行API測試，像[Postman](https://www.getpostman.com/)。之後，如果您有更多的資源和時間，繼續使用高階測試類型，如單元測試、DB測試、效能測試等。

**否則:** 您可能需要花很長時間編寫單元測試，才發現只有20%的系統覆蓋率。

<br/><br/>

## ![✔] 4.2 使用一個linter檢測程式碼問題

**TL;DR:** 使用程式碼linter檢查基本質量並及早檢測反模式。在任何測試之前執行它, 並將其新增為預提交的git鉤子, 以最小化審查和更正任何問題所需的時間。也可在[Section 3](https://github.com/goldbergyoni/nodebestpractices#3-code-style-practices)中查閱編碼樣式實踐

**否則:** 您可能讓一些反模式和易受攻擊的程式碼傳遞到您的生產環境中。


<br/><br/>

## ![✔] 4.3 仔細挑選您的持續整合（CI）平臺

**TL;DR:** 您的持續整合平臺（cicd）將整合各種質量工具（如測試、lint），所以它應該是一個充滿活力的生態系統，包含各種外掛。[jenkins](https://jenkins.io/)曾經是許多項目的預設選項，因為它有最大的社羣，同時也是一個非常強大的平臺，這樣的代價是要求一個陡峭的學習曲線。如今，使用SaaS工具，比如[CircleCI](https://circleci.com)及其他，安裝一套CI解決方案，相對是一件容易的事情。這些工具允許構建靈活的CI管道，而無需管理整個基礎設施。最終，這是一個魯棒性和速度之間的權衡 - 仔細選擇您支援的方案。

**否則:** 一旦您需要一些高階定製，選擇一些細分市場供應商可能會讓您停滯不前。另一方面，伴隨著jenkins，可能會在基礎設施設定上浪費寶貴的時間。

🔗 [**更多: 挑選 CI 平臺**](./sections/testingandquality/citools.chinese.md)

<br/><br/>

## ![✔] 4.4 經常檢查易受攻擊的依賴

**TL;DR:** 即使是那些最有名的依賴模組，比如Express，也有已知的漏洞。使用社羣和商業工具，比如 🔗 [npm audit](https://docs.npmjs.com/cli/audit) ，整合在您的CI平臺上，在每一次構建的時候都會被呼叫，這樣可以很容易地解決漏洞問題。

**否則:** 在沒有專用工具的情況下，使程式碼清除漏洞，需要不斷地跟蹤有關新威脅的線上出版物，相當繁瑣。

<br/><br/>

## ![✔] 4.5 測試標籤化

**TL;DR:**  不同的測試必須執行在不同的情景：quick smoke，IO-less，當開發者儲存或提交一個檔案，測試應該啟動；完整的端到端的測試通常執行在一個新的pull request被提交之後，等等。這可以通過對測試用例設定標籤，比如關鍵字像#cold #api #sanity，來完成。這樣您可以對您的測試集進行grep，呼叫需要的子集。例如，這就是您通過[Mocha](https://mochajs.org/)僅僅呼叫sanity測試集所需要做的：mocha --grep 'sanity'。

**否則:** 執行所有的測試，包括執行資料庫查詢的幾十個測試，任何時候開發者進行小的改動都可能很慢，這使得開發者不願意執行測試。

<br/><br/>

## ![✔] 4.6 檢查測試覆蓋率，它有助於識別錯誤的測試模式

**TL;DR:** 程式碼覆蓋工具比如 [Istanbul](https://github.com/istanbuljs/istanbuljs)/[NYC](https://github.com/istanbuljs/nyc)，很好用有3個原因：它是免費的（獲得這份報告不需要任何開銷），它有助於確定測試覆蓋率降低的部分，以及最後但非最不重要的是它指出了測試中的不匹配：通過檢視顏色標記的程式碼覆蓋報告您可以注意到，例如，從來不會被測到的程式碼片段像catch語句（即測試只是呼叫正確的路徑，而不呼叫應用程式發生錯誤時的行為）。如果覆蓋率低於某個閾值，則將其設定為失敗的構建。

**否則:** 當你的大部分程式碼沒有被測試覆蓋時，就不會有任何自動化的度量指標告訴你了。



<br/><br/>

## ![✔] 4.7 檢查過期的依賴包

**TL;DR:** 使用您的首選工具 (例如 “npm outdated” or [npm-check-updates](https://www.npmjs.com/package/npm-check-updates) 來檢測已安裝的過期依賴包, 將此檢查注入您的 CI 管道, 甚至在嚴重的情況下使構建失敗。例如, 當一個已安裝的依賴包滯後5個補丁時 (例如:本地版本是1.3.1 的, 儲存庫版本是1.3.8 的), 或者它被其作者標記為已棄用, 可能會出現嚴重的情況 - 停掉這次構建並防止部署此版本。

**否則:** 您的生產環境將執行已被其作者明確標記為有風險的依賴包

<br/><br/>

## ![✔] 4.8 對於e2e testing，使用docker-compose

**TL;DR:** 端對端(e2e)測試包含現場資料，由於它依賴於很多重型服務如資料庫，習慣被認為是CI過程中最薄弱的環節。Docker-compose通過制定類似生產的環境，並使用一個簡單的文字檔案和簡單的命令，輕鬆化解了這個問題。它為了e2e測試，允許製作所有相關服務，資料庫和隔離網路。最後但並非最不重要的一點是，它可以保持一個無狀態環境，該環境在每個測試套件之前被呼叫，然後立即消失。


**否則:** 沒有docker-compose，團隊必須維護一個測試資料庫在每一個測試環境上，包含開發機器，保持所有資料同步，這樣測試結果不會因環境不同而不同。


<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ 返回頂部</a></p>

<h1 id="5-going-to-production-practices"><code>5. 上線實踐</code></h1>

## ![✔] 5.1. 監控!

**TL;DR:** 監控是一種在顧客之前發現問題的遊戲 – 顯然這應該被賦予前所未有的重要性。考慮從定義你必須遵循的基本度量標準開始（我的建議在裡面），到檢查附加的花哨特性並選擇解決所有問題的解決方案。市場已經淹沒其中。點選下面的 ‘The Gist’ ，瞭解解決方案的概述。

**否則:** 錯誤 === 失望的客戶. 非常簡單.


🔗 [**更多: 監控!**](./sections/production/monitoring.chinese.md)

<br/><br/>

## ![✔] 5.2. 使用智慧日誌增加透明度

**TL;DR:** 日誌可以是偵錯語句的一個不能說話的倉庫，或者表述應用執行過程的一個漂亮儀表板的驅動。從第1天計劃您的日誌平臺：如何收集、儲存和分析日誌，以確保所需資訊（例如，錯誤率、通過服務和伺服器等完成整個事務）都能被提取出來。

**否則:** 您最終像是面對一個黑盒，不知道發生了什麼事情，然後你開始重新寫日誌語句新增額外的資訊。


🔗 [**更多: 使用智慧日誌增加透明度**](./sections/production/smartlogging.chinese.md)

<br/><br/>

## ![✔] 5.3. 委託可能的一切（例如：gzip，SSL）給反向代理

**TL;DR:** Node處理CPU密集型任務，如gzipping，SSL termination等，表現糟糕。相反，使用一個 ‘真正’ 的中介軟體服務像Nginx，HAProxy或者雲供應商的服務。

**否則:** 可憐的單執行緒Node將不幸地忙於處理網路任務，而不是處理應用程式核心，效能會相應降低。


🔗 [**更多: 委託可能的一切（例如：gzip，SSL）給反向代理**](./sections/production/delegatetoproxy.chinese.md)

<br/><br/>

## ![✔] 5.4. 鎖住依賴

**TL;DR:** 您的程式碼必須在所有的環境中是相同的，但是令人驚訝的是，NPM預設情況下會讓依賴在不同環境下發生偏移 – 當在不同的環境中安裝包的時候，它試圖拿包的最新版本。克服這種問題可以利用NPM配置檔案， .npmrc，告訴每個環境儲存準確的（不是最新的）包的版本。另外，對於更精細的控制，使用NPM “shrinkwrap”。*更新：作為NPM5，依賴預設鎖定。新的包管理工具，Yarn，也預設鎖定。

**否則:** QA測試通過的程式碼和批准的版本，在生產中表現不一致。更糟糕的是，同一生產叢集中的不同伺服器可能執行不同的程式碼。


🔗 [**更多: 鎖住依賴**](./sections/production/lockdependencies.chinese.md)

<br/><br/>

## ![✔] 5.5. 使用正確的工具保護程序正常執行

**TL;DR:** 程序必須繼續執行，並在失敗時重新啟動。對於簡單的情況下，“重啟”工具如PM2可能足夠，但在今天的“Dockerized”世界 – 叢集管理工具也值得考慮

**否則:** 執行幾十個例項沒有明確的戰略和太多的工具（叢集管理，docker，PM2）可能導致一個DevOps混亂


🔗 [**更多: 使用正確的工具保護程序正常執行**](./sections/production/guardprocess.chinese.md)


<br/><br/>

## ![✔] 5.6. 利用CPU多核

**TL;DR:** 在基本形式上，node應用程式執行在單個CPU核心上，而其他都處於空閒狀態。複製node程序和利用多核，這是您的職責 – 對於中小應用，您可以使用Node Cluster和PM2. 對於一個大的應用，可以考慮使用一些Docker cluster（例如k8s，ECS）複製程序或基於Linux init system（例如systemd）的部署指令碼

**否則:** 您的應用可能只是使用了其可用資源中的25% (!)，甚至更少。注意，一臺典型的伺服器有4個或更多的CPU，預設的Node.js部署僅僅用了一個CPU（甚至使用PaaS服務，比如AWS beanstalk，也一樣）。


🔗 [**更多: 利用所有的CPU**](./sections/production/utilizecpu.chinese.md)

<br/><br/>

## ![✔] 5.7. 建立一個“維護端點”

**TL;DR:** 在一個安全的API中暴露一組系統相關的資訊，比如記憶體使用情況和REPL等等。儘管這裡強烈建議依賴標準和作戰測試工具，但一些有價值的資訊和操作更容易使用程式碼完成。

**否則:** 您會發現，您正在執行許多“診斷部署” – 將程式碼傳送到生產中，僅僅只為了診斷目的提取一些資訊。


🔗 [**更多: 建立一個 '維護端點'**](./sections/production/createmaintenanceendpoint.chinese.md)

<br/><br/>

## ![✔] 5.8. 使用APM產品發現錯誤和宕機時間

**TL;DR:** 監控和效能的產品（即APM）先前一步地評估程式碼庫和API，自動的超過傳統的監測，並測量在服務和層級上的整體使用者體驗。例如，一些APM產品可以突顯導致終端使用者負載過慢的事務，同時指出根本原因。

**否則:** 你可能會花大力氣測量API效能和停機時間，也許你永遠不會知道，真實場景下哪個是你最慢的程式碼部分，這些怎麼影響使用者體驗。


🔗 [**更多: 使用APM產品發現錯誤和宕機時間**](./sections/production/apmproducts.chinese.md)


<br/><br/>


## ![✔] 5.9. 使您的程式碼保持生產環境就緒

**TL;DR:** 在意識中抱著最終上線的想法進行編碼，從第1天開始計劃上線。這聽起來有點模糊，所以我編寫了一些與生產維護密切相關的開發技巧（點選下面的要點）

**否則:** 一個世界冠軍級別的IT/運維人員也不能拯救一個編碼低劣的系統。


🔗 [**更多: 使您的程式碼保持生產環境就緒**](./sections/production/productioncode.chinese.md)

<br/><br/>

## ![✔] 5.10. 測量和保護記憶體使用

**TL;DR:** Node.js和記憶體有引起爭論的聯絡：V8引擎對記憶體的使用有稍微的限制（1.4GB），在node的程式碼裡面有記憶體洩漏的很多途徑 – 因此監視node的程序記憶體是必須的。在小應用程式中，你可以使用shell命令週期性地測量記憶體，但在中等規模的應用程式中，考慮把記憶體監控建成一個健壯的監控系統。

**否則:** 您的記憶體可能一天洩漏一百兆，就像曾發生在沃爾瑪的一樣。


🔗 [**更多: 測量和保護記憶體使用**](./sections/production/measurememory.chinese.md)

<br/><br/>


## ![✔] 5.11. Node外管理您的前端資源

**TL;DR:** 使用專門的中介軟體（nginx，S3，CDN）服務前端內容，這是因為在處理大量靜態檔案的時候，由於node的單執行緒模型，它的效能很受影響。

**否則:** 您的單個node執行緒將忙於傳輸成百上千的html/圖片/angular/react檔案，而不是分配其所有的資源為了其擅長的任務 – 服務動態內容


🔗 [**更多: Node外管理您的前端資源**](./sections/production/frontendout.chinese.md)

<br/><br/>


## ![✔] 5.12. 保持無狀態，幾乎每天都要停下伺服器

**TL;DR:** 在外部資料儲存上，儲存任意類型資料（例如使用者會話，快取，上傳檔案）。考慮間隔地停掉您的伺服器或者使用 ‘serverless’ 平臺（例如 AWS Lambda），這是一個明確的強化無狀態的行為。

**否則:** 某個伺服器上的故障將導致應用程式宕機，而不僅僅是停用故障機器。此外，由於依賴特定伺服器，伸縮彈性會變得更具挑戰性。


🔗 [**更多: 保持無狀態，幾乎每天都要停下伺服器**](./sections/production/bestateless.chinese.md)


<br/><br/>


## ![✔] 5.13. 使用自動檢測漏洞的工具

**TL;DR:** 即使是最有信譽的依賴項，比如Express，會有使系統處於危險境地的已知漏洞（隨著時間推移）。通過使用社羣的或者商業工具，不時的檢查漏洞和警告（本地或者Github上），這類問題很容易被抑制，有些問題甚至可以立即修補。

**否則:** 否則: 在沒有專用工具的情況下，使程式碼清除漏洞，需要不斷地跟蹤有關新威脅的線上出版物。相當繁瑣。


🔗 [**更多: 使用自動檢測漏洞的工具**](./sections/production/detectvulnerabilities.chinese.md)

<br/><br/>


## ![✔] 5.14. 在每一個log語句中指明 ‘TransactionId’

**TL;DR:** 在每一個請求的每一條log入口，指明同一個標識符，transaction-id: {某些值}。然後在檢查日誌中的錯誤時，很容易總結出前後發生的事情。不幸的是，由於Node非同步的天性自然，這是不容易辦到的，看下程式碼裡面的例子

**否則:** 在沒有上下文的情況下檢視生產錯誤日誌，這會使問題變得更加困難和緩慢去解決。


🔗 [**更多: 在每一個log語句中指明 ‘TransactionId’**](./sections/production/assigntransactionid.chinese.md)

<br/><br/>


## ![✔] 5.15. 設定NODE_ENV=production

**TL;DR:** 設定環境變數NODE_ENV為‘production’ 或者 ‘development’，這是一個是否啟用上線優化的標誌 - 很多NPM的包通過它來判斷當前的環境，據此優化生產環境程式碼。

**否則:** 遺漏這個簡單的屬性可能大幅減弱效能。例如，在使用Express作為服務端渲染頁面的時候，如果未設定NODE_ENV，效能將會減慢大概三分之一！


🔗 [**更多: 設定NODE_ENV=production**](./sections/production/setnodeenv.chinese.md)


<br/><br/>


## ![✔] 5.16. 設計自動化、原子化和零停機時間部署

**TL;DR:** 研究表明，執行許多部署的團隊降低了嚴重上線問題的可能性。不需要危險的手動步驟和服務停機時間的快速和自動化部署大大改善了部署過程。你應該達到使用Docker結合CI工具，使他們成為簡化部署的行業標準。

**否則:** 長時間部署 -> 線上宕機 & 和人相關的錯誤 -> 團隊部署時不自信 -> 更少的部署和需求

<br/><br/>

## ![✔] 5.17. 使用 Node.js 的 LTS 版本

**TL;DR:** 確保您是使用LTS版本的Node.js來獲取關鍵的錯誤修復、安全更新和效能改進。

**否則:** 新發現的錯誤或漏洞可能會被用於生產環境中執行的應用程式，您的應用程式可能會變得難以維護且不受各種模組支援.


🔗 [**更多: 使用node.js的LTS版本**](./sections/production/LTSrelease.chinese.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ 返回頂部</a></p>

<h1 id="6-security-best-practices"><code>6. 安全最佳實踐</code></h1>

<div align="center">
<img src="https://img.shields.io/badge/OWASP%20Threats-Top%2010-green.svg" alt="53 items"/>
</div>

## ![✔] 6.1. 擁護linter安全準則

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20XSS%20-green.svg" alt=""/></a>

**TL;DR:** 使用安全相關的linter外掛，比如[eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security)，儘早捕獲安全隱患或者問題，最好在編碼階段。這能幫助察覺安全的問題，比如使用eval，呼叫子程序，或者根據字面含義（比如，使用者輸入）引入模組等等。點選下面‘更多’獲得一個安全linter可以檢測到的程式碼示例。

**Otherwise:** 在開發過程中, 可能一個直白的安全隱患, 成為生產環境中一個嚴重問題。此外, 項目可能沒有遵循一致的安全規範, 而導致引入漏洞, 或敏感資訊被提交到遠端倉庫中。

🔗 [**更多: Lint 規範**](./sections/security/lintrules.md)

<br/><br/>

## ![✔] 6.2. 使用中介軟體限制併發請求

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** DOS攻擊非常流行而且相對容易處理。使用外部服務，比如cloud負載均衡, cloud防火牆, nginx, 或者（對於小的，不是那麼重要的app）一個速率限制中介軟體(比如[express-rate-limit](https://www.npmjs.com/package/express-rate-limit))，來實現速率限制。

**否則:** 應用程式可能受到攻擊, 導致拒絕服務, 在這種情況下, 真實使用者會遭受服務降級或不可用。

🔗 [**更多: 實施速率限制**](./sections/security/limitrequests.md)

<br/><br/>

## ![✔] 6.3 把機密資訊從配置檔案中抽離出來，或者使用包對其加密

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A3-Sensitive_Data_Exposure" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A3:Sensitive%20Data%20Exposure%20-green.svg" alt=""/></a>

**TL;DR:** 不要在配置檔案或原始碼中儲存純文字機密資訊。相反, 使用諸如Vault產品、Kubernetes/Docker Secrets或使用環境變數之類的安全管理系統。最後一個結果是, 儲存在原始碼管理中的機密資訊必須進行加密和管理 (滾動金鑰(rolling keys)、過期時間、稽覈等)。使用pre-commit/push鉤子防止意外提交機密資訊。

**否則:** 原始碼管理, 即使對於私有倉庫, 也可能會被錯誤地公開, 此時所有的祕密資訊都會被公開。外部組織的原始碼管理的訪問許可權將無意中提供對相關係統 (資料庫、api、服務等) 的訪問。

🔗 [**更多: 安全管理**](./sections/security/secretmanagement.md)

<br/><br/>

## ![✔] 6.4. 使用 ORM/ODM 庫防止查詢注入漏洞

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** 要防止 SQL/NoSQL 注入和其他惡意攻擊, 請始終使用 ORM/ODM 或database庫來轉義資料或支援命名的或索引的參數化查詢, 並注意驗證使用者輸入的預期類型。不要只使用JavaScript模板字元串或字元串串聯將值插入到查詢語句中, 因為這會將應用程式置於廣泛的漏洞中。所有知名的Node.js資料訪問庫(例如[Sequelize](https://github.com/sequelize/sequelize), [Knex](https://github.com/tgriesser/knex), [mongoose](https://github.com/Automattic/mongoose))包含對注入漏洞的內建包含措施。

**否則:** 未經驗證或未脫敏處理的使用者輸入，可能會導致操作員在使用MongoDB進行NoSQL操作時進行注入, 而不使用適當的過濾系統或ORM很容易導致SQL隱碼攻擊, 從而造成巨大的漏洞。

🔗 [**更多: 使用 ORM/ODM 庫防止查詢注入**](./sections/security/ormodmusage.md)

<br/><br/>

## ![✔] 6.5. 通用安全最佳實踐集合

**TL;DR:** 這些是與Node.js不直接相關的安全建議的集合-Node的實現與任何其他語言沒有太大的不同。單擊 "閱讀更多" 瀏覽。

🔗 [**更多: 通用安全最佳實踐**](./sections/security/commonsecuritybestpractices.md)

<br/><br/>

## ![✔] 6.6. 調整 HTTP 響應頭以加強安全性

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** 應用程式應該使用安全的header來防止攻擊者使用常見的攻擊方式，諸如跨站點指令碼(XSS)、點選劫持和其他惡意攻擊。可以使用模組，比如 [helmet](https://www.npmjs.com/package/helmet)輕鬆進行配置。

**否則:** 攻擊者可以對應用程式的使用者進行直接攻擊, 導致巨大的安全漏洞

🔗 [**更多: 在應用程式中使用安全的header**](./sections/security/secureheaders.md)

<br/><br/>

## ![✔] 6.7. 經常自動檢查易受攻擊的依賴庫

<a href="https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Known%20Vulnerabilities%20-green.svg" alt=""/></a>

**TL;DR:** 在npm的生態系統中, 一個項目有許多依賴是很常見的。在找到新的漏洞時, 應始終將依賴項保留在檢查中。使用工具，類似於[npm audit](https://docs.npmjs.com/cli/audit) 或者 [snyk](https://snyk.io/)跟蹤、監視和修補易受攻擊的依賴項。將這些工具與 CI 設定整合, 以便在將其上線之前捕捉到易受攻擊的依賴庫。

**否則:** 攻擊者可以檢測到您的web框架並攻擊其所有已知的漏洞。

🔗 [**更多: 安全依賴**](./sections/security/dependencysecurity.md)

<br/><br/>

## ![✔] 6.8. 避免使用Node.js的crypto庫處理密碼，使用Bcrypt

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** 密碼或機密資訊(API金鑰)應該使用安全的雜湊+salt函數(如 "bcrypt")來儲存, 因為效能和安全原因, 這應該是其JavaScript實現的首選。

**否則:** 在不使用安全功能的情況下，儲存的密碼或祕密資訊容易受到暴力破解和字典攻擊, 最終會導致他們的洩露。

🔗 [**更多: 使用Bcrypt**](./sections/security/bcryptpasswords.chinese.md)

<br/><br/>

## ![✔] 6.9. 轉義 HTML、JS 和 CSS 輸出

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a>

**TL;DR:** 傳送給瀏覽器的不受信任資料可能會被執行, 而不是顯示, 這通常被稱為跨站點指令碼(XSS)攻擊。使用專用庫將資料顯式標記為不應執行的純文字內容(例如:編碼、轉義)，可以減輕這種問題。

**否則:** 攻擊者可能會將惡意的JavaScript程式碼儲存在您的DB中, 然後將其傳送給可憐的客戶端。

🔗 [**更多: 轉義輸出**](./sections/security/escape-output.md)

<br/><br/>

## ![✔] 6.10. 驗證傳入的JSON schemas

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7: XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a>

**TL;DR:** 驗證傳入請求的body payload，並確保其符合預期要求, 如果沒有, 則快速報錯。為了避免每個路由中繁瑣的驗證編碼, 您可以使用基於JSON的輕量級驗證架構，比如[jsonschema](https://www.npmjs.com/package/jsonschema) or [joi](https://www.npmjs.com/package/joi)

**否則:** 您疏忽和寬鬆的方法大大增加了攻擊面, 並鼓勵攻擊者嘗試許多輸入, 直到他們找到一些組合, 使應用程式崩潰。

🔗 [**更多: 驗證傳入的JSON schemas**](./sections/security/validation.md)

<br/><br/>

## ![✔] 6.11. 支援黑名單的JWT

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** 當使用JSON Web Tokens(例如, 通過[Passport.js](https://github.com/jaredhanson/passport)), 預設情況下, 沒有任何機制可以從發出的令牌中撤消訪問許可權。一旦發現了一些惡意使用者活動, 只要它們持有有效的標記, 就無法阻止他們訪問系統。通過實現一個不受信任令牌的黑名單，並在每個請求上驗證，來減輕此問題。

**否則:** 過期或錯誤的令牌可能被第三方惡意使用，以訪問應用程式，並模擬令牌的所有者。

🔗 [**更多: 為JSON Web Token新增黑名單**](./sections/security/expirejwt.md)

<br/><br/>

## ![✔] 6.12. 限制每個使用者允許的登入請求

<a href="https://www.owasp.org/index.php/Top_10-2017_A2-Broken_Authentication" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A9:Broken%20Authentication%20-green.svg" alt=""/></a>

**TL;DR:** 一類保護暴力破解的中介軟體，比如[express-brute](https://www.npmjs.com/package/express-brute)，應該被用在express的應用中，來防止暴力/字典攻擊；這類攻擊主要應用於一些敏感路由，比如/admin 或者 /login，基於某些請求屬性, 如使用者名稱, 或其他標識符, 如正文參數等。

**否則:** 攻擊者可以發出無限制的密碼匹配嘗試, 以獲取對應用程式中特權帳戶的訪問許可權。

🔗 [**更多: 限制登入頻率**](./sections/security/login-rate-limit.md)

<br/><br/>

## ![✔] 6.13. 使用非root使用者執行Node.js

<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A5:Broken%20Access%20Access%20Control-green.svg" alt=""/></a>

**TL;DR:** Node.js作為一個具有無限許可權的root使用者執行，這是一種普遍的情景。例如，在Docker容器中，這是預設行為。建議建立一個非root使用者，並儲存到Docker映象中（下面給出了示例），或者通過呼叫帶有"-u username" 的容器來代表此使用者執行該程序。

**否則:** 在伺服器上執行指令碼的攻擊者在本地計算機上獲得無限制的權利 (例如，改變iptable，引流到他的伺服器上)

🔗 [**更多: 使用非root使用者執行Node.js**](./sections/security/non-root-user.md)

<br/><br/>

## ![✔] 6.14. 使用反向代理或中介軟體限制負載大小

<a href="https://www.owasp.org/index.php/Top_10-2017_A8-Insecure_Deserialization" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A8:Insecured%20Deserialization%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** 請求body有效載荷越大, Node.js的單執行緒就越難處理它。這是攻擊者在沒有大量請求(DOS/DDOS 攻擊)的情況下，就可以讓伺服器跪下的機會。在邊緣上（例如，防火牆，ELB）限制傳入請求的body大小，或者通過配置[express body parser](https://github.com/expressjs/body-parser)僅接收小的載荷，可以減輕這種問題。

**否則:** 您的應用程式將不得不處理大的請求, 無法處理它必須完成的其他重要工作, 從而導致對DOS攻擊的效能影響和脆弱性。

🔗 [**更多: 限制負載大小**](./sections/security/requestpayloadsizelimit.md)

<br/><br/>

## ![✔] 6.15. 避免JavaScript的eval聲明

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** `eval` 是邪惡的, 因為它允許在執行時執行自定義的JavaScript程式碼。這不僅是一個效能方面的問題, 而且也是一個重要的安全問題, 因為惡意的JavaScript程式碼可能來源於使用者輸入。應該避免的另一種語言功能是 `new Function` 建構函式。`setTimeout` 和 `setInterval` 也不應該傳入動態JavaScript程式碼。

**否則:** 惡意JavaScript程式碼查詢傳入 `eval` 或其他實時判斷的JavaScript函數的文字的方法, 並將獲得在該頁面上javascript許可權的完全訪問權。此漏洞通常表現為XSS攻擊。

🔗 [**更多: 避免JavaScript的eval聲明**](./sections/security/avoideval.chinese.md)

<br/><br/>

## ![✔] 6.16. 防止惡意RegEx讓Node.js的單執行緒過載執行

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** 正規表示式，在方便的同時，對JavaScript應用構成了真正的威脅，特別是Node.js平臺。匹配文字的使用者輸入需要大量的CPU週期來處理。在某種程度上，正則處理是效率低下的，比如驗證10個單詞的單個請求可能阻止整個event loop長達6秒，並讓CPU引火燒身。由於這個原因，偏向第三方的驗證包，比如[validator.js](https://github.com/chriso/validator.js)，而不是採用正則，或者使用[safe-regex](https://github.com/substack/safe-regex)來檢測有問題的正規表示式。

**否則:** 寫得不好的正規表示式可能容易受到正規表示式DoS攻擊的影響, 這將完全阻止event loop。例如，流行的`moment`包在2017年的11月，被發現使用了錯誤的RegEx用法而易受攻擊。

🔗 [**更多: 防止惡意正則**](./sections/security/regex.md)

<br/><br/>

## ![✔] 6.17. 使用變數避免模組載入

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** 避免通過作為參數的路徑requiring/importing另一個檔案, 原因是它可能源自使用者輸入。此規則可擴充套件為訪問一般檔案(即:`fs.readFile()`)或使用來自使用者輸入的動態變數訪問其他敏感資源。[Eslint-plugin-security](https://www.npmjs.com/package/eslint-plugin-security) linter可以捕捉這樣的模式, 並儘早提前警告。

**否則:** 惡意使用者輸入可以找到用於獲得篡改檔案的參數, 例如, 檔案系統上以前上載的檔案, 或訪問已有的系統檔案。

🔗 [**更多: 安全地載入模組**](./sections/security/safemoduleloading.chinese.md)

<br/><br/>

## ![✔] 6.18. 在沙箱中執行不安全程式碼

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** 當任務執行在執行時給出的外部程式碼時(例如, 外掛), 使用任何類型的`沙盒`執行環境保護主程式碼，並隔離開主程式碼和外掛。這可以通過一個專用的過程來實現 (例如:cluster.fork()), 無伺服器環境或充當沙盒的專用npm包。

**否則:** 外掛可以通過無限迴圈、記憶體超載和對敏感程序環境變數的訪問等多種選項進行攻擊

🔗 [**更多: 在沙箱中執行不安全程式碼**](./sections/security/sandbox.chinese.md)

<br/><br/>

## ![✔] 6.19. 使用子程序時要格外小心

<a href="https://www.owasp.org/index.php/Top_10-2017_A7-Cross-Site_Scripting_(XSS)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A7:XSS%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a> <a href="https://www.owasp.org/index.php/Top_10-2017_A4-XML_External_Entities_(XXE)" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A4:External%20Entities%20-green.svg" alt=""/></a>

**TL;DR:** 儘可能地避免使用子程序，如果您仍然必須這樣做，驗證和清理輸入以減輕shell注入攻擊。更喜歡使用 "child_process"。execFile 的定義將只執行具有一組屬性的單個命令, 並且不允許 shell 參數擴充套件。傾向於使用`child_process.execFile`，從定義上來說，它將僅僅執行具有一組屬性的單個命令，並且不允許shell參數擴充套件。

**否則:** 由於將惡意使用者輸入傳遞給未脫敏處理的系統命令, 直接地使用子程序可能導致遠端命令執行或shell注入攻擊。

🔗 [**更多: 處理子程序時要格外小心**](./sections/security/childprocesses.chinese.md)

<br/><br/>

## ![✔] 6.20. 隱藏客戶端的錯誤詳細資訊

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** 預設情況下, 整合的express錯誤處理程式隱藏錯誤詳細資訊。但是, 極有可能, 您實現自己的錯誤處理邏輯與自定義錯誤物件(被許多人認為是最佳做法)。如果這樣做, 請確保不將整個Error物件返回到客戶端, 這可能包含一些敏感的應用程式詳細資訊。

**否則:** 敏感應用程式詳細資訊(如伺服器檔案路徑、使用中的第三方模組和可能被攻擊者利用的應用程式的其他內部工作流)可能會從stack trace發現的資訊中洩露。

🔗 [**更多: 隱藏客戶端的錯誤詳細資訊**](./sections/security/hideerrors.md)

<br/><br/>

## ![✔] 6.21. 對npm或Yarn，配置2FA

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** 開發鏈中的任何步驟都應使用MFA(多重身份驗證)進行保護, npm/Yarn對於那些能夠掌握某些開發人員密碼的攻擊者來說是一個很好的機會。使用開發人員憑據, 攻擊者可以向跨項目和服務廣泛安裝的庫中注入惡意程式碼。甚至可能在網路上公開發布。在npm中啟用2因素身份驗證（2-factor-authentication）, 攻擊者幾乎沒有機會改變您的軟體包程式碼。

**否則:** [Have you heard about the eslint developer who's password was hijacked?](https://medium.com/@oprearocks/eslint-backdoor-what-it-is-and-how-to-fix-the-issue-221f58f1a8c8)

<br/><br/>

## ![✔] 6.22. 修改session中介軟體設定

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** 每個web框架和技術都有其已知的弱點-告訴攻擊者我們使用的web框架對他們來說是很大的幫助。使用session中介軟體的預設設定, 可以以類似於`X-Powered-By`header的方式向模組和框架特定的劫持攻擊公開您的應用。嘗試隱藏識別和揭露技術棧的任何內容(例如:Nonde.js, express)。

**否則:** 可以通過不安全的連線傳送cookie, 攻擊者可能會使用會話標識來標識web應用程式的基礎框架以及特定於模組的漏洞。

🔗 [**更多: cookie和session安全**](./sections/security/sessions.md)

<br/><br/>

## ![✔] 6.23. 通過顯式設定程序應崩潰的情況，以避免DOS攻擊

<a href="https://www.owasp.org/index.php/Denial_of_Service" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20DDOS%20-green.svg" alt=""/></a>

**TL;DR:** 當錯誤未被處理時, Node程序將崩潰。即使錯誤被捕獲並得到處理，許多最佳實踐甚至建議退出。例如, Express會在任何非同步錯誤上崩潰 - 除非使用catch子句包裝路由。這將開啟一個非常愜意的攻擊點, 攻擊者識別哪些輸入會導致程序崩潰並重復傳送相同的請求。沒有即時補救辦法, 但一些技術可以減輕苦楚: 每當程序因未處理的錯誤而崩潰，都會發出警報，驗證輸入並避免由於使用者輸入無效而導致程序崩潰，並使用catch將所有路由處理包裝起來，並在請求中出現錯誤時, 考慮不要崩潰(與全局發生的情況相反)。

**否則:** 這只是一個起到教育意義的假設: 給定許多Node.js應用程式, 如果我們嘗試傳遞一個空的JSON正文到所有POST請求 - 少數應用程式將崩潰。在這一點上, 我們可以只是重複傳送相同的請求, 就可以輕鬆地搞垮應用程式。

<br/><br/><br/>

## ![✔] 6.24. 避免不安全的重定向

<a href="https://www.owasp.org/index.php/Top_10-2017_A1-Injection" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A1:Injection%20-green.svg" alt=""/></a>

**TL;DR:** 不驗證使用者輸入的重定向可使攻擊者啟動網路釣魚詐騙，竊取使用者憑據，以及執行其他惡意操作。

**否則:** 當攻擊者發現你沒有校驗使用者提供的外部輸入時，他們會在論壇、社交媒體以和其他公共場合釋出他們精心製作的連結來誘使使用者點選，以此達到漏洞利用的目的。

🔗 [**閱讀更多: 避免不安全的重定向**](./sections/security/saferedirects.chinese.md)

<br/><br/><br/>

## ![✔] 6.25. 避免將機密資訊釋出到NPM倉庫

<a href="https://www.owasp.org/index.php/Top_10-2017_A6-Security_Misconfiguration" target="_blank"><img src="https://img.shields.io/badge/%E2%9C%94%20OWASP%20Threats%20-%20A6:Security%20Misconfiguration%20-green.svg" alt=""/></a>

**TL;DR:** 您應該採取預防措施來避免偶然地將機密資訊釋出到npm倉庫的風險。 一個 `.npmignore` 檔案可以被用作忽略掉特定的檔案或目錄, 或者一個在 `package.json` 中的 `files` 陣列可以起到一個白名單的作用.

**否則:** 您項目的API金鑰、密碼或者其它機密資訊很容易被任何碰到的人濫用，這可能會導致經濟損失、身份冒充以及其它風險。

🔗 [**閱讀更多: 避免釋出機密資訊**](./sections/security/avoid_publishing_secrets.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `7. 草稿: 有關效能的最佳實踐`

## 我們的貢獻者們正在努力完善這個章節。 [你想要加入嗎?](https://github.com/goldbergyoni/nodebestpractices/issues/256)

<br/><br/>

## ![✔] 7.1. 不要阻塞事件迴圈

**TL;DR:** 避免執行CPU密集型的任務，並將這些任務轉移到基於上下文的專用執行緒中，因為它們會阻塞大多數單執行緒事件迴圈。

**否則:** 由於事件迴圈被阻塞了，Node.js 將無法處理其它請求，從而導致同時請求的使用者的延遲。 **3000 位使用者正在等待響應，內容本身已經準備好了提供服務， 但是一個單獨的請求阻止了伺服器將結果分發回去。**

🔗 [**閱讀更多: 不要阻塞事件迴圈**](./sections/performance/block-loop.md)

<br /><br /><br />

## ![✔] 7.2. 優先使用原生的JS方法，而不是像 Lodash 這樣的使用者空間級別的實用工具

**TL;DR:** 使用像 `lodash` 和 `underscore` 這樣的實用庫替代原生的JS方法，通常來說這麼做更不好，因為它導致了一些不必要的依賴項以及更差的效能表現。
請記住，隨著新的V8引擎以及新的ES標準的引入，原生方法得到了改進，它們現在會比這些實用工具庫高出大概 50% 的效能。

**否則:** 你將不得不維護一些效能更低的項目，在這些項目中，你本可以很簡單的使用那些已經可以用的東西，或者用幾行程式碼來取代掉幾個檔案。

🔗 [**閱讀更多: 原生方法勝過實用工具**](./sections/performance/nativeoverutil.md)

<br/><br/><br/>

<p align="right"><a href="#table-of-contents">⬆ Return to top</a></p>

# `API Practices`

## Our contributors are working on this section. Would you like to join?

<br/><br/><br/>

# Milestones
To maintain this guide and keep it up to date, we are constantly updating and improving the guidelines and best practices with the help of the community. You can follow our [milestones](https://github.com/goldbergyoni/nodebestpractices/milestones) and join the working groups if you want to contribute to this project.

<br/><br/>

# Contributors
## `Yoni Goldberg`
Independent Node.js consultant who works with customers at USA, Europe and Israel on building large-scale scalable Node applications. Many of the best practices above were first published on his blog post at [http://www.goldbergyoni.com](http://www.goldbergyoni.com). Reach Yoni at @goldbergyoni or me@goldbergyoni.com

## `Ido Richter`
👨‍💻 Software engineer, 🌐 web developer, 🤖 emojis enthusiast.

## `Refael Ackermann` [@refack](https://github.com/refack) &lt;refack@gmail.com&gt; (he/him)
Node.js Core Collaborator, been noding since 0.4, and have noded in multiple production sites. Founded `node4good` home of [`lodash-contrib`](https://github.com/node4good/lodash-contrib), [`formage`](https://github.com/node4good/formage), and [`asynctrace`](https://github.com/node4good/asynctrace).
`refack` on freenode, Twitter, GitHub, GMail, and many other platforms. DMs are open, happy to help.

## `Bruno Scheufler`
💻 full-stack web developer and Node.js enthusiast.

## `Kyle Martin` [@js-kyle](https://github.com/js-kyle)
Full Stack Developer based in New Zealand, interested in architecting and building Node.js applications to perform at global scale. Keen contributor to open source software, including Node.js Core.


<br/><br/>

## Thank You Notes

We appreciate any contribution, from a single word fix to a new best practice. View our contributors and [contributing documentation here!](./README.md#contributors-)

<br/><br/><br/>

