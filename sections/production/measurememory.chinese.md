# 測量和防範記憶體使用情況

<br/><br/>


### 一段解釋

在一個完美的開發過程中, Web開發人員不應該處理記憶體洩漏問題。 實際上，記憶體問題是一個必須瞭解的Node已知的問題。首先，記憶體使用必須不斷監視.在開發和小型生產站點上，您可以使用Linux命令或NPM工具和庫（如node-inspector和memwatch）來手動測量。 這個人工操作的主要缺點是它們需要一個人進行積極的監控 - 對於正規的生產站點來說，使用魯棒性監控工具是非常重要的，例如（AWS CloudWatch，DataDog或任何類似的主動系統），當洩漏發生時提醒。 防止洩漏的開發指南也很少：避免將資料儲存在全局級別，使用動態大小的流資料，使用let和const限制變數範圍。

<br/><br/>

### 其他部落格說了什麼

* 摘自部落格 [Dyntrace](https://www.dynatrace.com/news/blog/understanding-garbage-collection-and-hunting-memory-leaks-in-node-js/):
> ... ”正如我們所瞭解到的，在Node.js 中，JavaScript被V8編譯為機器碼。由此產生的機器碼資料結構與原始表達沒有多大關係，只能由V8管理. 這意味著我們不能主動分配或釋放JavaScript中的記憶體. V8 使用了一個眾所周知的垃圾收集機制來解決這個問題.”

* 摘自部落格 [Dyntrace](http://blog.argteam.com/coding/hardening-node-js-for-production-part-2-using-nginx-to-avoid-node-js-load):
> ... “雖然這個例子導致了明顯的結果，但這個過程總是一樣的：用一些時間和相當數量的記憶體分配建立heap dumps，比較dumps，以找出正在增長的記憶體洩露。”

* 摘自部落格 [Rising Stack](https://blog.risingstack.com/finding-a-memory-leak-in-node-js/):
> ... “故障, 在記憶體較少的系統上執行時必須限制記憶體，Node.js會嘗試使用大約1.5GB的記憶體。這是預期的行為，垃圾收集是一個代價很高的操作。
解決方案是為Node.js程序新增一個額外的參數:
node –max_old_space_size=400 server.js –production ”
“為什麼垃圾收集代價很高? V8 JavaScript 使用了 stop-the-world (STW)的垃圾回收機制。 事實上，這意味著程式在進行垃圾回收時停止執行。”