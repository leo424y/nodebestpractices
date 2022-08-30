# 使用智慧日誌使你的應用程式變得清晰

<br/><br/>


### 一段解釋

無論如何，您要列印日誌，而且需要一些可以在其中跟蹤錯誤和核心指標的介面來展示生產環境資訊（例如，每小時發生了多少錯誤，最慢的API節點是哪一個）為什麼不在健壯的日誌框架中進行一些適度的嘗試呢? 要實現這一目標，需要在三個步驟上做出深思熟慮的決定:

**1. 智慧日誌** – 在最基本的情況下，您需要使用像[Winston](https://github.com/winstonjs/winston), [Bunyan](https://github.com/trentm/node-bunyan)這樣有信譽的日誌庫，在每個事務開始和結束時輸出有意義的資訊。還可以考慮將日誌語句格式化為JSON，並提供所有上下文屬性（如使用者id、操作類型等）。這樣運維團隊就可以在這些欄位上操作。在每個日誌行中包含一個唯一的transaction ID，更多的資訊查閱條款 “Write transaction-id to log”。最後要考慮的一點還包括一個代理，它記錄系統資源，如記憶體和CPU，比如Elastic Beat。

**2. 智慧聚合** – 一旦您在伺服器檔案系統中有了全面的資訊，就應該定期將這些資訊推送到一個可以聚合、處理和視覺化資料的系統中。例如，Elastic stack是一種流行的、自由的選擇，它提供所有元件去聚合和產生視覺化資料。許多商業產品提供了類似的功能，只是它們大大減少了安裝時間，不需要主機託管。

**3. 智慧視覺化** – 現在的資訊是聚合和可搜尋的, 一個可以滿足僅僅方便地搜尋日誌的能力, 可以走得更遠, 沒有編碼或花費太多的努力。我們現在可以顯示一些重要的操作指標, 如錯誤率、平均一天CPU使用, 在過去一小時內有多少新使用者選擇, 以及任何其他有助於管理和改進我們應用程式的指標。

<br/><br/>


### 視覺化示例: Kibana(Elastic stack的一部分)促進了對日誌內容的高階搜尋
![Kibana facilitates advanced searching on log content](../../assets/images/smartlogging1.png "Kibana facilitates advanced searching on log content")

<br/><br/>

### 視覺化示例: Kibana(Elastic stack的一部分)基於日誌來視覺化資料
![Kibana visualizes data based on logs](../../assets/images/smartlogging2.jpg "Kibana visualizes data based on logs")

<br/><br/>

### 部落格應用: Logger的需求
摘自部落格 [Strong Loop](https://strongloop.com/strongblog/compare-node-js-logging-winston-bunyan/):

> 讓我們識別一些需求（對於一個日誌記錄器來說）：
> 1. 每條日誌對應一個時間戳。這個很好解釋 – 您應該能夠判斷每個日誌條目出現的時間。
> 2. 日誌格式應該很容易被人和機器消化理解。
> 3. 允許多個可配置的目標流。例如，您可能在一個檔案中寫入trace日誌，但是當遇到錯誤時，將其寫入相同的檔案，然後寫入錯誤日誌檔案並同時傳送電子郵件...
 <br/><br/>
 

 
<br/><br/>
